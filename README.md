# AMQ Broker HA Replicatate 2VM

## TBD


## Prerequirement

* 2VM
  * prepare 2VM RHEL 8.x
  * ex. AWS EC2, Azure VM

* AMQ Broker 7.8.1
  * Download AMQ Broker installer at Red Hat Portal site.
  * amq-broker-7.8.1-bin.zip

* Open JDK 11
  * not use openjdk11 zip installer. use yum command.

## 1. Install OpenJDK 11

install openjdk11 both VMs.

```bash
$ sudo yum install java-11-openjdk-devel

$ java -version
openjdk version "11.0.10" 2021-01-19 LTS
OpenJDK Runtime Environment 18.9 (build 11.0.10+9-LTS)
OpenJDK 64-Bit Server VM 18.9 (build 11.0.10+9-LTS, mixed mode, sharing)

# sudo su -
Last login: Fri Apr 16 05:01:01 UTC 2021 on pts/0

# java -version
openjdk version "11.0.10" 2021-01-19 LTS
OpenJDK Runtime Environment 18.9 (build 11.0.10+9-LTS)
OpenJDK 64-Bit Server VM 18.9 (build 11.0.10+9-LTS, mixed mode, sharing)

# exit
```

## 2. Upload AMQ Broker Install file

upload AMQ Broker Install file on to current user directory.

ex. /home/\<USER>\>/amq-broker-7.8.1-bin.zip


## 3. Install Procedure

execute following commands both VMs.

```
$ sudo useradd amq-broker
$ sudo passwd amq-broker
Changing password for user amq-broker.
New password: 
Retype new password: 
passwd: all authentication tokens updated successfully.

$ sudo mkdir /opt/redhat
$ sudo mkdir /opt/redhat/amq-broker
$ sudo chown -R amq-broker:amq-broker /opt/redhat/amq-broker

$ sudo chown amq-broker:amq-broker amq-broker-7.8.1-bin.zip
$ sudo mv amq-broker-7.8.1-bin.zip /opt/redhat/amq-broker

$ su - amq-broker
$ cd /opt/redhat/amq-broker
$ unzip amq-broker-7.8.1-bin.zip

$ /opt/redhat/amq-broker/amq-broker-7.8.1/bin/artemis create mtg
Creating ActiveMQ Artemis instance at: /opt/redhat/amq-broker/amq-broker-7.8.1/mtg

--user: is a mandatory property!
Please provide the default username:
admin

--password: is mandatory with this configuration:
Please provide the default password:

--allow-anonymous | --require-login: is a mandatory property!
Allow anonymous access?, valid values are Y,N,True,False
Y

Auto tuning journal ...
done! Your system can make 0.62 writes per millisecond, your journal-buffer-timeout will be 1624000

You can now start the broker by executing:  

   "/opt/redhat/amq-broker/amq-broker-7.8.1/mtg/bin/artemis" run

Or you can run the broker in the background using:

   "/opt/redhat/amq-broker/amq-broker-7.8.1/mtg/bin/artemis-service" start
```

## 4. Registrer Linux Service

execute following commands both VMs.

```
$ sudo touch /etc/systemd/system/amq-broker.service
$ sudo vi /etc/systemd/system/amq-broker.service
$ sudo cat /etc/systemd/system/amq-broker.service
[Unit]
Description=AMQ Broker
After=syslog.target network.target

[Service]
ExecStart=/opt/redhat/amq-broker/amq-broker-7.8.1/mtg/bin/artemis run
Restart=on-failure
User=amq-broker
Group=amq-broker

# A workaround for Java signal handling
SuccessExitStatus=143

[Install]
WantedBy=multi-user.target

$ sudo systemctl enable amq-broker
Created symlink /etc/systemd/system/multi-user.target.wants/amq-broker.service → /etc/systemd/system/amq-broker.service.

$ sudo systemctl start amq-broker

$ sudo systemctl status amq-broker
● amq-broker.service - AMQ Broker
   Loaded: loaded (/etc/systemd/system/amq-broker.service; disabled; vendor preset: disabled)
   Active: active (running) since Sat 2021-04-17 01:02:28 UTC; 23s ago
 Main PID: 6184 (java)
    Tasks: 44 (limit: 11073)
   Memory: 230.6M
   CGroup: /system.slice/amq-broker.service
           └─6184 java -XX:+PrintClassHistogram -XX:+UseG1GC -Xms512M -Xmx2G -Dhawtio.disableProxy=true -Dhawtio.realm=>

Apr 17 01:02:39 AMQ01 artemis[6184]: SLF4J: Found binding in [jar:file:/opt/redhat/amq-broker/amq-broker-7.8.1/lib/slf4>
Apr 17 01:02:39 AMQ01 artemis[6184]: SLF4J: See http://www.slf4j.org/codes.html#multiple_bindings for an explanation.
Apr 17 01:02:39 AMQ01 artemis[6184]: SLF4J: Actual binding is of type [org.slf4j.impl.Log4jLoggerFactory]
Apr 17 01:02:39 AMQ01 artemis[6184]: INFO  | main | Initialising hawtio services
Apr 17 01:02:39 AMQ01 artemis[6184]: INFO  | main | Configuration will be discovered via system properties
Apr 17 01:02:39 AMQ01 artemis[6184]: INFO  | main | Welcome to hawtio 2.0.0.fuse-sb2-780022-redhat-00001
Apr 17 01:02:39 AMQ01 artemis[6184]: INFO  | main | Starting hawtio authentication filter, JAAS realm: "activemq" autho>
Apr 17 01:02:39 AMQ01 artemis[6184]: INFO  | main | Proxy servlet is disabled
Apr 17 01:02:39 AMQ01 artemis[6184]: INFO  | main | Jolokia overridden property: [key=policyLocation, value=file:/opt/r>
Apr 17 01:02:39 AMQ01 artemis[6184]: 2021-04-17 01:02:39,522 INFO  [org.apache.activemq.artemis] AMQ241001: HTTP Server>

Break[Cntrl + C]
```

## 5. Linux OS Configure

add lines into limits.conf both VMs.

```bash
$ sudo vi /etc/security/limits.conf
$ sudo cat /etc/security/limits.conf 
...

amq-broker   soft  nofile  20000
amq-broker   hard  nofile  20000

...
```


## 6. AMQ Broker HA Configure

TBD

## 7. Testing

TBD

## 8. etc

TBD

## 9. Reference

https://access.redhat.com/documentation/ja-jp/openjdk/11/html/installing_and_using_openjdk_11_on_rhel/installing-openjdk11-on-rhel8

https://access.redhat.com/documentation/en-us/red_hat_amq/2020.q4/html/getting_started_with_amq_broker/installing-broker-getting-started

https://activemq.apache.org/components/artemis/documentation/latest/perf-tuning.html

