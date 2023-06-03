# Installation WordPress Application on AWS EC2 by RDS Multi-AZ Deployment and Read Replica


This guide provides step-by-step instructions on how to deploy a WordPress application on AWS EC2, utilizing RDS Multi-AZ deployment for high availability and a Read Replica for improved performance.

## Prerequisites

- An AWS account with appropriate permissions to create and manage EC2 instances, RDS instances, and security groups.
- Basic knowledge of AWS services, EC2, RDS, and SSH.

## Steps

### Step 1: Launch an EC2 instance

Launch an EC2 instance using the AWS Management Console or AWS CLI. Ensure that the instance has the necessary security group rules to allow inbound traffic on port 80 (HTTP) and port 22 (SSH).

```bash
# Example AWS CLI command:
aws ec2 run-instances --image-id <ami-id> --instance-type <instance-type> --security-group-ids <security-group-id> --key-name <key-pair-name>
```

### Step 2: Connect to the EC2 instance

Connect to the EC2 instance using SSH, using the private key associated with the instance.

```bash
# Example SSH command:
ssh -i <private-key.pem> ec2-user@<public-ip-address>
```

### Step 3: Install and configure WordPress on EC2

Update the package repository, install Apache web server, MySQL server, PHP, and required modules. Download and extract WordPress files, and configure the WordPress database.

```bash
# Example commands:
sudo yum update -y
sudo yum install httpd -y
sudo service httpd start
sudo yum install mysql-server -y
sudo service mysqld start
sudo mysql_secure_installation
sudo yum install php php-mysql -y
wget https://wordpress.org/latest.tar.gz
tar -xvzf latest.tar.gz
sudo cp -r wordpress/* /var/www/html/
sudo chown -R apache:apache /var/www/html/
sudo chmod -R 755 /var/www/html/
```

### Step 4: Launch an RDS instance with Multi-AZ deployment

Launch an RDS instance using the AWS Management Console or AWS CLI, ensuring Multi-AZ deployment is enabled for high availability.

```bash
# Example AWS CLI command:
aws rds create-db-instance --db-instance-identifier <db-instance-identifier> --engine <engine> --db-instance-class <instance-class> --multi-az --allocated-storage <storage-size> --master-username <username> --master-user-password <password> --publicly-accessible
```

### Step 5: Create a Read Replica for the RDS instance

Create a Read Replica for the RDS instance using the AWS Management Console or AWS CLI, which enhances scalability and read performance.

```bash
# Example AWS CLI command:
aws rds create-db-instance-read-replica --db-instance-identifier <read-replica-identifier> --source-db-instance-identifier <source-db-instance-identifier> --db-instance-class <instance-class> --publicly-accessible
```

### Step 6: Configure WordPress to use RDS

In the EC2 instance, navigate to the WordPress directory and edit the `wp-config.php` file. Update the database host, name, username, and password with the RDS details.

```bash
# Example commands:
cd /var/www/html/
sudo nano wp-config.php
```

### Step 7: Test the WordPress installation

- Open

 a web browser and enter your EC2 instance's public IP address or domain name.
- Follow the WordPress setup wizard to complete the installation.
- Verify that the WordPress site is accessible and functioning properly.

## Conclusion

By following these steps, you will have successfully deployed a WordPress application on AWS EC2, utilizing RDS Multi-AZ deployment and a Read Replica. This setup provides high availability, fault tolerance, and improved performance for your WordPress site.