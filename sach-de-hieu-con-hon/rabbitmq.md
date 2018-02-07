\# Overview

A RabbitMQ broker is a logical grouping of one or several Erlang nodes, each running the RabbitMQ application and sharing users, virtual hosts, queues, exchanges, bindings, and runtime parameters. Sometimes we refer to the collection of nodes as a cluster.



\# What is Replicated

All data/state required for the operation of a RabbitMQ broker is replicated across all nodes. An exception to this are message queues, which by default reside on one node, though they are visible and reachable from all nodes.



\# Hostname Resolution Requirements

- RabbitMQ nodes address each other using domain names, either short or fully-qualified \(FQDNs\). Therefore hostnames of all cluster members must be resolvable from all cluster nodes, as well as machines on which command line tools such as rabbitmqctl might be used.

- Hostname resolution can use any of the standard OS-provided methods:

	+ DNS records

	+ Local host files \(e.g. /etc/hosts\)



\# Cluster Formation

Cluster can formed in a number of ways:

	+ Manually with rabbitmqctl \(e.g. in development environments\)

	+ Declaratively by listing cluster nodes in config file

	+ Declaratively with rabbitmq-autocluster \(a plugin\)	



\# Failure Handling

- RabbitMQ brokers tolerate the failure of individual nodes. Nodes can be started and stopped at will, as long as they can contact a cluster member node known at the time of shutdown.

- RabbitMQ clustering has several modes of dealing with network partitions, primarily consistency oriented. 

	+ pause-minority: RabbitMQ will automatically pause cluster nodes which determine themselves to be in a minority \(i.e. fewer or equal than half the total number of nodes\) after seeing other nodes go down. It therefore chooses partition tolerance over availability from the CAP theorem. This ensures that in the event of a network partition, at most the nodes in a single partition will continue to run. The minority nodes will pause as soon as a partition starts, and will start again when the partition ends.

	+ pause-if-all-down: RabbitMQ will automatically pause cluster nodes which cannot reach any of the listed nodes. In other words, all the listed nodes must be down for RabbitMQ to pause a cluster node. This is close to the pause-minority mode, however, it allows an administrator to decide which nodes to prefer, instead of relying on the context. For instance, if the cluster is made of two nodes in rack A and two nodes in rack B, and the link between racks is lost, pause-minority mode will pause all nodes. In pause-if-all-down mode, if the administrator listed the two nodes in rack A, only nodes in rack B will pause. Note that it is possible the listed nodes get split across both sides of a partition: in this situation, no node will pause. That is why there is an additional ignore/autoheal argument to indicate how to recover from the partition

	+ autoheal: RabbitMQ will automatically decide on a winning partition if a partition is deemed to have occurred, and will restart all nodes that are not in the winning partition. Unlike pause\_minority mode it therefore takes effect when a partition ends, rather than when one starts. The winning partition is the one which has the most clients connected \(or if this produces a draw, the one with the most nodes; and if that still produces a draw then one of the partitions is chosen in an unspecified way\)

	+ ignore \(default\)	



\# Which mode should I pick

- allowing RabbitMQ to deal with network partitions automatically does not make them less of a problem

- might wish to pick a recovery mode as follows

	+ ignore: Your network really is reliable. All your nodes are in a rack, connected with a switch, and that switch is also the route to the outside world. You don't want to run any risk of any of your cluster shutting down if any other part of it fails \(or you have a two node cluster\)

	+ pause\_minority: Your network is maybe less reliable. You have clustered across 3 AZs in EC2, and you assume that only one AZ will fail at once. In that scenario you want the remaining two AZs to continue working and the nodes from the failed AZ to rejoin automatically and without fuss when the AZ comes back

	+ autoheal: Your network may not be reliable. You are more concerned with continuity of service than with data integrity. You may have a two node cluster.



\# More about pause-minority mode

- Erlang VM on the paused nodes will continue running but the nodes will not listen on any ports or do any other work. They will check once per second to see if the rest of the cluster has reappeared, and start up again if it has

- Note that nodes will not enter the paused state at startup, even if they are in a minority then. It is expected that any such minority at startup is due to the rest of the cluster not having been started yet.

- Also note that RabbitMQ will pause nodes which are not in a strict majority of the cluster - i.e. containing more than half of all nodes. It is therefore not a good idea to enable pause-minority mode on a cluster of two nodes since in the event of any network partition or node failure, both nodes will pause. However, pause\_minority mode is likely to be safer than ignore mode for clusters of more than two nodes, especially if the most likely form of network partition is that a single minority of nodes drops off the network.	



\# Disk and RAM Nodes

In most cases you want all your nodes to be disk nodes; RAM nodes are a special case that can be used to improve the performance clusters with high queue, exchange, or binding churn



\# Clustering Transcript

\#\# How Nodes \(and CLI tools\) Authenticate to Each Other: the Erlang Cookie

- RabbitMQ nodes and CLI tools \(e.g. rabbitmqctl\) use a cookie to determine whether they are allowed to communicate with each other. For two nodes to be able to communicate they must have the same shared secret called the Erlang cookie. The cookie is just a string of alphanumeric characters. It can be as long or short as you like. Every cluster node must have the same cookie.

- Erlang VM will automatically create a random cookie file when the RabbitMQ server starts up. The easiest way to proceed is to allow one node to create the file, and then copy it to all the other nodes in the cluster.

- On Unix systems, the cookie will be typically located in /var/lib/rabbitmq/.erlang.cookie or $HOME/.erlang.cookie.

- As an alternative, you can insert the option "-setcookie cookie" in the erl call in the  rabbitmq-server and rabbitmqctl scripts.

- When the cookie is misconfigured \(for example, not identical\), RabbitMQ will log errors such as "Connection attempt from disallowed node" and "Could not auto-cluster".

\#\# Restarting cluster nodes

- Nodes that have been joined to a cluster can be stopped at any time. It is also ok for them to crash. In both cases the rest of the cluster continues operating unaffected, and the nodes automatically "catch up" with the other cluster nodes when they start up again.

- important caveats:

	+ When the entire cluster is brought down, the last node to go down must be the first node to be brought online. If this doesn't happen, the nodes will wait 30 seconds for the last disc node to come back online, and fail afterwards. If the last node to go offline cannot be brought back up, it can be removed from the cluster using the forget\_cluster\_node command - consult the rabbitmqctl manpage for more information.

\#\# Breaking up a cluster

$rabbitmqctl stop\_app

$rabbitmqctl reset

$rabbitmqctl start\_app

\#\# Hostname Changes

- RabbitMQ nodes use hostnames to communicate with each other. Therefore, all node names must be able to resolve names of all cluster peers. This is also true for tools such as rabbitmqctl.

- In addition to that, by default RabbitMQ names the database directory using the current hostname of the system. If the hostname changes, a new empty database is created. To avoid data loss it's crucial to set up a fixed and resolvable hostname. Whenever the hostname changes you should restart RabbitMQ:

$/etc/init.d/rabbitmq-server restart	

\#\# Erlang Versions Across the Cluster

All nodes in a cluster must run the same minor version of Erlang: 19.3.4 and 19.3.6 can be mixed but 19.0.1 and 19.3.6 \(or 17.5 and 19.3.6\) cannot. Compatibility between individual Erlang/OTP patch versions can vary between releases but that's generally rare.

\#\# Connecting to Clusters from Clients

- Recommend:

	+ this could be a dynamic DNS service which has a very short TTL configuration

	+ or a plain TCP load balancer

	+ or some sort of mobile IP achieved with pacemaker 

	+ or similar technologies

\#\# Clusters with RAM nodes

- RAM nodes keep their metadata only in memory. As RAM nodes don't have to write to disc as much as disc nodes, they can perform better. However, note that since persistent queue data is always stored on disc, the performance improvements will affect only resource management \(e.g. adding/removing queues, exchanges, or vhosts\), but not publishing or consuming speed.

- RAM nodes are an advanced use case; when setting up your first cluster you should simply not use them. You should have enough disc nodes to handle your redundancy requirements, then if necessary add additional RAM nodes for scale.

- A cluster containing only RAM nodes is fragile; if the cluster stops you will not be able to start it again and will lose all data. RabbitMQ will prevent the creation of a RAM-node-only cluster in many situations, but it can't absolutely prevent it.



\# A Cluster on a Single Machine

- make sure the nodes have distinct:

	+ node names: RABBITMQ\_NODENAME

	+ data store locations: RABBITMQ\_MNESIA\_DIR

	+ log file locations: RABBITMQ\_LOG\_BASE, RABBITMQ\_CONFIG\_FILE

	+ bind to different ports, including those used by plugins: RABBITMQ\_NODE\_PORT, RABBITMQ\_DIST\_PORT

- start nodes

RABBITMQ\_NODE\_PORT=5671 RABBITMQ\_DIST\_PORT=25671 RABBITMQ\_NODENAME=rabbit RABBITMQ\_CONFIG\_FILE=/u01/applications/rabbitmq/instance1/rabbitmq RABBITMQ\_MNESIA\_DIR=/u01/applications/rabbitmq/instance1/data RABBITMQ\_LOG\_BASE=/u01/applications/rabbitmq/instance1/log rabbitmq-server -detached

RABBITMQ\_NODE\_PORT=5672 RABBITMQ\_DIST\_PORT=25672 RABBITMQ\_NODENAME=rabbit2 RABBITMQ\_CONFIG\_FILE=/u01/applications/rabbitmq/instance2/rabbitmq RABBITMQ\_MNESIA\_DIR=/u01/applications/rabbitmq/instance2/data RABBITMQ\_LOG\_BASE=/u01/applications/rabbitmq/instance2/log rabbitmq-server -detached

RABBITMQ\_NODE\_PORT=5673 RABBITMQ\_DIST\_PORT=25673 RABBITMQ\_NODENAME=rabbit3 RABBITMQ\_CONFIG\_FILE=/u01/applications/rabbitmq/instance3/rabbitmq RABBITMQ\_MNESIA\_DIR=/u01/applications/rabbitmq/instance3/data RABBITMQ\_LOG\_BASE=/u01/applications/rabbitmq/instance3/log rabbitmq-server -detached



- join cluster

rabbitmqctl -n rabbit2 stop\_app

rabbitmqctl -n rabbit2 join\_cluster rabbit@\`hostname -s\`

rabbitmqctl -n rabbit2 start\_app

rabbitmqctl -n rabbit3 stop\_app

rabbitmqctl -n rabbit3 join\_cluster rabbit@\`hostname -s\`

rabbitmqctl -n rabbit3 start\_app



- set policy

rabbitmqctl -n rabbit set\_policy ha-two "^queue\."    '{"ha-mode":"exactly","ha-params":2,"ha-sync-mode":"automatic"}'



\# Standard ports

4369: epmd, a peer discovery service used by RabbitMQ nodes and CLI tools

5672, 5671: used by AMQP 0-9-1 and 1.0 clients without and with TLS

25672: used by Erlang distribution for inter-node and CLI tools communication and is allocated from a dynamic range \(limited to a single port by default, computed as AMQP port + 20000\). See networking guide for details.

15672: HTTP API clients and rabbitmqadmin \(only if the management plugin is enabled\)

61613, 61614: STOMP clients without and with TLS \(only if the STOMP plugin is enabled\)

1883, 8883: \(MQTT clients without and with TLS, if the MQTT plugin is enabled

15674: STOMP-over-WebSockets clients \(only if the Web STOMP plugin is enabled\)

15675: MQTT-over-WebSockets clients \(only if the Web MQTT plugin is enabled\)



\# Highly Available \(Mirrored\) Queues

- By default, queues within a RabbitMQ cluster are located on a single node \(the node on which they were first declared\). This is in contrast to exchanges and bindings, which can always be considered to be on all nodes. Queues can optionally be made mirrored across multiple nodes. Each mirrored queue consists of one master and one or more mirrors, with the oldest mirror being promoted to the new master if the old master disappears for any reason.

- Messages published to the queue are replicated to all mirrors. Consumers are connected to the master regardless of which node they connect to, with mirrors dropping messages that have been acknowledged at the master. Queue mirroring therefore enhances availability, but does not distribute load across nodes \(all participating nodes each do all the work\).

- This solution requires a RabbitMQ cluster, which means that it will not cope seamlessly with network partitions within the cluster and, for that reason, is not recommended for use across a WAN \(though of course, clients can still connect from as near and as far as needed\).

\#\# How Mirroring is Configured

- In addition to mandatory properties \(e.g. durable or exclusive\), queues in RabbitMQ have optional parameters \(arguments\), sometimes referred to as x-arguments. Those are provided by clients when they declare queues and control various optional queue features, such as mirroring or TTL. The x-arguments can be used to configure mirroring parameters but there's a better way.

- A more flexible, unobtrisuve, and manageable way of configuring x-arguments is via policies. A policy matches one or more queues by name \(using a regular expression pattern\) and appends its definition \(a map of optional arguments\) to the x-arguments of the matching queues. In other words, it is possible to configure x-arguments for multiple queues at once with a policy, and update them all at once by updating policy definition. This is the most common and recommended way of configuring mirroring in modern RabbitMQ versions.

\#\# Queue Arguments that Control Mirroring

To cause queues to become mirrored, you should create a policy which matches them and sets policy keys ha-mode and \(optionally\) ha-params. The following table explains the options for these keys:

\| ha-mode 	\| ha-params				Result  	                                                        

\|---------- \|-----------------  \|---------------------------------------------------

\| all		\| \(absent\)			\| Queue is mirrored across all nodes in the cluster. When a new node is added to the cluster, the queue will be 		

\|			\|					\| mirrored to that node.

\| exactly	\| count				\| Queue is mirrored to count nodes in the cluster. If there are less than count nodes in the cluster, the queue is 

\|			\|					\| mirrored to all nodes. If there are more than count nodes in the cluster, and a node containing a mirror goes down, 

\|			\|					\| then a new mirror will be created on another node

\| nodes		\| node names		\| Queue is mirrored to the nodes listed in node names. Node names are the Erlang node names as they appear in 

\|			\|					\| rabbitmqctl cluster\_status; they usually have the form "rabbit@hostname". If any of those node names are not a part 

\|			\|					\| of the cluster, this does not constitute an error. If none of the nodes in the list are online at the time when the 

\|			\|					\| queue is declared then the queue will be created on the node that the declaring client is connected to

\#\# To How Many Nodes to Mirror?

Note that mirroring to all queues is the most conservative option and is unnecessary in many cases. For clusters of 3 and more nodes it is recommended to mirror to a quorum \(the majority\) of nodes, e.g. 2 nodes in a 3 node cluster or 3 nodes in a 5 node cluster. Since some data can be inherently transient or very time sensitive, it can be perfectly reasonable to use a lower number of mirrors for some queues \(or even not use any mirroring\).



\# Message durability

- Two things are required to make sure that messages aren't lost: we need to mark both the queue and messages as durable \(https://www.rabbitmq.com/tutorials/tutorial-two-python.html\)

	--&gt; declare queue as "durable"

	--&gt; mark messages as persistent - by supplying a delivery\_mode property with a value 2

- Marking messages as persistent doesn't fully guarantee that a message won't be lost. Although it tells RabbitMQ to save the message to disk, there is still a short time window when RabbitMQ has accepted a message and hasn't saved it yet. Also, RabbitMQ doesn't do  fsync\(2\) for every message -- it may be just saved to cache and not really written to the disk. The persistence guarantees aren't strong, but it's more than enough for our simple task queue. If you need a stronger guarantee then you can use publisher confirms	



\# Referrence

https://www.rabbitmq.com/clustering.html

http://www.rabbitmq.com/relocate.html

http://www.rabbitmq.com/configure.html





- doc defautl queue

- concurrent có giảm khi message giảm? số channel, socket khi chạy multi thread.

- review ack \(publish, consume\)

- proxy dùng nginx







cluster:

mirrored:



https://www.rabbitmq.com/ha.html

https://www.rabbitmq.com/partitions.html

https://www.rabbitmq.com/confirms.html

https://www.rabbitmq.com/tutorials/tutorial-two-python.html

https://www.rabbitmq.com/lazy-queues.html

https://www.rabbitmq.com/persistence-conf.html

https://www.rabbitmq.com/reliability.html































