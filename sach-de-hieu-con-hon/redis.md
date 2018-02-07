http://redis4you.com/articles.php?id=010



\# Do BGSAVE

0  0 \* \* \*    redis-cli bgsave



\# Simple copy

15 0 \* \* \*    cp /data/redis/dump.rdb /backup/redis.dump.rdb



\# ... or call more complex script \(example is commented\)

\#15 0 \* \* \*    /scripts/perform\_copy.sh

