# Before the workshop: preparing your machine

## Admin privileges

For our workshops, ideally you have a laptop over which you have administrative privileges, meaning you can install software without requiring special permission.  

### Virtual machine software


## Windows

Windows 10

## Your own AWS account

### 1.) Log into AWS

First off, you'll need to go to https://devopsgirls.signin.aws.amazon.com/console using your browser. Log in with your supplied account credentials.

![Instance Logins][1-1-1-awslogin]

This will take you to the AWS Console.

### 1.B) Make sure you're in the right region

Then, we'll need to make sure we're working somewhere close. On the top-right side, make sure you select *Sydney*.

![Region][1-1-0-region]

### 2.) Go into the EC2 section

If you click on **Services**, you should see something similar to below. Because Amazon has a lot of services, you might want to type out *EC2* on the search box. Click on the link afterwards.

![DevOpsGirls-1-1-2][1-1-2-gotoec2]

### 3.) Create your key pair

The first thing we'll need to do is to create a key pair. This will be the key file that you will use to login to your instance. On the left-hand section, go to **Key Pair**, then click on **Create Key Pair**.

![Create key pair][1-1-3-keypair]

Make sure you name this key pair accordingly. You're going to need to remember it for later.

### 4.) Create your EC2 Instance

The next thing we'll need to do is create your *EC2 Instance*. To do so, we'll need to go to the left-hand side of the panel, and choose "Instances". Click on *"Launch Instance"*.

![Launch instance][1-1-4-launchinstance]

### 5.) Choose an AMI

An AMI is essentially the *operating system* that you'll have on your instance. Have a look around - there's a ton of pre-baked AMIs that you can use, which range from Windows, to Linux, to more specialized systems.

![Select your AMI][1-1-4-ami]

For this lesson, we're just going to pick the first one: called an *Amazon Linux AMI*

### 6.) Choose your instance type

When you think of instance types, think of it as asking the question: *"How big do I want my servers to be?"*

![instancetype][1-1-5-instancetype]

Note that the per-hour pricing varies with each instance type, and that some of them are suited for very specific tasks. For this example, we're going to choose a small size: a *t2.micro*

### 7.) Take a look at instance details

We're not going to change anything in this section - but these are additional networking and authorization details that you can specify when creating an instance. Where are you going to install it? Will it be private, or public? How does it handle being shut down?

![Instance details][1-1-6-instancedeets]

For the moment, we're going to be picking the defaults. Click on **"Add Storage"** afterwards.

### 8.) Specify your disks/storage

In the same way that any computer has disks to store the data in, you'll also need ones for your AWS instance. We can change the size, the type, and how fast it will be.

![Disks][1-1-7-instancestor]

Again, we're sticking with the defaults, so click on "**Next: Add Tags**".


### 9.) Tagging your instance

Tagging your instance is important - think of it as a way to identify your instance. Different companies tag their instances differently - and you can have up to 10 tags per instance!

![Instancetags][1-1-8-instancetags]

For this example, we're going to tag your instance with a *Name*. Put *Name* in the box below *Key*; put any name you want on the *Value*. This will be the name of your instance when you see it on a list later.


### 10.) Setting security groups

You'll need to set security groups, so that you can allow certain traffic towards your instance. Because we want to put a website on this instance, we'll need to add a HTTP rule that allows any traffic from anywhere to go to it. See as follows:

![Secgroups][1-1-9-secgroups]

You also have the option to specify *"My IP"* in the SSH section, so that only you can access the management traffic for your instance.

### 11.) Using the keypair you made previously

Finally, we get to the part where you have to choose the key file that you want to access your instance with. Remember the key pair you made earlier? Select *"Choose an existing keypair"* and select the name of the keypair you made previously. Now, click on **Launch Instance**.

![Keypair][1-1-10-keypair]

### 12.) Launching your instance

Once your instance launches, you'll see a window that shows your instance IDs. There will also be details below of how you should connect to your instance. You can follow these, or you can click on the next few links.

![Launching][1-1-11-launch]

### 13.) Seeing your instance properties

If you click on your instance ID, you will be able to see it from the list of EC2 instances. At the bottom window, you're going to see a list if your instance properties - what it's called, what the IP addresses are, and where it lives. You're going to want to look for the section that says **Public IP**.

![Properties][1-1-12-properties]


## Logging into your instance

### 14.) Login to your instance

Make note of the IP address you saw in your instance properties. Use the following guides depending on what operating system you have:

 - [If you are using a Mac](https://github.com/DevOpsGirls/devopsgirls-bootcamp/blob/master/8-1-SSH-from-Mac.md)
 - [If you are using Windows](https://github.com/DevOpsGirls/devopsgirls-bootcamp/blob/master/8-2-SSH-from-Windows.md)


## Installing Wordpress

### 15.) Try to access your instance using your browser

Now that you're logged in, try using your browser to access your instance. It will probably have an error - this is expected. This is because you haven't enabled or installed any services yet.

![Image][1-1-13-blankbrowser]


### 16.) Install services

Using yum, you will install the dependencies you need to get a basic Wordpress blog up and running.

Yum is used to install or update rpm packages (refers to the .rpm file format, files in the .rpm file format, software packaged in such files, and the package manager program itself. RPM is for Red Hat Linux distributions), the main benefit of using yum is that it also installs or upgrades any package dependencies.

Sudo allows to run with elevated privileges and is required for administrative tasks

```
sudo yum install -y php php-mysql mysql mysql-server httpd
```

### 17.) Download, then unpack wordpress:

xvfz is a method used for extracting the zip files - "extract verbose file zip" - which extracts the zip file showing the details of the extract

```
wget "https://wordpress.org/latest.tar.gz"
sudo tar xvfz latest.tar.gz -C /var/www/html/ --strip-components=1 wordpress
sudo chown -R apache /var/www/html/
```
-C changes the directory so the extracted files can be copied to the folder
chown -R – is used to change the owner and group of files, directories and links (the default owner belongs to the user that created it). The basic syntax for using chown to change owners is

              "chown [options] new_owner foldername"

–R is an option and is used to operate on a filesystem recursively

### 18.) Start your services:

```
sudo service httpd start
sudo service mysqld start
```

### 19.) Login to MySQL and create users:

Login to MySQL using the following command. When it prompts you for a password, leave it blank.

```
mysql -u root -p
```


### 20.) Create database and permissions:

Now it's time to create your database and permissions. Make sure you replace 'mypassword' with your ideal password.

```
CREATE DATABASE mywordpress;
GRANT ALL PRIVILEGES ON mywordpress.* to 'mywordpress'@'localhost' identified by 'mypassword';
FLUSH PRIVILEGES;

```

It should look like this:

![Image][1-1-14-mysql]


### 21.) Try to access your instance using your browser

Now that the services have started, you will see something like an install page. Something like this:

![Image][1-1-15-wordpresssetup]

Apache (httpd) is essentially the system service that serves your web files (the contents of your Wordpress directory).


### 22.) Finishing up

Fill in the installation with all the details you declared.

![Image][1-1-16-wpsqlsetup]

If everything went well, you'll have a complete Wordpress installation!

![Image][1-1-17-wpfinished]





[1-1-0-region]: https://raw.githubusercontent.com/DevOpsGirls/devopsgirls-bootcamp/master/images/1-1-EC2/1-1-0-region.png
[1-1-1-awslogin]: https://raw.githubusercontent.com/DevOpsGirls/devopsgirls-bootcamp/master/images/1-1-EC2/1-1-1-awslogin.png
[1-1-2-gotoec2]: https://raw.githubusercontent.com/DevOpsGirls/devopsgirls-bootcamp/master/images/1-1-EC2/1-1-2-gotoec2.png
[1-1-3-keypair]: https://raw.githubusercontent.com/DevOpsGirls/devopsgirls-bootcamp/master/images/1-1-EC2/1-1-3-keypair.png
[1-1-4-launchinstance]: https://raw.githubusercontent.com/DevOpsGirls/devopsgirls-bootcamp/master/images/1-1-EC2/1-1-4-launchinstance.png
[1-1-4-ami]: https://raw.githubusercontent.com/DevOpsGirls/devopsgirls-bootcamp/master/images/1-1-EC2/1-1-4-ami.png
[1-1-5-instancetype]: https://raw.githubusercontent.com/DevOpsGirls/devopsgirls-bootcamp/master/images/1-1-EC2/1-1-5-instancetype.png
[1-1-6-instancedeets]: https://raw.githubusercontent.com/DevOpsGirls/devopsgirls-bootcamp/master/images/1-1-EC2/1-1-6-instancedeets.png
[1-1-7-instancestor]: https://raw.githubusercontent.com/DevOpsGirls/devopsgirls-bootcamp/master/images/1-1-EC2/1-1-7-instancestor.png
[1-1-8-instancetags]: https://raw.githubusercontent.com/DevOpsGirls/devopsgirls-bootcamp/master/images/1-1-EC2/1-1-8-instancetags.png
[1-1-9-secgroups]: https://raw.githubusercontent.com/DevOpsGirls/devopsgirls-bootcamp/master/images/1-1-EC2/1-1-9-secgroups.png
[1-1-10-keypair]: https://raw.githubusercontent.com/DevOpsGirls/devopsgirls-bootcamp/master/images/1-1-EC2/1-1-10-keypair.png
[1-1-11-launch]: https://raw.githubusercontent.com/DevOpsGirls/devopsgirls-bootcamp/master/images/1-1-EC2/1-1-11-launch.png
[1-1-12-properties]: https://raw.githubusercontent.com/DevOpsGirls/devopsgirls-bootcamp/master/images/1-1-EC2/1-1-12-properties.png
[1-1-13-blankbrowser]: https://raw.githubusercontent.com/DevOpsGirls/devopsgirls-bootcamp/master/images/1-1-EC2/1-1-13-blankbrowser.png
[1-1-14-mysql]: https://raw.githubusercontent.com/DevOpsGirls/devopsgirls-bootcamp/master/images/1-1-EC2/1-1-14-mysql.png
[1-1-15-wordpresssetup]: https://raw.githubusercontent.com/DevOpsGirls/devopsgirls-bootcamp/master/images/1-1-EC2/1-1-15-wordpresssetup.png
[1-1-16-wpsqlsetup]: https://raw.githubusercontent.com/DevOpsGirls/devopsgirls-bootcamp/master/images/1-1-EC2/1-1-16-wpsqlsetup.png
[1-1-17-wpfinished]: https://raw.githubusercontent.com/DevOpsGirls/devopsgirls-bootcamp/master/images/1-1-EC2/1-1-17-wpfinished.png