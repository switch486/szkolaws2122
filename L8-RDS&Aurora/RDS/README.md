<img src="https://welastic.pl/wp-content/uploads/2020/05/cropped-welastic_logo-300x259.png" alt="Welastic logo" width="100" align="left">
<br><br>
<br><br>
<br><br>

# Introduction to Amazon Relational Database Service

## LAB Overview

#### This lab introduces you to Amazon Relational Database Service using the AWS Management Console.

## Task 1: Create new DB Subnet Group
DB subnet group defines which subnets and IP ranges the DB instance can use in the VPC you selected. During RDS instance creation is possible to let a wizard create a subnet group automatically. But for our lab, we want to have a full control of which subnets will be selected for RDS.

1. In the AWS Management Console, on the Services menu, click Relational Database Service.
2. In the left navigation pane, click **Subnet groups**.
3. Click **Create DB Subnet Group**.
4. On **Create DB subnet group**, step configure the following:

* **Name**: StudentX-DBSG
* **Description**: Your name
* **VPC**: Select your VPC created in LAB-VPC
* Click **Add all the subnets related to this VPC**

It is the fastest way, select all and then remove unnecessary. If you followed VPC-LAB instructions correctly,
fours subnet should appear in **Subnets in this subnet group**. Now you need to remove the public subnets
by clicking **Remove** button. You should remove 10.X.0.0/24 and 10.X.1.0/24 and leave 10.X.10.0/24 and
10.X.11.0/24.

* Click **Remove** button for 10.X.0.0/24 (where X indicates your student number)
* Click **Remove** button for 10.X.1.0/24

5. Click **Create**.

## Task 2: Create a Relational Database Service (RDS) Instance

In this task, you will create an Amazon RDS database for MySQL

1. In the left navigation pane, click **Databases**.
2. Click **Create database** button.
3. On **Create database**, select
   * Engine type: **MySQL**
   * Version: **Leave suggested option**
   * Template: **Production**
   * DB cluster identifier: **StudentX-mysql**
   * Master password: **Type password and save it in notepad**
   * DB instance class: **Burstable classes**
   * DB instance class: **db.t2.micro**
   * Storage type: **General Purpose (SSD)**
   * Allocated storage: **20**
   * Multi-AZ deployment: **Create create a standby instance (recommended for production usage)**
   * Virtual Private Cloud (VPC): **Select your VPC**
   * Expand **Additional connectivity configuration**
   * Subnet group: **Select group created in Task 1**
   * VPC security group: **Choose existing**
   * Existing VPC security groups: 
     - Select: **StudentX_DB**
     - Deselect: **default**
   * Expand **Addotional configuration**
   * Initial database name: **studentXdb**
   * Enable automatic backups: **Unselect**
   * Delection protection: **Unselect**
4. Click **Create database**.
5.  Wait until **Successfully created database studentX-mysql.** green information appear.
6.  Click **View credential details**.
7.  Copy **Master username**, **Master password** and **Endpoint** values to the notepad.

This is the only time you will be able to view this password. However you can modify your database to create a new password at any time.

## Task 3: Test your DB instance

You will now connect to the RDS database from your EC2 server. 

1. Connect to your  laboratory EC2 instance over SSH.
2. Go you website folder: 

```bash
cd /var/www/html
```

3. Create a new file **rdsdb.php**:

```bash
sudo nano rdsdb.php
```

4. Paste the following code ( code of the file [db.php](db.php))

```php
<?php
$servername = "enter your ENDPOINT";
$database = "studentXdb";
$username = "admin";
$password = "enter master password";
// Create connection
$conn = mysqli_connect($servername, $username, $password, $database);
// Check connection
if ($conn->connect_error) {
   die("Connection failed: " . $conn->connect_error);
}
  echo "<h1>Great!</h1><br>Connected successfully to:<b> $servername</b>";
?>
```

5.  Provide correct values for: **servername**, **database** and **password**
6.  Press CTRL+O, ENTER to save your document. 
7.  Press CTRL+X to exit the Nano editor.
8.  Open a new browser window, and then paste the public DNS value into the address bar and add tested.php.

Example: http://ec2-34-240-160-241.eu-west-1.compute.amazonaws.com/rdsdb.php

## Task 4: Test your DB instance with cli (optional)

1. Connect to your  laboratory EC2 instance over SSH.
2. Install mysql tool:

```shell
 yum install mysql -y
```

3. Connect to the database:

```bash
mysql -u admin -p -h enter_db_entpoint
```

4. List databases:

```my
show databases;
```

## 

## END LAB

This is the end of this lab. Go over following steps to remove database.

1.  In the AWS Management Console, on the **Services** menu, click **RDS**.
2.  In the left navigation pane, click **Databases**.
3.  Select your database and click **Actions** button.
4.  Select **Delete**
5.  Skip the final snapshot, confirm and click **Delete**.

<br><br>

<p align="right">&copy; 2020 Welastic Sp. z o.o.<p>