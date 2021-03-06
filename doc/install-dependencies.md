<!--
#
# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
#
-->

# Install Dependencies
- [Install Dependencies](#install-dependencies)
- [Note](#note)
- [CentOS 7](#centos-7)
- [Fedora 31 & 32](#fedora-31--32)
- [Ubuntu 16.04 & 18.04](#ubuntu-1604--1804)
- [Debian 9 & 10](#debian-9--10)
- [Mac OSX](#mac-osx)

Note
====
- Apache APISIX currently only supports the v2 protocol storage to etcd, but the latest version of etcd (starting with 3.4) has turned off the v2 protocol by default.

You need to add `--enable-v2=true` to the startup parameter to enable the v2 protocol. The development of the v3 protocol supporting etcd has begun and will soon be available.

```shell
etcd --enable-v2=true &
```

- If you want use Tengine instead of OpenResty, please take a look at this installation step script [Install Tengine at Ubuntu](../.travis/linux_tengine_runner.sh).


CentOS 7
========

```shell
# install epel, `luarocks` need it.
wget http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
sudo rpm -ivh epel-release-latest-7.noarch.rpm

# add OpenResty source
sudo yum install yum-utils
sudo yum-config-manager --add-repo https://openresty.org/package/centos/openresty.repo

# install OpenResty, etcd and some compilation tools
sudo yum install -y etcd openresty curl git gcc luarocks lua-devel

# start etcd server
sudo service etcd start
```

Fedora 31 & 32
==============

```shell
# add OpenResty source
sudo yum install yum-utils
sudo yum-config-manager --add-repo https://openresty.org/package/fedora/openresty.repo

# install OpenResty, etcd and some compilation tools
sudo yum install -y etcd openresty curl git gcc luarocks lua-devel

# start etcd server
sudo etcd --enable-v2=true &
```

Ubuntu 16.04 & 18.04
====================

```shell
# add OpenResty source
wget -qO - https://openresty.org/package/pubkey.gpg | sudo apt-key add -
sudo apt-get update
sudo apt-get -y install software-properties-common
sudo add-apt-repository -y "deb http://openresty.org/package/ubuntu $(lsb_release -sc) main"
sudo apt-get update

# install OpenResty, etcd and some compilation tools
sudo apt-get install -y git etcd openresty curl luarocks

# start etcd server
sudo service etcd start
```

Debian 9 & 10
=============

```shell
# optional
sed -i 's|^deb http://deb.debian.org/debian|deb http://mirrors.huaweicloud.com/debian|g' /etc/apt/sources.list
sed -i 's|^deb http://security.debian.org/debian-security|deb http://mirrors.huaweicloud.com/debian-security|g' /etc/apt/sources.list
apt update
apt install wget gnupg -y

# add OpenResty source
wget -qO - https://openresty.org/package/pubkey.gpg | sudo apt-key add -
sudo apt-get -y install software-properties-common
sudo add-apt-repository -y "deb http://openresty.org/package/debian $(lsb_release -sc) openresty"
sudo apt-get update

# install etcd
wget https://github.com/etcd-io/etcd/releases/download/v3.3.13/etcd-v3.3.13-linux-amd64.tar.gz
tar -xvf etcd-v3.3.13-linux-amd64.tar.gz && \
    cd etcd-v3.3.13-linux-amd64 && \
    sudo cp -a etcd etcdctl /usr/bin/

# install OpenResty and some compilation tools
sudo apt-get install -y git openresty curl luarocks make

# start etcd server
nohup etcd &
```

Mac OSX
=======

```shell
# install OpenResty, etcd and some compilation tools
brew install openresty/brew/openresty etcd luarocks curl git

# start etcd server with v2 protocol
etcd --enable-v2=true &
```
