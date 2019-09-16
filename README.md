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
ssh -i "*.pem" ubuntu@public-dns-name
```

![alt text](./Images/20.png "SSH")


![alt text](./Images/21.png "Login 1")

6. Update the packages of AMI (Amazon Machine Image)
```
sudo apt update
sudo apt upgrade
```
![alt text](./Images/22.png "Update")

7. Change Hostname to *hadoop*
```
sudo nano /etc/hostname
```
![alt text](./Images/23.png "Hostname")


8. Reboot and Reconnect to Instance
```
sudo reboot
```
![alt text](./Images/24.png "Reboot")


```
ssh -i "*.pem" ubuntu@public-dns-name
```

![alt text](./Images/25.png "SSH")


9. Install Open JDK 8

```
sudo apt install openjdk-8-jdk
```

![alt text](./Images/26.png "JDK")

