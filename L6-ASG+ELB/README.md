<img src="https://welastic.pl/wp-content/uploads/2020/05/cropped-welastic_logo-300x259.png" alt="Welastic logo" width="100" align="left">
<br><br>
<br><br>
<br><br>

# Build your first Amazon EC2 Auto Scaling

## LAB Overview

#### This lab shows you how to use Auto Scaling to automatically launch Amazon EC2 instances in response to conditions that you specify. You will then test Auto Scaling by terminating a running instance and watching while Auto Scaling automatically creates a replacement instance.

## Task 1: Creating an AMI from your Instance
First you need to create an image (AMI) from your EC2 instance.

1. In the AWS Management Console, on the **Services** menu, click **EC2**.
2. Go to Instances and select **stundetX_01** (where X is your login number). 
3. On the **Actions** menu, click **Image** and then **Create Image**. 
4. For **Image name** box, enter **studentX_AMI**. 
5. In the **Image description** box, type your name. 
6. Click **Create Image**. 
7. In the navigation pane on the left, click **AMIs**. 
8. Locate your AMI and wait until Status will change to **available**. 

## Task 2: Create Elastic Load Balancer

For this section, you will be creating a simple HTTP load balancer. This load balancer will be used with auto
scaling group.

1.  In *EC2* service on the left navigation pane, click **Load Balancers**. 
2.  Click **Create Load Balancer**. 
3.  In **Select load balancer type** page, for **Classic Load Balancer**, click **Create**. 
4.  In the **Load Balancer name** box, type a new name **studentX-ELB**. 
5.  In the **Create LB Inside** box, select your VPC. 
6.  Check **Enable advanced VPC configuration** checkmark.
7.  In the **Listener Configuration:** set parameters:
    * **Load Balancer Protocol**: HTTP
    * **Load Balancer Port**: 80
    * **Instance Protocol**: HTTP
    * **Instance Port**: 80
8.  In **Select Subnets** configuration look for **Available subnets** and click a **+** button for your all Public subnets. 
9.  Click **Next: Assign Security Groups**. 
10. For **Assign a security group**, make sure that **Create a new security group** is selected. 
11. In the **Security group name** box, type a name **studentX-ELB-SG**. 
12. In the **Description** box, type **studentX SG for ELB**. 
13. Create a rule for **HTTP** and **custom source 0.0.0.0/0** 
14. Click **Next: Configure Security Settings** and **Next: Configure Health Check**. 
15. For Health Check settings provide following configuration: 
    * **Ping Protocol**: HTTP
    * **Ping Port**: 80
    * **Ping Path**: /index.php
    * **Response Timeout**: 5 seconds
    * **Interval**: 7 seconds
    * **Unhealthy threshold**: 2
    * **Healthy threshold**: 2
16. Click **Next: Add EC2 Instances**.

At this time you don’t select any EC2 instances, because they will configured by Auto Scaling Group.

17. Leave default and click **Next: Add Tags.**
18. Leave default and click **Review and Create**.
19. Click **Create**.

## Task 3: Create a Launch Configuration

A launch configuration specifies the type of EC2 instance that Auto Scaling launches for you. You create the
launch configuration by specifying information such as the Amazon Machine Image (AMI) ID to use for
launching the EC2 instance, the instance type, key pairs, security groups, and block device mappings,
among other configuration settings.

1. In the left navigation pane, click **Launch Configurations** (you might need to scroll down to find it).

2. Click **Create launch configuration**.
3. In **Name** type a new name "studneX-LC"

You need to select an Amazon Machine Image (AMI), which is a template for the root volume of the
instance and can contain an operating system, an application server and applications. You use an AMI to
launch an instance, which is a copy of the AMI running as a virtual server in the cloud. In this LAB you will
use an AMI created in previous task.

4. Click **Choose an AMIs**.
5. Look for an AMI you created in task 1 and press it.

You need to choose **Instance Type**.
When you launch an instance, the **instance type** determines the hardware allocated to your instance.

6.  Click **Choose instace type** find and select **t2.micro** and click **Choose**
7.  In *Additional configuration - optional*  select **Enable EC2 instance detailed monitoring within CloudWatch**.
8.  Expand **Advanced Details** in *Additional configuration*.
9.  For **IP Address Type** select **Assign a public IP address to every instance.**
10. In *Security groups* choose **Select an existing security group** and select your security group for WebServers.
11. In *Key pair (login)* Select a your key and check Acknowledge box and **Create launch configuration**.
12. Click **Create launch configuration** 

Your launch configuration has now been created and you will be prompted to create an Auto Scaling group.

## Task 4: Create an Auto Scaling Group

The Launch Configuration defines what should be launched, while the Auto Scaling group defines how
many instances to launch and where to launch them within your network.

1. In *Launch configurations* select your Launch configuration, click **Actions** and click **Create Auto Scaling Group**
1. For *Auto Scaling group name* type **studentX_ASG**
2. You should have Launch configuration alredy selected.
3. Click **Next**
4. On the Configure Auto Scaling group details page, configure:
    * **VPC:** Select your VPC 
    * **Subnet:** Select your all **Public** subnets
5. Click **Next**
6.  In *Load balancing - optional* select **Enable load balancing** and select **Classic Load Balancer**
7.  In *Select Load Balancer* field select your load balancer created in task 2
9.  In *Health checks - optional* section select **ELB**.
10. In *Additional settings - optional* section select **Enable group metrics collection within CloudWatch**
11. Click **Next**, **Next**, **Next**, **Next**  and **Create Auto Scaling group**

Auto Scaling will launch an Amazon EC2 instance and will maintain the Auto Scaling group at that quantity.

## Task 5: Verify your Auto Scaling group

Now that you have created your Auto Scaling group, you can verify that the group has launched your EC2
instance.

1.  On the Auto Scaling Groups page, select the Auto Scaling group that you just created.

Examine the **Details** tab to view information about the Auto Scaling group.

2.  Scroll down to *Advanced configurations* section and clic **Edit**
3.  Change a **Default Cooldown** to **60**.
4.  Click **Update**.
5.  Click the **Activity** tab.

In the *Activity history* section, status column contains the current status of your instance. When your instance is launching, the status
column shows In progress. The status changes to Successful once the instance is launched. You can click the
refresh button to see the current status of your instance.

6. Click the **Instance managment** tab.

In the *Instance* section, lifecycle column contains the state of your newly launched instance. You can see that your Auto
Scaling group has launched your EC2 instance and it is in the *InService* lifecycle state. The Health Status
column shows the result of the EC2 instance health check on your instance.

## Task 6: Verify your Load Balancer

Now when the Auto Scaling Group is created and EC2 machine is operational, you can check the status of
your Load Balancer.

1.  In the left navigation pane, click **Load Balancers**.
2.  Select the load balancer that you created, click the **Instances** tab, and wait for the status of Instance to change to *InService*. To refresh the status, click the circular arrow icon in the upper-right. 
3.  When the status of Instance is *InService*, click the **Description** tab. 
4.  Copy the **DNS name** value to your Clipboard. 

Load balancers can span Availability Zones, and they also scale elastically as needed to handle demand. Therefore, you should always access a load balancer by DNS hostname, and not by IP address. A load balancer may have multiple IP addresses associated with its DNS hostname.

5.  Open a new browser window, paste the DNS Name value into the address bar, and press ENTER. You should get a view of your application.
6.  Return to the AWS Management Console and, on the **Services** menu, click **CloudWatch**.
7.  In the navigation pane, click **Metrics** and in the **All metrics** tab, click **ELB**.
8.  Scroll up and down to select the metric or metrics you would like to view.

Load balancing metrics include latency, request count, and healthy and unhealthy host counts. Metrics are reported as they are encountered and can take several minutes to show up in CloudWatch.

## Task 7: Increase instance number

This time you will reconfigure Auto Scaling group to maintain two EC2 instances.

1.  In the AWS Management Console, on the **Services** menu, click **EC2**.
2.  In the left navigation pane, click **Auto Scaling Groups**.
3.  Select your Auto Scaling Group and press  **Edit**.
4.  In *Group size*  **Set**
    * **Desired capacity** - 2
    * **Maximum capacity** - 2
5.  Scroll down and click **Update** .

If you scale you group manually you have to configure this parameters of Minimum, Desired and Maximum number of instances that Auto Scaling Group should have at any time.

6.  Observe **Activity** and **Instance managment*** tab for any changes.
7.  When both instances in **Instance managment*** tab will be in *InService* go back to your Load Balancer.
8.  Select your load balancer and check in **Instances** tab if both servers are in *InService*.
9.  When the status of both Instances is *InService*, click the **Description** tab.
10. Copy the DNS name value to your Clipboard.
11. Open a new browser window, paste the DNS Name value into the address bar, and press ENTER.
12. Press Refresh button few times.

By default Load Balancer use Round Robin to distribute a new connection to backend servers. It is possible to implement Sticky Sessions, but it’s not recommended option for auto scaled environments. Do you know why?

13. After all, change a configuration of an Auto Scaling Group to Max 3 instances.

## Task 8: Test Auto Scaling

Try the following experiment to learn more about Auto Scaling. The Desired size for your Auto Scaling group is 2 instances. Therefore, if you terminate the running instance, Auto Scaling must launch a new instance to replace it.

1. Go to *Auto Scaling groups* select your ASG and go to the **Instances managment** tab, click Instance ID.

You will be taken to the Amazon EC2 console the Instances page.

2.  Click on **Instane ID** to go to properties of your instance. 
3.  From **Instance summary** copy **VPC ID**.
4.  In new tab open **http://cm.wecloud.ninja/** and paste your **VPC ID**.
5.  Press **ATTACK** button.
6.  Go back to EC2 console.
The instance will change to shutting-down.

7. In the left navigation pane, select **Auto Scaling Groups** and then select our AS group and the **Instances managment** tab.

You will see the initial instance status as Terminating. Soon thereafter you will see a new instance appear with a status of Pending or InService. The default cooldown for the Auto Scaling group is 300 seconds (5 minutes), so it may take about 5 minutes until you see the scaling activity

8. Return to the **Activity** tab.

All scaling activities are logged here. After the scaling activity starts. Click the triangles to view entries for the launch and termination of the first instance and then an entry for the launch of the new instance.

9. When a new instance will be in *InService*, check if your application is running.
10. Open a new browser window, paste the ELB’s DNS Name value into the address bar, and press ENTER.


## Task 9: Scaling Policies for AutoScaling

This time you will use ScalingPolicies to scale your application based on CPU utilization.

1.  Go back yo your AutoScaling configuration. 
2.  Click the **Automatic scaling** tab.
3.  In *Scaling Policies* section click **Add policy**.
4.  Select **Policy type** to **Targe tracking scaling** (default)
5.  Set a **Scaling policy name**: studentX_UP
6.  Select **Metric type** to **Average CPU utilization**(default)
7.  Set  **Target value** to 20
8.  Click **Create**.

9.  Login over SSH into two instances created by AutoScaling group. 
10. On both instances install a test tool:

`` sudo yum install stress -y ``

11. On both instances run a stress command: 

``
sudo stress --cpu 100 
``

12. Observe Activity History and Instances tabs for any changes.

When the cpu utilisation will go above the created alarm, a new instance will be added. 

13. Stop a stress command by pressing CTRL+C.
14. Observe Activity History and Instances tabs for any changes.


## END YOUR LAB

Follow these steps to close the console, end your lab.

1.   In the AWS Management Console, on the **Services menu**, click **EC2**. 
2.   In the left navigation pane, click **Auto Scaling Groups**.
3.   Select your AS group and click **Delete**, type *dekete* and click **Delete**. 
4.   In the left navigation pane, click **Launch Configurations**.
5.   Select your configuration and click **Actions**, select **Delete launch configuration**, click **Delete**.
6.   In the left navigation pane, click **Load Balancers**.
7.   Select your ELB and click **Actions**, select **Delete**, click **Yes, Delete**.

<br><br>

<p align="right">&copy; 2020 Welastic Sp. z o.o.<p>
