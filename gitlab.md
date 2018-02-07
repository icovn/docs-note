\# Install

https://about.gitlab.com/installation/\#ubuntu



\# Start all GitLab components

sudo gitlab-ctl start



\# Stop all GitLab components

sudo gitlab-ctl stop



\# Restart all GitLab components

sudo gitlab-ctl restart



\# Restart a component

sudo gitlab-ctl restart nginx



\# Invoking Rake tasks 

sudo gitlab-rake gitlab:check



\# Starting a Rails console session

sudo gitlab-rails console





\# Install runners \(https://docs.gitlab.com/runner/install/linux-repository.html\)

curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-runner/script.deb.sh \| sudo bash

sudo apt-get install gitlab-runner



\# Register runner \(https://docs.gitlab.com/runner/register/index.html\)

sudo gitlab-runner register





\# Change external URL \(https://docs.gitlab.com/omnibus/settings/configuration.html\)

/etc/gitlab/gitlab.rb



sudo gitlab-ctl reconfigure





\# CI config sample

https://gitlab.com/gitlab-org/gitlab-ci-yml/tree/master

https://gitlab.com/gitlab-org/gitlab-runner/blob/master/docs/configuration/advanced-configuration.md

https://docs.gitlab.com/ce/ci/docker/using\_docker\_build.html

https://docs.gitlab.com/ce/ci/docker/using\_docker\_images.html

https://about.gitlab.com/2016/12/14/continuous-delivery-of-a-spring-boot-application-with-gitlab-ci-and-kubernetes/



/u01/applications/gitlab-runner/maven:/root/.m2/:rw

