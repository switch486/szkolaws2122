<img src="https://welastic.pl/wp-content/uploads/2020/05/cropped-welastic_logo-300x259.png" alt="Welastic logo" width="100" align="left">
<br><br>
<br><br>
<br><br>

# Create container based application with AWS ECS Fargate

## LAB Overview

#### This lab leads you through the steps to run a simple Mario Bros game as website. You will use provided docker image and run it using Amazon ECS service.


## Task 1: Create ECR repository.

In thisk task you will create a ECR repository and learn how to push into your new repo. 

1. On the **Services** menu, click IAM.
2. On the left menu click Users.
3. Click on your user name.
4. Go to **Security credencials** tab.
5. Click **Create acces key**.
6. Save yor Access key and Secret key to notepad.


7. On the **Services** menu, click **EC2**. 
8. Go into **Instances**. 
9. Click **Launch Instance**.
10. Choose **Amazon Linux AMI 2018.03.0 (HVM), SSD Volume Type** and click **Select**
11. Select **Type** **t2.micro** and click **Review and Launch**. 
12. Click **Launch**.
13. If you already have a key pair, select your key pair, if not then create a new one and save it.
14. Check **I acknowledge that I have access to the selected private key file (keyfile.pem), and that without this file, I won't be able to log into my instance** .
15. Click **Launch Instances**.
16. Click **View Instances**.
17. Wait till your instance starts.
18. Select your new instance and click **Actions** and click **Connect**. 
19. Use **Example** shh comand to connect to your machine.


20. In your EC2 machine run following commands. 

```she
sudo yum install docker
```

```she
sudo service docker start
```

```she
sudo chmod 777 /var/run/docker.sock
```
```she
docker pull kaminskypavel/mario
```
21. To configure AWS Credetials run comand bellow.
```she
aws configure
```
22. Paste preveriusly saved **AWS Access Key** and **AWS Secret Access Key**. Set **Default region name** to eu-west-1.

24. In comand bellow replace X with your student number, and run it to create ECR repository.
```she
aws ecr create-repository --repository-name studentXmario
```

25. Go back to AWS Console, in **Services** click **Elastic Container Service**.
26. In the left menu click **Repositories**
27. Click on your repository name. 
28. Click on **View push commands**. 
29. To retrieve an authentication token and authenticate your Docker client to your registry. Paste first command in your EC2 terminal. 
```she
aws ecr get-login-password --region eu-west-1 | docker login --username AWS --password-stdin 536966783968.dkr.ecr.eu-west-1.amazonaws.com
```
30. Go back to AWS console.
31. To push image to repository you have to tag it. Paste third comand in your EC2 terminal and rename name of docker image as in example bellow. 
```she
docker tag kaminskypavel/mario:latest 536966783968.dkr.ecr.eu-west-1.amazonaws.com/studentXmario:latest
```

32. Copy fourth command to your EC2 terminal, and run this command to push your docker image to AWS ECR repostory. 
```she
docker push 536966783968.dkr.ecr.eu-west-1.amazonaws.com/studentXmario:latest
```

33. Go back to AWS console and check if your image is present in ECR repository. 



## Task 2: Create ECS Cluster
1. In **Services** select Elastic Container Service
2. In the navigation pane on the left, click **Clusters** under Amazon ECS.
3. Click **Create cluster** button.

**Note:** You will find tree options. Two options with classic EC2 machines, where virtual machines will be created in ECS Cluster. Third option (Fargate) is a serverless approach.

4. Select option **Networking only (Powered by AWS Fargate)** and **Next step**.
5. Provide all informations: 
   * **Cluster name:** studentXcluster,
   * **Create VPC**: leave unselected.

6. Click **Create** and **View cluster**.

## Task 3: Create a task definition

In this section, you will create and run task that will spin up our docker container with an application.

1.  In the navigation pane on the left, click **Task Definitions**.
2.  Then **Create new Task Definition**, select **FARGATE** and click **Next step**.
3.  Provide all necessary information:

* **Task Definition Name:** StudentX_task
* **Task memory:** 1GB
* **Task CPU:** 0.5 vCPU

4. In **Container Definition** select **Add Container.**
5. Provide following information: 

* **Container name**: studentX_container
* **Image**: 536966783968.dkr.ecr.eu-west-1.amazonaws.com/studentXmario:latest
* **Port mappings:** 8080

6.  Rest of the container configuration leave default and click **Add**. 
7.  Under the task definition windows leave the rest of configuration default and click **Create**. 
8.  Click **View task definition**. 

You can review the configuration. Eventually you can also specify the config in the form of json template.

## Task 4: Run a task

Almost all of the configuration is ready, now you can run your task and test the application.

1.  In the navigation pane on the left, click **Clusters**. 
2.  Select your cluster and switch to **Tasks** tab. 
3.  Click **Run new Task** and provide the following configuration. 

* **Launch type:** Fargate
* **Task definition:** select your task created in previous task.
* **Cluster**: select your cluster 
* **Number of tasks:** 1
* **Cluster VPC:** use your vpc
* **Subnets:** select your Public subnets
* **Security groups**: make sure that port 8080 is opened
* **Auto-assign public IP**: ENABLED

4.  Leave the rest and click **Run Task**
5.  Wait until the service will be in **RUNNING** state 
6.  Click on your task and look for **Public IP** 
7.  Open a new windows in web browser and paste the address with :8080 at the end. 

## END LAB

This is the end of the lab. You can remove an ECS cluster, and terminate EC2 machine. 







<br><br>

<p align="right">&copy; 2020 Welastic Sp. z o.o.<p>