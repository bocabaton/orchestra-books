# GitLab

## Installation

~~~bash
yum install -y curl
curl https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
yum install gitlab-ce
~~~

## GitLab Configuration

~~~bash
gitlab-ctl reconfigure
~~~
