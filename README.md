# lamp-deployment-aws

This repository contains a step by step guide for deploying a LAMP (Linux, Apache, MySQL, PHP) stack on Amazon Web Services (AWS)

Requirements:

- An AWS account to create and manage the necessary resources for LAMP stack,including EC2 instance for hosting and RDS for managing mysql database.

- Terminal or command-line interface:to set up and manage the LAMP stack on my server. It's where I’ll handle everything from installing Apache and MySQL to configuring PHP, making sure everything works seamlessly.

- An SSH key pair for EC2 access which would be generated during EC2 instance setup and used to manage and configure my server remotely.

  **STEP 1 :  LAUNCH EC2 INSTANCE**
  
   Having our AWS account ready,navigate to AWS management console then:
  
i.   Firstly we're going to launch EC2 instance.An EC2 instance on AWS is a virtual server that provides scalable computing power, allowing us to run applications or services in the cloud.Navigate to EC2 
         dashboard and click on Launch Instance.
  
ii.  Name the instance,I'll nam e this 'odianatotheworld'
  
iii.  Select Amazon Ubuntu 24.04 LTS HVM as the AMI.

![image](https://github.com/user-attachments/assets/efad3d50-ec20-4828-a0d3-b35a61f2e5da)

  
iv.   Choose an instance type,T3 micro if on free tier like me

![image](https://github.com/user-attachments/assets/b542a830-6ba9-4e4b-8794-b9ddfae51d9f)

  
v.   Create or select a key pair if you already got one for SSH access.A key pair in SSH includes our *private key* (kept secret on our machine) and our *public key* (stored on the server). They work 
     together to securely authenticate and establish a connection between our local machine and the remote server and then donwload the .pem file aftwerwards(take note of where you're storing this file as 
      you'll need soon)

![image](https://github.com/user-attachments/assets/0d84cf44-0d74-4c8c-828e-f73dd9ccd7d7)


  
vi.  Now let's configure a Security Group with these rules:

 HTTP (port 80):Handles our regular web traffic, allowing browsers to connect to our websites without encryption.
          
HTTPS (port 443) : Manages our secure web traffic, encrypting data to ensure secure communication between our server and browser.

SSH (port 22) :  Used for secure remote access to our server, allowing us to manage and configure it over an encrypted connection.

vii.  Launch the instance:  — and boom! Our virtual server is born.

**STEP 2 :  CONNECT OUR EC2 INSTANCE**

i.Now let's dash to our command line interface,I'll be using the Ubuntu terminal.

 ii. On your AWS EC2 instance tab,click on connect that leads us to this

 ![image](https://github.com/user-attachments/assets/fdd62485-ce9d-4b02-b0a4-8bfd72127b1f)

               
iii. We'll be connecting via ssh so copy the ssh command

            
                          ssh -i your-key.pem ubuntu@<your-ec2-public-ip>



![image](https://github.com/user-attachments/assets/4cb7a4da-d5ad-442e-8547-567d91e6d2de)


*  iv. Now navigate to the directory where your .pem file is stored using the defined path in your case and paste the command


![image](https://github.com/user-attachments/assets/309d0473-3656-4bda-acd2-9d11944a03d6)

  
**STEP 3  :  UPDATE THE SYSTEM**

   Now we're going to update our system.Why?,yes we update our system to ensure security, stability, and compatibility by installing the latest patches and software versions, which prevents 
  vulnerabilities and ensures smooth operations.

              sudo apt update
              sudo apt upgrade -y


![image](https://github.com/user-attachments/assets/b4ffd5b5-62c0-44e6-9f78-c5d6ec628802)

  
**STEP 4  :   APACHE INSTALLATION**

   Apache is responsible for handling HTTP requests so that our site can be accessible.

   i. Install apache :

            sudo apt install apache2 -y

I already got mine installed 
![image](https://github.com/user-attachments/assets/fde600a1-80be-410f-a013-9b50625ee2c1)



   ii. Now let's get it running by starting and enabling it by:

            sudo systemctl start apache2

            sudo systemctl enable apache2




![image](https://github.com/user-attachments/assets/9d29170b-a760-467a-a988-98c842b06849)

   iii. We can verify Apache is running by :

         sudo systemctl status apache2


![image](https://github.com/user-attachments/assets/443bb56c-b9e4-4d19-913f-5c0e1550de94)

   iv. Let's go to our browser to get a view by using the EC2 public IPV4 IP.

   ![image](https://github.com/user-attachments/assets/479c15f5-8f9c-4e79-851e-8b02248f3abf)


   

**STEP 5 :       MYSQL INSTALLATION**

   MySql is our database in this lamp stack now we get it onboard by

         sudo apt install mysql-server -y

 ![image](https://github.com/user-attachments/assets/d1df78ae-d025-4495-b75f-6bbcad81c32c)

ii.  Now verify the installation by typing 

                   

iii. Now let's login by

         sudo mysql
           
iv. In mycase, I already have setup MySql with a Password 

 ![image](https://github.com/user-attachments/assets/b0e1b401-aa9c-496a-a166-053b7c6b3418)
           


v. It's advisable to run a security script that'a pre-inatalled MySql this script will remove some insecure default settings and and lockdown access to our database system.prior to that we'll set for the root user using mysql_native_password as default authentication method let's use 'PassWord.1' as our password by

            ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'PassWord.1';
   

this will ask for validation as seen and we answer yes to the query

vi. we setup a validate password plugin and answer yes to the questions

vii. Let's test our login with the password which will request for a password to login

            sudo mysql -p

vii. Then we gracefully exit... like a ninja disappearing into the shadows, with a simple 'exit'

![image](https://github.com/user-attachments/assets/4d5b9439-c2c6-4686-b1cb-f112c4dc374d)


It's the Bye for me 

**STEP 6 : PHP INSTALLATION **

i. To install PHP for ubuntu, we use the command

         sudo apt install php libapache2-mod-php php-mysql php-cli -y

![image](https://github.com/user-attachments/assets/4eca6aad-8cbb-4e26-9f39-30423f35df5e)




ii. Check php version

         php -v

   ![image](https://github.com/user-attachments/assets/ad6aadd2-76e7-4af9-b819-b51cb7d40c65)
      

**STEP 7 : VIRTUAL HOST SETUP **

Having that Apache  on Ubuntu has one server block enabled by default and is configured to serve documents from the var/www/html

i.  We create the directory    

        sudo mkdir /var/www/lampproject

ii. Then we asign ownwrship using the command below

        sudo chown -R $USER:$USER /var/www/lampproject

iii. Next we create and script a new new configuration file in Apache's sites-available directory.Let's use vim text editor in this case

               sudo vim /etc/apache2/sites-available/lampproject.conf

iv.   Now that our file is open to input,press I which stands for Insert and add the following 

              <VirtualHost *:80>
    ServerName lampproject
    ServerAlias www.lampproject
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/lampproject
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
    </VirtualHost>


![image](https://github.com/user-attachments/assets/349bffcc-4efc-4ce1-8a93-623c27c57561)


v.   Then esc to exit INSERT mode and :wq to write our file and quit the text editor.

vi.   We can list our files by using the ls command


        sudo ls /etc/apache2/sites-available


![image](https://github.com/user-attachments/assets/35f6d481-53f2-465b-b43a-4fb89867aa62)


vii.   Now we're going toenable our site using the command

viii.   But then we're going to disable default website so it doesn't override  our virtual host

                      sudo a2dissite 000-default

![image](https://github.com/user-attachments/assets/7fa65cc6-7adf-4a61-ba64-7ca76f6e87a8)


ix.    Now to ensure that our file doesn't contain errors,we check by

            sudo apachectl configtest

x.     Then we reload our apache server so our to effect the changes made.

            zsudo systemctl reload apache2

   ![image](https://github.com/user-attachments/assets/5257c0ef-4186-4222-b5b7-040f4c1401ff)

xi.    Let's create an index.html file to display on our server 

        sudo echo 'Hello LAMP from hostname' $(TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` && curl -H "X-aws-ec2-metada-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/public-hostname) 'With public IP' $(TOKEN=`curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600"` && curl -H "X-aws-ec2-metadata-token: $TOKEN" -s http://169.254.169.254/latest/meta-data/public-ipv4) > /var/www/projectlamp/index.html
          

xii.    Now let's access our public ip address.

![image](https://github.com/user-attachments/assets/7caf28a0-e987-4404-bf2e-066d51938bde)

**STEP 8 : PHP SETUP ON SERVER **

It's known that based on the default settings,an index.php html file will always override any other file including the index.html.Now to change this so as to enable our index.php file on our server.

i. We find our way to and change the order in the DirectiveIndex so that our index.php  


        sudo vim /etc/apache2/mods-enabled/dir.conf

        DirectoryIndex index.php  index.cgi index.pl index.html index.xhtml index.htm
~                                                                               
   ![image](https://github.com/user-attachments/assets/f529cf16-99fd-4b0c-a7ec-9fb332c796c0)

   ![image](https://github.com/user-attachments/assets/39a315fb-e2f2-4a4c-a4b4-8d18ab07c019)


ii. After which save the file and exit

iii.  Now we reload our server so the changes can take effect

iv.    Then create a new index.php file in our /var/wwww/project lamp folder

v.    Finally,we add this to the file.

            <?php
             phpinfo();

vi.    Save file and reload server.

               sudo systemctl reload apache2

![image](https://github.com/user-attachments/assets/b8d8264e-f799-45b4-8d05-dbc24eb998c9)

Here you go!

![image](https://github.com/user-attachments/assets/9ae9b1a9-bdce-41af-a873-0c3038e3960e)


To conclude,it's advisable to remove the file due to the sensitive information . We'll do that by

             sudo rm /var/www/your_domain/info.php


no worries,our index.php can always be accessed anytime we need to look it up.

#Cheers









                        
  
  
                 
