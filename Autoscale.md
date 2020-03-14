<h1>Creating an auto scaling group and Application Load Balancer in AWS</h1>

## Some user data to test the labs for this project

 ```bash
 #!/bin/bash
 yum update -y
# install apache web server  
 yum install -y httpd
 #install wget
 yum install -y wget

#Navigate to the default web location
 cd /var/www/html

 wget https://raw.githubusercontent.com/linuxacademy/content-aws-csa2019/master/lab_files/07_hybrid_scaling/ASGandALB/index.html

 wget https://raw.githubusercontent.com/linuxacademy/content-aws-csa2019/master/lab_files/07_hybrid_scaling/ASGandALB/pinehead.png

```

## Use the following command to the strees test on the web server we installed

```bash
#installtion needed
sudo amazon-linux-extras install epel -y

sudo yum install -y stress

#strees testing command
stress --cpu 2 --timeout 300

```

Creating an application Load Balancer to load balance between EC2 instances you will create later on inside your Autoscaling group:

* Navigate to the EC2 portion of the console
* Click on the Load Balancers section under Load Balancing
* Press the Create button under the Application Load Balancer
* Name your Load Balancer - LABALB, leave the ALB set to internet          facing and the ip address type as ipv4
* Select the VPC and add the us-east-1a and us-east-1b AZs to your ALB, but NOT the us-east-1c AZ
* Create a new Security Group for your ALB, for name and description use ALBSG.
*  Configure rules ensuring that HTTP is allowed from 0.0.0.0/0 and ::/0 (IPV6)
*	Configure a Target Group for your ALB naming it ALBTG
* Expand advanced health check settings and reduce the healthy threshold check down to 2
*	Proceed to create your ALB.

### Create and configure the auto scaloing group used to scale the instances

* EC2 -> Auto Scaling -> Auto Scaling Groups
* Create auto scaling group, select launch template, choose the template you just created
* Call the group LABASG
* Start with 2 instances
* Pick the VPC from the lab environment and select us-east-1a and us-east-1b as subnets
* Click next configure scaling policies
* For now, keep the group at the initial size, next configure notifications, next configure tags, review, create, click close.

**MAKE SURE THE APPLICATION LOAD BALANCER IS READY AT THIS POINT**

### Enable Group Metrics Collection
* Click actions then click edit
* Set max instances to whatever number of instances needed, click the target group and pick the target group of the ALB.

### Configure Autoscaling group scalion policies:

* Select the scaling Polices tab of the ASG
* Add Policy 
* Click create a simple scaling policy
* Name is SCALEOUT, take the action add 1 instances, enter 300 seconds before allowing another scalin activity.
* Click create alram, close
* Click Create
* Click Add Policy again
* Click create a simple scaling policy, call it SCALEIN
* Take the action Remoive 1 instances, wait  300 seconds
* Click create new alram.
* Uncheck SNS
* Average CPU utilisation is <= 20 percent for 1 period of 5 minutesm, call it LOWCPU
* Create Alarm, close, create
check_circleTest Horizontal Scalingkeyboard_arrow_up.

## Connect to one of the webservers and stree test it using the stress command the begining of this instruction.

* Connect to one of the EC2 instances
* Install the stress test application using 

```bash
#installtion needed 
sudo amazon-linux-extras install epel -y

sudo yum install -y stress

#strees testing command 
*  Run the Stress test on the EC2 instances stress --cpu 2 --timeout 300 (Optionally using 3000 for timeout)

*  fter a few minutes watch the number of intances increase.



