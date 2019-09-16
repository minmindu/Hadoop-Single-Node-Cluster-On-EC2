# Hadoop Single Node Cluster On EC2

### Creating a Single Node Hadoop Cluster on Amazon Web Services EC2

![alt text](https://mapr.com/products/apache-hadoop/assets/hadoop-logo.png "Hadoop Logo")

### Prerequisites
* AWS Acount
* Terminal [(If On Windows UNIX-Like terminal)](https://itsfoss.com/run-linux-commands-in-windows/)
* Stable Internet Connection

### Setting up of Instance in AWS EC2
1. Login to AWS (I've login using AWS Educate)

![alt text](./Images/1.png "AWS Educate Homepage")

![alt text](./Images/2.png "AWS Educate Login")

![alt text](./Images/3.png "Navigate to AWS Account")

![alt text](./Images/4.png "Navigate to AWS Console")


2.  Navigate to EC2

![alt text](./Images/5.png "Navigate to AWS Account")


3. Launch an Instance

![alt text](./Images/6.png "Launch Instance")

* Choose Ubuntu Server 18.04 LTS as Amazon Machine Image

![alt text](./Images/7.png "Ubuntu AMI")


* Choose t2.large as instance type (Recommended), if not you can go with other configuration

![alt text](./Images/8.png "Instance Type")


* Make sure the security group has all traffic allowed to inbound and outbound.

![alt text](./Images/9.png "Security Group")


* Generate a Private Key file (.pem)

![alt text](./Images/10.png "Pem")


* Launch the Insatance 

![alt text](./Images/11.png "Launch Instance")


4. Assign Elastic IP to Instance

* Navigate to Elastic IP

![alt text](./Images/12.png "Elastic IP")

* Allocate IP Address

![alt text](./Images/13.png "Allocate")


* Associate the elastic IP to the Instance

![alt text](./Images/14.png "Allocate")


![alt text](./Images/15.png "Allocate")


![alt text](./Images/16.png "Allocate")


5. Connecting to Instance 

* Start the Instance

![alt text](./Images/17.png "Start Instance")


* Connect to the Instance

![alt text](./Images/18.png "Connect")

* Change the permission of Private Key

![alt text](./Images/19.png "Permissions")

* Connect using SSH

```
$ ssh -i "*.pem" ubuntu@public-dns-name
```

![alt text](./Images/20.png "SSH")


![alt text](./Images/21.png "Login 1")

6. Update the packages of AMI (Amazon Machine Image)
```
$ sudo apt update
$ sudo apt upgrade
```
![alt text](./Images/22.png "Update")

7. Change Hostname to *hadoop*
```
$ sudo nano /etc/hostname
```
![alt text](./Images/23.png "Hostname")


8. Reboot and Reconnect to Instance
```
$ sudo reboot
```
![alt text](./Images/24.png "Reboot")


```
$ ssh -i "*.pem" ubuntu@public-dns-name
```

![alt text](./Images/25.png "SSH")


9. Install Open JDK 8

```
$ sudo apt install openjdk-8-jdk
```

![alt text](./Images/26.png "JDK")

10. Download and Install Hadoop

* Go to Downloads page of Hadoop

![alt text](./Images/27.png "Hadoop Download Page")


* Copy the US mirror link

![alt text](./Images/28.png "US Mirror")

* Download in the Instance

```
$ wget <link> -P ~/Downloads
```
![alt text](./Images/29.png "Wget")


* Extract the package

```
$ sudo tar zxvf ~/Downloads/hadoop-* -C /usr/local
```

![alt text](./Images/30.png "Extract")


* Rename the folder

```
$ sudo mv /usr/local/hadoop-* /usr/local/hadoop
```

![alt text](./Images/31.png "Rename")


11. Setting up of Environment Variables

* Open .bashrc file in your home directory with your favorite editor. Include the below lines.

```
$ nano ~/.bashrc
```
* For Hadoop

```
export HADOOP_HOME=/usr/local/hadoop
export HADOOP_INSTALL=$HADOOP_HOME
export HADOOP_MAPRED_HOME=$HADOOP_HOME
export HADOOP_COMMON_HOME=$HADOOP_HOME
export HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop
export HADOOP_HDFS_HOME=$HADOOP_HOME
export YARN_HOME=$HADOOP_HOME
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_HOME/lib/native
export PATH=$PATH:$HADOOP_HOME/sbin:$HADOOP_HOME/bin
```

![alt text](./Images/32.png "Env Var")


* For reflecting to current session with out restarting.

```
$ source ~/.bashrc
```

* Open hadoop-env.sh from $HADOOP_CONF_DIR

```
$ sudo nano $HADOOP_CONF_DIR/hadoop-env.sh
```

![alt text](./Images/33.png "Env Var")

* Change Java Home and Hadoop Conf Dir

![alt text](./Images/34.png "Env Var")


12. SSH

* Copy .pem file to .ssh directory

```
$ cp <Path of .pem> ~/.ssh/ (If LINUX Or MacOS)

For Windows:
Locate .ssh directory and perform the same
```

* Create a config file in .ssh directory of local system (Not Instance)

```
$ nano ~/.ssh/config
```

* Enter the following into it

```
Host hadoop
  HostName <publicDnsname>
  User ubuntu
  IdentityFile ~/.ssh/<.pem file name>
```

![alt text](./Images/35.png "SSH Config")

* Copy the config and pem files to Instance .ssh directory

```
$ scp ~/.ssh/config hadoop:~/.ssh
$ scp ~/.ssh/*.pem hadoop:~/.ssh
```

* Log in to NameNode, create a public key using ssh-keygen and copy it to authorized_keys

```
$ ssh hadoop
$ ssh-keygen -f ~/.ssh/id_rsa -t rsa -P ""
$ cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
```

* Open /etc/hosts

```
$ sudo nano /etc/hosts
```

* Add Instance IPV4 address with hostname (hadoop)

![alt text](./Images/36.png "SSH Config")


13. Hadoop Configuration

* In the configuration folder, edit core-site.xml.

```
$ sudo nano $HADOOP_CONF_DIR/core-site.xml
```

* Paste the following

```
<configuration>
  <property>
    <name>fs.defaultFS</name>
    <value>hdfs://<your namenode public dns name>:9000</value>
  </property>
</configuration>
```

* Edit yarn-site.xml.

```
$ sudo nano $HADOOP_CONF_DIR/yarn-site.xml
```

* Paste the following

```
<configuration>
  <property>
    <name>yarn.nodemanager.aux-services</name>
    <value>mapreduce_shuffle</value>
  </property>
  <property>
    <name>yarn.resourcemanager.hostname</name>
    <value><your namenode public dns name></value>
  </property>
</configuration>
```

* Copy mapred-site.xml from mapred-site.xml.template

```
$ sudo cp $HADOOP_CONF_DIR/mapred-site.xml.template $HADOOP_CONF_DIR/mapred-site.xml
```

* Edit the following

```
<configuration>
  <property>
    <name>mapreduce.jobtracker.address</name>
    <value><your namenode public dns name>:54311</value>
  </property>
  <property>
    <name>mapreduce.framework.name</name>
    <value>yarn</value>
  </property>
</configuration>
```

* Edit hdfs-site.xml

```
<configuration>
  <property>
    <name>dfs.replication</name>
    <value>1</value>
  </property>
  <property>
    <name>dfs.namenode.name.dir</name>
    <value>file:///usr/local/hadoop/data/hdfs/namenode</value>
  </property>
  <property>
    <name>dfs.datanode.data.dir</name>
    <value>file:///usr/local/hadoop/data/hdfs/datanode</value>
  </property>
</configuration>
```

* Make namenode and datanode directories

```
$ sudo mkdir -p $HADOOP_HOME/data/hdfs/namenode
$ sudo mkdir -p $HADOOP_HOME/data/hdfs/datanode
```

* Change the owner of HADOOP_HOME to ubuntu

```
$ sudo chown -R ubuntu $HADOOP_HOME
```
![alt text](./Images/37.png "CHOWN")


* Change masters and slaves to hadoop

```
sudo nano $HADOOP_HOME/masters
sudo nano $HADOOP_HOME/slaves
```

![alt text](./Images/38.png "slaves")


#### Launch Hadoop Cluster

* Format the file system, then start HDFS.

```
$ hdfs namenode -format
$ $HADOOP_HOME/sbin/start-dfs.sh
```

![alt text](./Images/39.png "HDFS Format")


* Start YARN

```
$ $HADOOP_HOME/sbin/start-yarn.sh
```

![alt text](./Images/40.png "Services")


* Start the job history server.

```
$ $HADOOP_HOME/sbin/mr-jobhistory-daemon.sh start historyserver
```
* To see the Java processes (Hadoop daemons for instance)

```
$ jps
```

![alt text](./Images/41.png "JPS")

#### Accessing NameNode using WebUI

* For Namenode Overview

```
publicdns:50070
```
![alt text](./Images/42.png "Overview")


* For Cluster Metrics

```
publicdns:8088
```

![alt text](./Images/43.png "Cluster Metrics")