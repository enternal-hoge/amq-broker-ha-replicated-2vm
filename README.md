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

$ sudo su -
Last login: Fri Apr 16 05:01:01 UTC 2021 on pts/0

$ java -version
openjdk version "11.0.10" 2021-01-19 LTS
OpenJDK Runtime Environment 18.9 (build 11.0.10+9-LTS)
OpenJDK 64-Bit Server VM 18.9 (build 11.0.10+9-LTS, mixed mode, sharing)

$ exit
```

## 2. Upload AMQ Broker Installer file



## 3. Install Procedure

## 4. Linux OS Configure

## 5. Registrer Linux Service

## 6. AMQ Broker HA Configure

## 7. Testing

## 8. etc

