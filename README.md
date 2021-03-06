[![Build Status](https://travis-ci.org/serverfarmer/serverfarmer.png?branch=master)](https://travis-ci.org/serverfarmer/serverfarmer)

Feeling overwhelmed by complexity of Puppet, Chef, Ansible or other server configuration
tools?

Scared of complex and possibly buggy software, that might become an attack vector?

Manage many different servers for many different customers, where storing passwords etc.
in single repository for all customers is not an option?

Server Farmer is the right tool for you.


# Why Server Farmer?

Me (the original author) and my company manage many servers with high-risk confidential
data (mostly medical) and simply can't manage them via Puppet/Chef, at least via single
Puppet/Chef instance shared between customers. And having multiple instances of such
software is way too expensive. Server Farmer solves this problem by requiring only one
central server, shared between all your customers.

Also, Puppet is production ready since 2011, and Chef since 2013. Server Farmer was open
sourced in 2015, however it successfully managed production servers since 2008.


# Advantages over Puppet, Chef etc.

1. Featherweight.
2. No customer passwords, private keys etc. in repository. All customer-specific or
machine-specific configuration done by runtime dialogs, so base Server Farmer can be
easily deployed everywhere.
3. Diff friendly: all configuration templates based on default configuration files
from each Linux distribution, therefore it's easy to track changes.
4. Tight integration with Cacti monitoring software.
5. Powerful extensions to simplify and automate many admin tasks.


# Supported operating systems

Server Farmer was built around Debian/Ubuntu Linux. It currently supports:

- Debian-based distributions:
  - Debian 4.x (Etch)
  - Debian 5.x (Lenny)
  - Debian 6.x (Squeeze)
  - Debian 7.x (Wheezy)
  - **Debian 8.x (Jessie)** - current
  - Ubuntu 8.04 LTS (Hardy Heron)
  - Ubuntu 9.04 (Jaunty Jackalope)
  - Ubuntu 10.04 LTS (Lucid Lynx)
  - Ubuntu 10.10 (Maverick Meerkat)
  - Ubuntu 12.04 LTS (Precise Pangolin)
  - Ubuntu 13.10 (Saucy Salamander)
  - **Ubuntu 14.04 LTS (Trusty Tahr)**
  - Ubuntu 15.04 (Vivid Vervet)
- Red Hat-based distributions:
  - Oracle Linux 6.x - tested with Oracle Database 10g2, 11g, 11g2, 12c
  - Oracle Linux 7.x - tested with Oracle Database 11g2, 12c
  - Red Hat Enterprise Linux 5.x
  - Red Hat Enterprise Linux 6.x
  - **Red Hat Enterprise Linux 7.x** - current
  - CentOS 5.x
  - CentOS 6.x
  - CentOS 7.x - current
- specialized distributions:
  - cPanel / WHM 11.x on CentOS 6.x
  - Elastix 2.5 and 4.0, Elastix MT 3.0
  - openATTIC 1.x - either VM or installed on Debian Wheezy
  - Proxmox VE 3.x - either standalone or installed on Debian Wheezy
  - Zentyal Server 4.1 - both commercial and development editions


# Cloud support

*Is this possible to deploy Server Farmer in the cloud?*

Yes, Server Farmer fully supports Ubuntu 14.04 HVM on Amazon EC2 (and should work without
problems on any OS from the above list). You can even install it on new instances in fully
unattended mode. To do it, simply clone this repository and edit credentials/ssh keys:

```bash
git clone https://github.com/serverfarmer/cloudfarmer /opt/cloud
vi /opt/cloud/credentials/ssh.keys
vi /opt/cloud/credentials/variables.sh
```

After you make proper changes to ssh.keys and variables.sh files, you are able to deploy
Server Farmer on the new instance just by executing a single command:

```bash
/opt/cloud/deploy.sh ec2-hostname.compute.amazonaws.com /path/to/initial/ssh.key
```


# Extension support

Server Farmer comes with a bunch of powerful extensions, that provide:
- secure, unified configuration of various common software (eg. PHP)
- various tools for infrastructure monitoring and management
- set of production-ready commercial services, just as used at http://fajne.it

### How to install an extension:

```bash
/opt/farm/scripts/setup/role.sh sf-helloworld
```

If installed extension contains setup.sh script, it will be executed right after downloading it.

### Infrastructure monitoring extensions:

|name | description|
|:----|:-----------|
|sf-monitoring-newrelic | connects current server to NewRelic server monitoring service|
|sf-monitoring-snmpd | unified snmpd configuration to allow monitoring current server through SNMP protocol|
|sf-monitoring-cacti | additional Cacti-specific reporting about hard drives, server temperatures, MTA queue site etc.|
|sf-ip-monitor | notifies sysadmin about sudden external IP changes|
|sf-log-monitor | notifies about unexpected/suspicious entries in syslog logs|
|sf-standby-monitor | prevents overheating of (mostly external) heat-sensitive hard drives|

### Infrastructure management extensions:

|name | description|
|:----|:-----------|
|sf-log-receiver | rsyslog configuration profile - central log storage|
|sf-log-forwarder | rsyslog configuration profile - forwards logs from current host to central log storage|
|sf-log-rotate | logrotate configuration profiles|
|sf-mta-forwarder | minimal MTA, forwarding all messages to other host in LAN|
|sf-mta-relay | full MTA (based on Postfix), forwarding all messages to external SMTP relay, using SSL, with message queue etc.|
|sf-secure-fs | enforce secure directory layout (ordinary users no longer see contents of system configuration directories)|
|sf-secure-sshd | unified and secured sshd configuration, that allows logging in as root using ssh key, and disables dangerous options|
|sf-farm-manager | provides set of simple tools to manage your server farm|
|sf-farm-inspector | provides tools to analyze the health level of your server farm|
|sf-backup-collector | central backup functionality for small and medium server farms|
|sf-db-tools | scripts to manage MySQL and Postgres database servers|

### Standard software installers extensions:

|name | description|
|:----|:-----------|
|sf-java8 | Oracle Java 8 JRE|
|sf-openvz | OpenVZ environment for Debian Wheezy using Proxmox VE kernel|
|sf-php | unified configuration of PHP 5.x|
|sf-rar | RAR for Linux compression software|
|sf-rssh | restricted shell for OpenSSH|

### Commercial services extensions:

|name | description|
|:----|:-----------|
|sf-apache22-server | unified Apache httpd 2.2 configuration|
|sf-tomcat8-server | unified Apache Tomcat 8.0.x configuration|
|sf-imap-server | complete IMAP server for small and middle companies, combined with fetchmail|
|sf-imap-storage | central storage and backup solution for managed IMAP servers|
|sf-moosefs-server | a bunch of useful management scripts for MooseFS|
|sf-rsync-server | complete office backup solution for small and middle companies|
|sf-samba-server | managed Samba solution for small and medium companies|
|sf-motion | visual surveillance solution using open source "motion" software, and set of cheap USB cameras|

### Other extensions:

|name | description|
|:----|:-----------|
|sf-backup | essential backup functionality for local machines|
|sf-gpg | backup encryption ability for Server Farmer|
|sf-helloworld | demonstation, how to write your own extensions|
|sf-mc-black | black color theme for Midnight Commander|
|sf-ntp | maintain local time on current machine using ntpdate|
|sf-sms-smsapi | SMS sender script, using smsapi.pl service|


# Adding support for new OS/distro

Support for more distributions can ba added very easily. To do it yourself, look at
latest dist subdirectory:

- dist/debian-jessie for Debian-based distributions
- dist/redhat-centos7 for Red Hat-based distributions

Each subdirectory contains set of configuration files taken from distribution's
/etc directory, with our patches applied. Now create and install two small virtual
machines:

- first with supported distribution, e.g. Debian Jessie
- second with your new distribution

On first VM you can view exact differences between default configuration files
and ones from repository, by running e.g.:

```
diff -u /etc/apt/sources.list /opt/farm/dist/debian-jessie/sources.list
```

On second VM you can replicate these changes. Note that you don't have to add
all files for all services.


# Owner-specific data

Server Farmer repository contains no customer passwords, private keys etc. However it
contains sensitive data related to the owner.

If you want to fork this repository, you probably will want to change them. All such
data are stored inside one file: scripts/functions.custom.


# Support

If you have any technical or non-technical questions about Server Farmer, feel free to
write to support@serverfarmer.org. We don't have enough time to reply to all emails,
but we will try either to respond, or directly to fix any reported issues.
