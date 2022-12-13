# WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS
## 1. Install Apache using Ubuntu’s package manager ‘apt’:
### step 1 - Update a list of packages in package manager

sudo apt update

### step 2 - Run apache2 package installation

sudo apt install apache2...

When i ran step 2 i came across “Package apache is not available, but is referred to by another package. This may mean that the package is missing, has been obsoleted, or is only available from another source “

![](https://github.com/beorel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/blob/main/images/Screenshot%20(98).png)

I went to browse for the error message and [came across](https://askubuntu.com/questions/990203/trying-to-install-apache-but-it-says-package-apache2-is-not-available)


I came across a solution there  which is: 
#### Step 1- sudo apt update && sudo apt upgrade 
#### Step 2- sudo apt install apache2

When i ran the **step 1 - sudo apt update && sudo apt upgrade**, i came across some pop out messages that required me to **click OK**, it is shown in the images

![](https://github.com/beorel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/blob/main/images/Screenshot%20(100).png)

![](https://github.com/beorel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/blob/main/images/Screenshot%20(101).png)

After which It took a while to complete the installation it stopped at 99% after over 20minutes, i reloaded the page and started my commands from the beginning
#### step 1 - Update a list of packages in package manager
*sudo apt update*
 
#### step 2- Run apache2 package installation
*sudo apt install apache2*

#### step 3- To verify that apache2 is running as a Service in our OS, use following command
*sudo systemctl status apache2*

If it is green and running, then you did everything correctly – you have just launched your first Web Server in the Clouds!
 
![](https://github.com/beorel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/blob/main/images/Screenshot%20(106).png)

Before we can receive any traffic by our Web Server, we need to open TCP port 80 which is the default port that web browsers use to access web pages on the Internet.

As we know, we have TCP port 22 open by default on our EC2 machine to access it via SSH, so we need to add a rule to EC2 configuration to open inbound connection through port 80:

Now it is time for us to test how our Apache HTTP server can respond to requests from the Internet.

Open a web browser of your choice and try to access following url **http://Public-IP-Address:80**

For example: 

![](https://github.com/beorel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/blob/main/images/Screenshot%20(106).png)

The IP address on this server is **3.85.233.120**

So just open a new tab on your browser and enter **http://3.85.233.120:80**

If you see the following page, then your web server is now correctly installed and accessible through your AWS.

![](https://github.com/beorel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/blob/main/images/Screenshot%20(116).png)

## 2. INSTALLING MYSQL
Now that you have a web server up and running, you need to install a Database Management System (DBMS) to be able to store and manage data for your site in a relational database. 

MySQL is a popular relational database management system used within PHP environments, so we will use it in our project.

Again, use ‘apt’ to acquire and install this software:

**$ sudo apt install mysql-server**

When prompted, confirm installation by typing Y, and then ENTER.

When the installation is finished, log in to the MySQL console by typing:

**$ sudo mysql**

This will connect to the MySQL server as the administrative database user root, which is inferred by the use of sudo when running this command. You should see output like this:

**Welcome to the MySQL monitor.  Commands end with; or \g.**

**Your MySQL connection id is 11**

**Server version: 8.0.22-0ubuntu0.20.04.3 (Ubuntu)**
 
**Copyright (c) 2000, 2020, Oracle and/or its affiliates. All rights reserved.**
 
**Oracle is a registered trademark of Oracle Corporation and/or its**
**affiliates. Other names may be trademarks of their respective**
**owners.**
 
**Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.**
 
**mysql>**

using mysql_native_password as default authentication method. We’re defining this user’s password as PassWord.1.

**ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';** 
 
Remember the password is : **PassWord.1**

Exit the MySQL shell with:

**mysql>** **exit**
 
 
Start the interactive script by running:

**$ sudo mysql_secure_installation**

When you’re finished, test if you’re able to log in to the MySQL console by typing:

**$ sudo mysql -p**

Notice the -p flag in this command, which will prompt you for the password used after changing the root user password.

Remember the password is : **PassWord.1**

To exit the MySQL console, type:

**mysql>** **exit**
 
Your MySQL server is now installed and secured. 
 
## 3 — INSTALLING PHP

To install these 3 packages at once, run:

**sudo apt install php libapache2-mod-php php-mysql**

Once the installation is finished, you can run the following command to confirm your PHP version:

**php -v**

You should see

![](https://github.com/beorel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/blob/main/images/Screenshot%20(110).png)

At this point, your LAMP stack is completely installed and fully operational.
- Linux (Ubuntu)
- Apache HTTP Server
- MySQL
- PHP

## 4 — CREATING A VIRTUAL HOST FOR YOUR WEBSITE USING APACHE

In this project, you will set up a domain called projectlamp, but you can replace this with any domain of your choice.

Create the directory for projectlamp using ‘mkdir’ command as follows:

**sudo mkdir /var/www/projectlamp**

Next, assign ownership of the directory with your current system user:

**sudo chown -R $USER:$USER /var/www/projectlamp**

Then, create and open a new configuration file in Apache’s sites-available directory using your preferred command-line editor. 

Here, we’ll be using vi or vim (They are the same by the way):

**sudo vi /etc/apache2/sites-available/projectlamp.conf**

This will create a new blank file. Paste in the following bare-bones configuration by hitting on i on the keyboard to enter the insert mode, and paste the text:

> 
> **<** **VirtualHost *:80** **>**
>
>>**ServerName projectlamp**
>>
>>**ServerAlias www.projectlamp**
>> 
>>**ServerAdmin webmaster@localhost**
>>
>>**DocumentRoot /var/www/projectlamp**
>>
>>**ErrorLog ${APACHE_LOG_DIR}/error.log**
>>
>>**CustomLog ${APACHE_LOG_DIR}/access.log combined**
>
>**</VirtualHost** **>**

To save and close the file, simply follow the steps below:

Hit the esc button on the keyboard

Type:

Type **wq** w for write and q for quit

Hit **ENTER** to save the file

You can use the ls command to show the new file in the sites-available directory

**sudo ls /etc/apache2/sites-available**

You will see something like this;

**000-default.conf default-ssl.conf  projectlamp.conf**

With this VirtualHost configuration, we’re telling Apache to serve projectlamp using /var/www/projectlampl as its web root directory. 

If you would like to test Apache without a domain name, you can remove or comment out the options ServerName and ServerAlias by adding a # character in the beginning of each option’s lines. 

Adding the # character there will tell the program to skip processing the instructions on those lines.

You can now use a2ensite command to enable the new virtual host:

**sudo a2ensite projectlamp**

You might want to disable the default website that comes installed with Apache. 

This is required if you’re not using a custom domain name, because in this case Apache’s default configuration would overwrite your virtual host. 

To disable Apache’s default website use a2dissite command, type:

**sudo a2dissite 000-default** 

To make sure your configuration file doesn’t contain syntax errors, run:

**sudo apache2ctl configtest**

Finally, reload Apache so these changes take effect:

**sudo systemctl reload apache2**

Your new website is now active, but the web root /var/www/projectlamp is still empty. 

Create an index.html file in that location so that we can test that the virtual host works as expected:

**Note:** pick everything at once, don't break the text.
> sudo echo 'Hello LAMP from hostname' $(curl -s http://169.254.169.254/latest/meta-data/public-hostname) 'with public IP' $(curl -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html

Now go to your browser and try to open your website URL using IP address:

*http://<Public-IP-Address>:80*

You should see this:

![](https://github.com/beorel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/blob/main/images/Screenshot%20(111).png)

## 5 — ENABLE PHP ON THE WEBSITE

With the default DirectoryIndex settings on Apache, a file named index.html will always take precedence over an index.php file. 
 
This is useful for setting up maintenance pages in PHP applications, by creating a temporary index.html file containing an informative message to visitors. 
 
Because this page will take precedence over the index.php page, it will then become the landing page for the application. 
 
Once maintenance is over, the index.html is renamed or removed from the document root, bringing back the regular application page.

In case you want to change this behavior, you’ll need to edit the /etc/apache2/mods-enabled/dir.conf file and change the order in which the index.php file is listed within the DirectoryIndex directive:

**sudo vim /etc/apache2/mods-enabled/dir.conf**
 
Press i to insert the text
<IfModule mod_dir.c>
        #Change this:
        #DirectoryIndex index.html index.cgi index.pl index.php index.xhtml index.htm
        #To this:
        DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
</IfModule>
After saving and closing the file, you will need to reload Apache so the changes take effect:
sudo systemctl reload apache2
Finally, we will create a PHP script to test that PHP is correctly installed and configured on your server.
Now that you have a custom location to host your website’s files and folders, we’ll create a PHP test script to confirm that Apache is able to handle and process requests for PHP files.
Create a new file named index.php inside your custom web root folder:
vim /var/www/projectlamp/index.php
This will open a blank file. Add the following text, which is valid PHP code, inside the file:
<?php
phpinfo();
When you are finished, save and close the file, refresh the page and you will see a page similar to this:

![](https://github.com/beorel/WEB-STACK-IMPLEMENTATION-LAMP-STACK-IN-AWS/blob/main/images/Screenshot%20(112).png)
