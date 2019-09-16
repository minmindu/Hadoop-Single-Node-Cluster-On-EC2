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


![alt text](./Images/14.png "Allocate")


![alt text](./Images/15.png "Allocate")


![alt text](./Images/16.png "Allocate")