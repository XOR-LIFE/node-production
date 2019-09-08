# **Take Your NodeJS Project to The Production Environment (VPS/Dedicated Server).**


This tutorial is based on **Ubuntu**, the steps are the same for whatever Linux distribution you are going to use but the commands might be different.
----------------------------------------------------------------------------------------

<br>
<br>

## Table of Contents:

* [Set up Your VPS](https://github.com/XOR-LIFE/node-production#set-up-your-vps)
  * [Install OpenSSH](https://github.com/XOR-LIFE/node-production#install-openssh)
  * [Enable SFTP](https://github.com/XOR-LIFE/node-production#enable-sftp)
  * [Install VNC](https://github.com/XOR-LIFE/node-production#install-vnc)
  * [Install Git](https://github.com/XOR-LIFE/node-production#install-git)
 
* [1- Install NodeJS and NPM](https://github.com/XOR-LIFE/node-production#1--install-nodejs-and-npm)
  * [Uninstall NodeJS and NPM](https://github.com/XOR-LIFE/node-production#uninstall-nodejs-and-npm)

* [2- Create a Node App With Express Server (Testing)](https://github.com/XOR-LIFE/node-production#2--create-a-node-app-with-express-server-testing)

* [3- Install PM2](https://github.com/XOR-LIFE/node-production#3--install-pm2)
  * [How to:-](https://github.com/XOR-LIFE/node-production#how-to-)
* [4- Install and Configure MongoDB](https://github.com/XOR-LIFE/node-production#4--install-and-configure-mongodb)
  * [Add Admin User For The First Time](https://github.com/XOR-LIFE/node-production#add-admin-user-for-the-first-time)
  * [Mongodb Configuration File](https://github.com/XOR-LIFE/node-production#mongodb-configuration-file)
  * [Enable MongoDB authentication](https://github.com/XOR-LIFE/node-production#enable-mongodb-authentication)
  * [Change MongoDB Default Port](https://github.com/XOR-LIFE/node-production#change-mongodb-default-port)
  * [Securing MongoDB from Injection Attacks](https://github.com/XOR-LIFE/node-production#securing-mongodb-from-injection-attacks)
  * [Uninstall MongoDB](https://github.com/XOR-LIFE/node-production#uninstall-mongodb)
* [5- Configure UFW and Add MongoDB Port to Rules](https://github.com/XOR-LIFE/node-production#5--configure-ufw-and-add-mongodb-port-to-rules)
  * [Allow External Access To MongoDB](https://github.com/XOR-LIFE/node-production#allow-external-access-to-mongodb)
* [6- Install Nginx](https://github.com/XOR-LIFE/node-production#6--install-nginx)
* [7- Adjust Your Node Application for Production](https://github.com/XOR-LIFE/node-production#7--adjust-your-node-application-for-production)
* [8- Configure NGINX](https://github.com/XOR-LIFE/node-production#8--configure-nginx)
  * [Configuration Notes](https://github.com/XOR-LIFE/node-production#configuration-notes)
  * [Hide Nginx Server Version](https://github.com/XOR-LIFE/node-production#hide-nginx-server-version)
  * [Setup NGINX As Reverse Proxy For Node Application](https://github.com/XOR-LIFE/node-production#setup-nginx-as-reverse-proxy-for-node-application)
  * [Running And Proxying Multiple Node Apps](https://github.com/XOR-LIFE/node-production#running-and-proxying-multiple-node-apps)
  * [Serving Error Pages](https://github.com/XOR-LIFE/node-production#serving-error-pages)
  * [Modifying Open File/Concurrent Connections Limit](https://github.com/XOR-LIFE/node-production#modifying-open-fileconcurrent-connections-limit)
  * [Enable Nginx Status Page](https://github.com/XOR-LIFE/node-production#enable-nginx-status-page)
  * [Nginx Better Configurations](https://github.com/XOR-LIFE/node-production#nginx-better-configurations)
  * [Enable Gzip Compression](https://github.com/XOR-LIFE/node-production#enable-gzip-compression)
  * [Serving Static Files](https://github.com/XOR-LIFE/node-production#serving-static-files)
  * [Configure HTTPS with Certbot](https://github.com/XOR-LIFE/node-production#configure-https-with-certbot)
  * [Redirect VPS-IP-Address To Domain](https://github.com/XOR-LIFE/node-production#redirect-vps-ip-address-to-domain)
  * [Enabling HTTP /2.0 Support And Set As Default Server](https://github.com/XOR-LIFE/node-production#enabling-http-20-support-and-set-as-default-server)
  * [Add Security Headers](https://github.com/XOR-LIFE/node-production#add-security-headers)
* [9- Canonical Livepatch](https://github.com/XOR-LIFE/node-production#9--canonical-livepatch)
* [10- Useful Readings](https://github.com/XOR-LIFE/node-production#10--useful-readings)
* [11- Checklist](https://github.com/XOR-LIFE/node-production#11--checklist)
* [12- Site Performance](https://github.com/XOR-LIFE/node-production#12--site-performance)
* [13- Securing VPS](https://github.com/XOR-LIFE/node-production#13--securing-vps)

<br>
<br>

## Set up Your VPS
----------------------------------------------------------------------------------------

### **Install OpenSSH**

Before we begin you need to make sure that you have an SSH connection so you can login to your VPS machine.

```
sudo apt update

sudo apt install openssh-server -y
```

Once the installation is completed, the SSH service will start automatically, type the following command to show the SSH server status
```
sudo systemctl status ssh
```

And you should now see **Active: active (running)**, Press `q` to get back to the command line prompt.

If not running enable the ssh server and start it as follows:
```
sudo systemctl enable ssh

sudo systemctl start ssh
```

You can now connect to your machine through SSH using, for example [PuTTY](https://www.putty.org)

<br>

### **Enable SFTP**

Secure File Transfer Protocol or **SFTP** is a network protocol that provides file access, file transfer, and file management over any reliable data stream.

SFTP has pretty much replaced legacy FTP as a file transfer protocol and is quickly replacing FTP/S. It provides all the functionality offered by these protocols, but more securely and more reliably, with easier configuration. There is basically no reason to use legacy protocols anymore.

SFTP also protects against password sniffing and man-in-the-middle attacks. It protects the integrity of the data using encryption and cryptographic hash functions and authenticates both the server and the user.

**SFTP uses the same port used by SSH** so you would need nothing more to install if you have already enabled SSH in your machine.

All that you need is just an application to connect to your machine through SFTP.

I recommend those two applications and you can use what you want.

* **FileZilla Client:** a cross-platform FTP, SFTP, and FTPS client with a vast list of features, which supports Windows, Mac OS X, Linux, and more.

Download from here: [FileZilla Client Download](https://filezilla-project.org/download.php?type=client)

* **Bitvise SSH Client (Windows Only):** Free and flexible SSH Client for Windows includes state of the art terminal emulation, graphical as well as command-line SFTP support, an FTP-to-SFTP bridge, powerful tunneling features including dynamic port forwarding through an integrated proxy, and remote administration for our SSH Server.

Download from here: [Bitvise SSH Client Download](https://www.bitvise.com/ssh-client-download)

<br>

### **Install VNC**

Even though this is not necessary if you are going to interact with your machine through commands all the time (SSH), but you might need to connect to your machine through VNC for any reason and access the GUI version due to simplicity.

If you installed **Ubuntu Server** on your machine then having VNC is _Useless_, unless you are going to install a GUI/Desktop on your Ubuntu Server. And if you want so then I suggest going with [Lubuntu Core Server Desktop](https://linuxconfig.org/install-gui-on-ubuntu-server-18-04-bionic-beaver#h7-2-lubuntu-core-server-desktop)

<br>

**Which VNC application is better ??**

I've tried **All** VNC applications and I can tell you that **RealVNC** is the best VNC application with no doubt.

RealVNC consists of two products, Server and Viewer.

* RealVNC Server: Download to the remote computer you want to control (This is to be downloaded on your VPS machine).
* RealVNC Viewer: Download to the local computer or mobile device you want to control from.

<br>

**Now,** Let's start installing RealVNC.

1. Register a free account of RealVNC from the link below
```
https://manage.realvnc.com/en/
```

2. Download VNC Server from the link below and choose `DEB x64`
```
https://www.realvnc.com/en/connect/download/vnc/linux/
```

3. `cd` to the folder which RealVNC was download, It's `Downloads` in my case
```
cd Downloads
```

4. Install .deb package using `sudo dpkg –i`
```
sudo dpkg –i packagename.deb

sudo apt-get install -f
```

5. Run subscription wizard so you could log in with your home subscription account
```
vnclicensewiz
```
This will launch the wizard and you just need to log in with your RealVNC credentials. You will also need root password when requested.

6. Start RealVNC and Enable it on boot
```
sudo systemctl start vncserver-x11-serviced

sudo systemctl enable vncserver-x11-serviced
```
**Installation is now finished.**

* You can now install RealVNC Viewer from the wide list of supported devices on your controller machine to access and controll your machine.
```
https://www.realvnc.com/en/connect/download/viewer/
```

**If you had the error "Cannot currently show the desktop", then click this link to know how to fix it. [Disable Wayland and enable Xorg display](https://linuxconfig.org/how-to-disable-wayland-and-enable-xorg-display-server-on-ubuntu-18-04-bionic-beaver-linux)**

<br>

### **Install Git**

Git is a de-facto standard for distributed version control systems and is used by the majority of developers nowadays. It allows you to keep track of your code changes, revert to previous stages, create branches and to collaborate with your fellow developers.

1. Add PPA to source list
```
sudo add-apt-repository ppa:git-core/ppa
```

2. Update package list and install Git
```
sudo apt update

sudo apt install git  -y
```

3. Validate installation and check for version
```
git --version
```

----------------------------------------------------------------------------------------

<br>
<br>

## 1- Install NodeJS and NPM
----------------------------------------------------------------------------------------

There are several ways to install node.js

 **1. Ubuntu Repository:**
Not recommended at all because Ubuntu sucks at keeping their packages updated.

 **2. NodeSource Repository:**
Very Recommended, Easy to install and Easy to update.
 
 **3. NVM (Node Version Manager):**
Used if you are going to install multiple node versions on your machine.

<br>

Based on the previous methods, I'll go with the second option.

<br>

1. Update your Ubuntu:
```
sudo apt update && sudo apt upgrade -y
```

2. Install important packages for node and npm:
```
sudo apt install gcc g++ make build-essential curl -y
```

3. Install node.js and npm from NodeSource Repository:
```
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -

sudo apt update

sudo apt-get install -y nodejs
```

Above command will install node.js version 12, if you want a previous version just replace 12 with desired node version e.g. (11, 10, 8)

to check both node.js and npm versions, simply use.
```
node -v

npm -v
```

## Uninstall NodeJS and NPM

**To uninstall node.js and npm simply use:**
```
sudo apt remove nodejs npm
```

this will uninstall node and npm but will keep your configuration files.

<br>

**To also delete your configuration files, use:**
```
sudo apt purge nodejs npm
```
----------------------------------------------------------------------------------------

<br>
<br>

## 2- Create a Node App With Express Server (Testing)
----------------------------------------------------------------------------------------

* Since you’ve already installed Node.js, create a directory to hold your application, and make it your working directory:
```
mkdir testapp

cd testapp
```

* Use the `npm init` command to create a package.json file for your application:
```
npm init
```
This command prompts you for a number of things, such as the name and version of your application. For now, you can simply hit 'RETURN/ENTER' (In Your Keyboard) to accept the defaults for all of them.

* Now install Express in the `testapp` directory and save it in the dependencies list:
```
npm install express --save
```
* In the `testapp` directory, create a file named `index.js` and copy in the code below:
```
const express = require('express')
const app = express()
const port = 3000

app.get('/', (req, res) => res.send('Hello World!'))

app.listen(port, () => console.log(`Example app listening on port ${port}!`))
```

* Run the app with the following command:
```
node index.js
```
Then, load http://localhost:3000/ in a browser to see the output.

You can of course now navigate to your http://VPS-IP-Address:3000 from an outside machine and you would see the **'Hello World!'** message, The example above is actually a working server, but we are just making sure we are making the right footsteps.

----------------------------------------------------------------------------------------

<br>
<br>

## 3- Install PM2
----------------------------------------------------------------------------------------

**PM2 identifies itself as "PM2 is a production process manager for Node.js applications with a built-in load balancer. It allows you to keep applications alive forever, to reload them without downtime and to facilitate common system admin tasks."**

The main feature of PM2 is to start your node.js app if your node app suffered any crash or if your VPS suffered an unexpected shutdown and restarted, therefore, you won't have to worry about manually starting your node app again.

Additionally, PM2 is more than that. It's capable of running several node apps at the same time with independent management for each one of them, Also it allows you to duplicate the processes to use all the cores of your CPUs.


### Install PM2
```
sudo npm install -g pm2

pm2 completion install

pm2 -v
```

<br>

### How to:-

<br>

#### Start and Daemonize any application:

```
pm2 start app.js  – Starts a specific application
```

Your app is now daemonized, monitored, forever alive, auto-restarting after crashes and machine restarts – all with one simple command

You can also Start your application and give it a name in the PM2 list if you run several applications and "index.js/app.js" might confuse you

```
pm2 start app.js --name="Shop App"
```

![App starting](https://pm2.io/_nuxt/img/04fdf99.png)

<br>

#### List/Manage applications:

```
pm2 list  – Show a list of all applications
```

![Process listing](https://github.com/unitech/pm2/raw/master/pres/pm2-list.png)

Managing apps is straightforward:

```
pm2 stop     <app_name|id|'all'|json_conf>  – Stops applications
pm2 restart  <app_name|id|'all'|json_conf>  – Restarts applications
pm2 delete   <app_name|id|'all'|json_conf>  – Delete from PM2 process list 
```

To have more details on a specific application:

```
pm2 describe <id|app_name>  - Display all informations about a specific process
```

[More about Application Management](https://pm2.io/doc/en/runtime/guide/process-management/?utm_source=github)

<br>

#### Cluster Mode: Node.js Load Balancing & Zero Downtime Reload

As Node.js is monothreaded, PM2 can duplicate the processes to use all the cores for your CPUs. The Cluster mode is a special mode when starting a Node.js application, it starts multiple processes and load-balance HTTP/TCP/UDP queries between them. This increase overall performance (by a factor of x10 on 16 cores machines) and reliability (faster socket re-balancing in case of unhandled errors).

Starting a Node.js application in cluster mode that will leverage all CPUs available:

```
pm2 start app.js -i <processes>
```

`<processes>` can be `'max'`, `-1` (all cpu cores minus 1) or a specified number of instances to start.

![](https://pm2.io/_nuxt/img/1f13170.png)

**Zero Downtime Reload**

Hot Reload allows to update an application without any downtime:

```
pm2 reload <all|app_name>  – Reloads the app configuration (this comes in handy when you modify your application’s environment variables)
```

<br>

#### Terminal Based Monitoring:

To monitor logs, custom metrics and information about your application’s health. For example, you’ll see CPU utilization, memory usage, requests/minute, and more!

```
pm2 monit
```

![Monit](https://github.com/Unitech/pm2/raw/master/pres/pm2-monit.png)

<br>

#### Log Management:

PM2 has built-in log management. It aggregates log data from all of your applications and writes it to a single source for viewing. You can even tail the logs in real-time to see what’s going on behind the scenes with your application. Log Management from PM2 comes with log rotation as well, which is important, especially if you’re application is outputting verbose logs on a frequent basis.



```
pm2 logs – Outputs logs from all running applications

pm2 logs app – Outputs logs from only the “app” application

pm2 flush – Flushes all log data, freeing up disk space
```

Remember, the most important thing to do is to enable log rotation. Doing so will split one giant log file into many smaller files that are more manageable for PM2. you can do so through 'pm2-logrotate' module (See PM2 Modules Section Below).


[More about log management](https://pm2.io/doc/en/runtime/guide/log-management/)

<br>

#### Startup Hooks Generation

PM2 can generate and configure a Startup Script to keep PM2 and your processes alive at every server restart.

Init Systems Supported: **systemd**, **upstart**, **launchd**, **rc.d**

```
# Generate Startup Script
pm2 startup

# Freeze your process list across server restart
pm2 save

# Remove Startup Script
pm2 unstartup
```

[More about Startup Hooks](https://pm2.io/doc/en/runtime/guide/startup-hook/)   **MUST READ**

<br>

#### PM2 Modules

PM2 embeds a simple and powerful module system. Installing a module is straightforward:

```
pm2 install <module_name>
```

Here are some PM2 compatible modules (standalone Node.js applications managed by PM2):

[**pm2-logrotate**](https://www.npmjs.com/package/pm2-logrotate) automatically rotate logs and limit logs size<br/>
[**pm2-server-monit**](https://www.npmjs.com/package/pm2-server-monit) monitor the current server with more than 20+ metrics and 8 actions<br/>

<br>

#### Updating PM2

```
# Install latest PM2 version
sudo npm install pm2@latest -g

# Save process list, exit old PM2 & restore all processes
pm2 update
```

*PM2 updates are seamless*

<br>

### PM2+ Monitoring

![](https://raw.githubusercontent.com/Unitech/pm2/master/pres/pm2-ls-multi.png)

If you manage your NodeJS app with PM2, **PM2+** makes it easy to monitor and manage apps across servers. Feel free to try it:

[Discover the monitoring dashboard for PM2](https://app.pm2.io/)


### More about PM2

- [Zero Downtime Reload](https://pm2.io/doc/en/runtime/guide/load-balancing/)
- [Watch File & Restart](https://pm2.io/doc/en/runtime/features/watch-restart/)
- [Restart Strategies](https://pm2.io/doc/en/runtime/features/restart-strategies/)
- [Startup Script Generation](https://pm2.io/doc/en/runtime/guide/startup-hook/)
- [Memory Limit Auto Reload](https://pm2.io/doc/en/runtime/features/memory-limit/)
- [Source Map Support](https://pm2.io/doc/en/runtime/features/javascript-source-maps/)
- [Using PM2 API](https://pm2.io/doc/en/runtime/reference/pm2-programmatic/)
- [Graceful Shutdown](https://pm2.io/doc/en/runtime/best-practices/graceful-shutdown/)
- **[Full Documentation](https://pm2.io/doc/en/runtime/overview/)**

----------------------------------------------------------------------------------------

<br>
<br>

## 4- Install and Configure MongoDB
----------------------------------------------------------------------------------------

**MongoDB is a well-known NoSQL database that offers high performance, high availability and automatic scaling. It differs from RDBMS such as MySQL, PostgreSQL, and SQLite because it does not use SQL to set and retrieve data. MongoDB stores data in `documents` called BSON (binary representation of JSON with additional information). MongoDB is only available for the 64-bit long-term support Ubuntu release.**


### Step 1: Install MongoDB

There are two ways to install MongoDB

 **1. Ubuntu Repository:**
Again, Not recommended at all because Ubuntu sucks at keeping their packages updated.

 **2. MongoDB Repository:**
Latest Version, Very Recommended, Easy to install and Easy to update.
 
<br>

Based on the previous methods, I'll go with the second option.

<br>
 
**MongoDB Version:**
This tutorial installs MongoDB 4.x Community Edition on LTS Ubuntu Linux systems.


**Platform Support:**
MongoDB only provides packages for the following 64-bit LTS (long-term support) Ubuntu releases:

* 16.04 LTS (xenial)
* 18.04 LTS (bionic)

<br>

**IMPORTANT (MongoDB Official Statement):**

> The `mongodb-org` package is officially maintained and supported by MongoDB Inc. and kept up-to-date with the most recent MongoDB releases. This installation procedure uses the `mongodb-org` package.
> 
> The `mongodb` package provided by Ubuntu is **not** maintained by MongoDB Inc. and conflicts with the `mongodb-org` package. To check if Ubuntu’s `mongodb` package is installed on the system, run `sudo apt list --installed | grep mongodb`. You can use `sudo apt remove mongodb` and `sudo apt purge mongodb` to remove and purge the `mongodb` package before attempting this procedure.
> 

<br>

Alright Then, Let's start:

<br>

1. Import the public key used by the package management system:
```
wget -qO - https://www.mongodb.org/static/pgp/server-4.2.asc | sudo apt-key add -
```

2. Create a list file for MongoDB:

If you are unsure of what Ubuntu version you are running, open a terminal or shell on the host and execute `lsb_release -dc`

* For Ubuntu 18.04 (Bionic):
```
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list
```

* For Ubuntu 16.04 (Xenial)
```
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.2 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.2.list
```

3. Reload local package database:
```
sudo apt-get update
```

4. Install the MongoDB packages (Latest Version):
```
sudo apt-get install -y mongodb-org
```

5. Check for MongoDB version:
```
mongod --version
```

6. Start MongoDB and make it autostart at reboot:
```
sudo service mongod start

sudo systemctl enable mongod
```

7. Check that MongoDb is active and Running:

* Method 1:
```
sudo systemctl status mongod
```
You should get **Active: active (running)**

* Method 2:
```
mongo --eval 'db.runCommand({ connectionStatus: 1 })'
```
A value of `1` for the `ok` field indicates success.


 **How to Manage MongoDB Service**

* Stop MongoDB
`
sudo service mongod stop
`

* Restart MongoDB
`
sudo service mongod restart
`

* Disable MongoDB from autostart on boot
`
sudo systemctl disable mongod
`

<br>

### Step 2: Configure MongoDB

<br>

#### **Add Admin User For The First Time:**

1. Open MongoDB shell:
```
mongo
```

2. Switch to the database admin:
```
use admin
```

3. Create the root user:
```
db.createUser({user:"admin", pwd:"pass123", roles:[{role:"root", db:"admin"}]})
```
change `pass123` to the password of your choice

3. To list all available databases:
```
show dbs
```

4. To list available users:
```
use admin

show users
```

5. Exit from the MongoDB shell:
```
exit
```

<br>

#### **Mongodb Configuration File:**

The configuration file for MongoDB is located at` /etc/mongod.conf`, and is written in **YAML** format. Most of the settings are well commented within the file. Here are some outlined default options below:

* `dbPath` indicates where the database files will be stored (`/var/lib/mongodb` by default)
* `systemLog` specifies the various logging options, explained below:
  * `destination` tells MongoDB whether to store the log output as a file or syslog
  * `logAppend` specifies whether to append new entries to the end of an existing log when the daemon restarts (as opposed to creating a backup and starting a new log upon restarting)
  * `path` tells the daemon where to send its logging information (`/var/log/mongodb/mongod.log` by default)
* `net` specifies the various network options, explained below:
  * `port` is the port on which the MongoDB daemon will run
  * `bindIP` specifies the IP addresses MongoDB to which binds, so it can listen for connections from other applications

These are only a few basic configuration options that are set by default.

For more information on how to customize these and other values in your configuration file, refer to the [official MongoDB configuration tutorial](https://docs.mongodb.com/manual/reference/configuration-options/).

**Note: upon making any change to the MongoDB configuration file, you would need to restart the mongodb service so that changes would take effect.**

<br>

#### **Enable MongoDB authentication:**

1. Edit the mongodb service file `/etc/mongod.conf` with your editor.

The default configuration settings are sufficient for most users. However, for production environments, it is recommended to uncomment the security section and enable authorization as shown below:

In the `#security` section, remove the hash `#` sign in front of security to enable the section then add authorization role and set it to enabled.
```
security:
  authorization: enabled
```

**YAML does not support tab characters for indentation: use spaces instead.**

The authorization option enables [Role-Based Access Control (RBAC)](https://docs.mongodb.com/manual/core/authorization/) that regulates users access to database resources and operations. If this option is disabled each user will have access to all databases and perform any action.

2. Reload the systemd service:
```
sudo service mongod restart
```

3. Now check that authorization work by connecting to mongo shell and try to list databases
```
mongo
```

then try to list database
```
show dbs
```

if you got nothing as a result or connection failed that means that authorization now works successfully.

4. Now to connect to MongoDB shell you need to use this command:
```
mongo -u admin -p --authenticationDatabase admin
```

And you will be prompted to enter your password, after login type `exit`.

**Note: It is not recommended to enter your password on the command-line because it will be stored in the shell history file and can be viewed later on by an attacker, instead, use above command and enter your password inside mongo shell.**

<br>

#### **Change MongoDB Default Port:**

This is an optional thing to do, if you intend, You can specify mongod’s listening port by specifying it in the mongodb configuration file.

Open `/etc/mongod.conf` with your favorite code editor and search for the following lines:

```
net:
  port: 27017
```

Now change the port number to any other of your choice, save and reload `mongod`:
```
sudo service mongod restart
```

**Please take into account that the chosen port needs to be equal or greater than 1000 and must not be taken by any other service running in the same host. You can check if a certain port is already in use with the nc command:**
```
$ sudo nc -l 3000
nc: Address already in use

$ sudo nc -l 23456
Listening on [0.0.0.0] (family 0, port 23456)
^C
```

or you can list all open ports through the lsof command (i do this):
```
sudo lsof -n -P | grep LISTEN
```

<br>

#### **Securing MongoDB from Injection Attacks:**

All of the following MongoDB operations permit you to run arbitrary JavaScript expressions directly on the server:

* $where
* mapReduce
* group

These methods can be really convenient, but they pose a huge security risk to your database integrity if your application does not sanitize and escape user-provided values properly.

Indeed, **you can express most queries in MongoDB without JavaScript**, so the most sensible option is to completely disable server-side Javascript.

**Open** `/etc/mongod.conf` with your favorite code editor and look for the security section:
```
security:
    authorization: enabled
```

Make sure to add the following line inside the security section:
```
    javascriptEnabled: false
```


<br>

## Uninstall MongoDB

**Warning: All configuration and databases will be completely removed after this process. And it is irreversible, so ensure that all of your configuration and data is backed up before proceeding.**



**To Uninstall MongoDB, use:**
```
sudo service mongod stop

sudo apt-get purge mongodb-org*

sudo rm -r /var/log/mongodb

sudo rm -r /var/lib/mongodb
```



**Finally:** 
* **[MongoDB User Credentials and Security Best Practices](https://medium.com/mongoaudit/mongodb-user-credentials-best-practices-9fa6de06bcc1)**
* **[MongoDB Production Notes](https://docs.mongodb.com/manual/administration/production-notes/)**
* **[MongoDB Security Checklist](https://docs.mongodb.com/manual/administration/security-checklist/)**

----------------------------------------------------------------------------------------

<br>
<br>

## 5- Configure UFW and Add MongoDB Port to Rules
----------------------------------------------------------------------------------------

**Uncomplicated Firewall (UFW), is a front-end to iptables. Its main goal is to make managing your firewall drop-dead simple and to provide an easy-to-use interface.**

Install UFW:

1. Install UFW:
```
sudo apt-get install ufw -y
```

2. Check UFW status:
```
sudo ufw status
```

And you should get the result `Status: inactive`

3. Ensure to allow SSH:
```
sudo ufw allow OpenSSH
```

4. **Allow ports 80, 443 for people to be able to access your website through HTTP and HTTPS (Very Important)**
```
sudo ufw allow http

sudo ufw allow https
```

5. If you use VNC to connect to your machine, make sure to also open its corresponding port (Its 5900 in my case, I use RealVNC):
```
sudo ufw allow 5900
```

6. Enable UFW, because it's probably inactive:
```
sudo ufw enable
```

7. Rerun the UFW status command:
```
sudo ufw status
```

which now should return the following:
```
Status: active

To                         Action      From
--                         ------      ----
80/tcp                     ALLOW       Anywhere
443/tcp                    ALLOW       Anywhere
OpenSSH                    ALLOW       Anywhere
5900                       ALLOW       Anywhere
80/tcp (v6)                ALLOW       Anywhere (v6)
443/tcp (v6)               ALLOW       Anywhere (v6)
OpenSSH (v6)               ALLOW       Anywhere (v6)
5900 (v6)                  ALLOW       Anywhere (v6)
```

<br>

**If you to deny traffic on a certain port (in this example, 111) you would only have to run:**
```
sudo ufw deny 111
```

<br>

### **Allow External Access To MongoDB**
 
Right now we have configured UFW to allow external access to our machine with Mongo port, but still, Mongo itself isn't configured for external access. even though the default port is open, the database server is currently listening on 127.0.0.1. And to permit remote connections, you must include a publicly-routed IP address for your server to `mongo.conf` file.

By default, MongoDB authorizes all logs from the local machine. There are no problems while the application is developing.

However, because you have to enable authentication, you might run into issues when the application is ready and you have to deploy it, So you need to configure it.

_**Note: You only allow external access of mongo if your node application is on another machine or if you want to access it through applications such as Robo 3T, but if your node application is running on the same machine where mongo is installed then you won't have to bind mongo IP.  Binding also could be on LAN scale or WAN scale, it all depends on where are your node and mongo installed.**_

<br>

1. Allow access to the default MongoDB port `27017`:
```
sudo ufw allow 27017
```

2. Check UFW status:
```
sudo ufw status
```

which returns
```
To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
27017                      ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
27017 (v6)                 ALLOW       Anywhere (v6)
```

3. Allow remote connections, to bind MongoDB to all network interfaces open the config file `/etc/mongod.conf`:
```
sudo nano /etc/mongod.conf
```

4. Edit `bindIP:` in `net:` section with setting below:
```
net:
  port: 27017
  bindIp: 127.0.0.1,your_vps_ip
```

A comma should be placed before adding the IP address you want to allow. Once that is done, Save the changes and exit the editor.

More on bindIp; [MongoDB bindIp Configurations](https://docs.mongodb.com/manual/reference/configuration-options/#net.bindIp)

5. Restart mongodb:
```
sudo service mongod restart
```

6. Bind to all (Step 2 Alternative):

If you still can't access externally your database, consider bind to all (I don't recommend it unless you have problems connecting).

* Replace `127.0.0.1` with `0.0.0.0`
```
net:
  port: 27017
  bindIp: 0.0.0.0
```

**Warning:** Do not allow the `bindIp` without enabling authorization. Otherwise you will be opening up the whole internet to have full admin access to all mongo databases on your MongoDB server!


**Restart mongo service for changes to take effect (step 3).**

<br>

**Finally:**
* **[How to Configure Firewall, Whitelist and Blacklist in a self-hosted MongoDB server](https://medium.com/mongoaudit/how-to-configure-firewall-whitelist-and-blacklist-in-a-self-hosted-mongodb-server-9a898c6df675)**
* **[Full UFW Tutorial by Ubuntu](https://help.ubuntu.com/community/UFW)**

----------------------------------------------------------------------------------------

<br>
<br>

## 6- Install Nginx
----------------------------------------------------------------------------------------

**Nginx** (pronounced "engine x"), is one of the most renowned open source amongst the web servers on the planet. It is also in charge of serving more than half of the activity on the web. It is equipped for taking care of assorted workloads and working with other programming languages to give a total web stack. Nginx is distinguished as the most effective and light-weight web server today.

HTTP proxies are commonly used with web applications for gzip encoding, static file serving, HTTP caching, SSL handling, load balancing, and spoon feeding clients. Using Nginx to handle static content and proxying requests to your scripts to the actual interpreter is better.

Nginx can be used to remove some load from the Node.js processes, for example, 

* Privileges - Not having to worry about privileges/setuid for the Node.js process. Only root can bind to port 80 typically. If you let nginx worry about starting as root, binding to port 80, and then relinquishing its root privileges, it means your Node app doesn't have to worry about it.
**If** you want to run node directly on port 80, that means you have to run node as root. If your application has any security flaws, it will become significantly compounded by the fact that it has access to do anything it wants. For example, if you write something that accidentally allows the user to read arbitrary files, now they can read files from other server user's accounts.
If you use nginx in front of node, you can run your node as a limited user on port 3000 (or whatever) and then have nginx run as root on port 80, forwarding requests to port 3000. Now the node application is limited to the user permissions you give it.

* Static files - it could be argued that using a CDN it's not as important (though the initial files must still come off your origin) and NGINX is able to serve static files much faster than NodeJS can, and keep node for the more valuable and complex task of serving up the dynamic part of your application. Most often, you'll use nginx as a reverse proxy: the web request will be received by nginx, which acts as a load balancer in front of several identical or subdivided servers.  If it needs to serve static files as well, it will just answer those requests directly. Web Servers, in general, are used to **cache pages** to provide them quicker than a service that would calculate the page every time requested.

* Error Pages - You can more easily display meaningful error pages or fall back onto a static site if your node service crashes. Otherwise, users may just get a timed out connection, this allows your users to never get an error about unable to establish a connection.

* Performance - it's true that node app clustering e.g. with PM2, and container-level scaling (e.g. kubernetes, AWS) will improve performance - though this would be in addition to - not instead of - any gains from using nginx.

Further Read: [Should I use both NGINX and PM2 for Node.js production deployment?](https://www.quora.com/Should-I-use-both-NGINX-and-PM2-for-Node-js-production-deployment)


* SSL/HTTPS - Issuing an SSL Certificate and Handling HTTPS (Nginx is performing consistently better in TLS performance).

* Security -  Running another web server in front of Node may help to mitigate security flaws and DoS attacks against Node. You put Nginx in front of node as a security precaution in case there are any undiscovered exploits in node that might be exposed by not having it protected by a proxy.

<br>

You also use Nginx in front of Node because it is easier to configure, can be restarted separately from the Node process using standard initd/upstart scripts & because binary patches will be released faster than Node in case of any potential future security issues like Heartbleed, which admittedly Node wasn't susceptible to.

Because servers such as NGINX are highly optimized (ie fast) for the task of responding to (and/or forwarding) http requests, particularly for static files. Node could do everything that NGINX can do in this respect but NGINX will do it faster.

It also happens to be a lot less work to set up NGINX configuration for a certain scenario than writing everything you'd need from scratch in JavaScript. **If you're going to** scale your app you're going to need some kind of load balancer / caching proxy in front of your node servers. You could write your own load balancer / caching proxy in node but why bother when you can use a mature, battle-tested industry standard product (that is also faster) instead? 

One of the major reasons to proxy node.js through Nginx is Server Blocks (a.k.a Virtual Hosts in Apache), And this is basically the feature of being able to host multiple websites (domain names) on a single server, meaning that, you don't have enough IPv4 addresses to host each one on its own dedicated IP. This is extremely useful given that you own multiple sites and don’t want to go through the lengthy (and expensive) process of setting up a new web server for each site.


That said, there are other benefits to using a server like nginx facing the public.

In those situations, usually, node.js will still be performing the functions of a web server, they'll just be behind the main nginx proxy and users won't be hitting them directly.

<br>

**NOTE: DON'T EVER USE Apache Server With NodeJS**

<br>

![](https://i.ibb.co/tXWzCNG/network-diagram.jpg)

<br>

### Install NGINX

There are two ways to install NGINX

 **1. Ubuntu Repository:**
Again, Not recommended at all because Ubuntu sucks at keeping their packages updated.

 **2. NGINX Repository:**
Latest Version, Very Recommended, Easy to install and Easy to update.

<br>

Based on the previous methods, I'll go with the second option.

<br>

First, you need to know that NGINX Open Source is available in two versions (Following description is according to NGINX):

* **Mainline** – Includes the latest features and bug fixes and is always up to date. It is reliable, but it may include some experimental modules, and it may also have some number of new bugs.
* **Stable** – Doesn’t include all of the latest features, but has critical bug fixes that are always backported to the mainline version. We recommend the stable version for production servers.

<br>

**Lets Start:**

<br>

1. Install the prerequisites:
```
sudo apt install curl gnupg2 ca-certificates lsb-release -y
```

2. Set up the apt repository:

* To get the Stable nginx package, use:
```
echo "deb http://nginx.org/packages/ubuntu `lsb_release -cs` nginx" \ | sudo tee /etc/apt/sources.list.d/nginx.list
```

* To get the Mainline nginx package, use:
```
echo "deb http://nginx.org/packages/mainline/ubuntu `lsb_release -cs` nginx" \ | sudo tee /etc/apt/sources.list.d/nginx.list
```

**I Personally go with the stable version, Also Nginx recommends the stable version for production.**

3. Import official nginx signing key so apt could verify the packages authenticity:
```
curl -fsSL https://nginx.org/keys/nginx_signing.key | sudo apt-key add -
```

4. To install nginx, run the following commands:
```
sudo apt-get remove nginx-common

sudo apt update

sudo apt install nginx  -y
```

6. Print NGINX version, compiler version, and configure script parameters:

```
sudo nginx -V
```

7. Start NGINX:

```
sudo nginx
```

8. Verify that NGINX is up and running:

```
curl -I 127.0.0.1
```

you should get
```
HTTP/1.1 200 OK
Server: nginx/1.16.0
```

You can also open your browser and navigate to your VPS-IP-Address to see the default NGINX page.

**Congratulations, Now your server is up and running very well.**

9. By default Nginx is now configured to run after a reboot, to check that use:

```
sudo systemctl is-enabled nginx
```

And you should get `enabled` as output. If you didn't, then:


10. Enable Nginx to start on boot and start Nginx immediately:

```
sudo systemctl enable nginx

sudo systemctl start nginx
```

<br>

 **How to Manage Nginx Service**

* Start Nginx:
````
sudo systemctl start nginx
````

* Stop Nginx:
```
sudo nginx -s stop  — fast shutdown

sudo nginx -s quit  — graceful shutdown
```

alternatively, `sudo systemctl stop nginx`

* Status of Nginx:
```
sudo systemctl status nginx
```

* Reload Nginx:

If we make changes to the server configuration, simply reload the nginx without dropping connection. Use the following command to reload the server.

```
sudo nginx -s reload
```

* Disable Nginx at Booting:
```
sudo systemctl disable nginx
```

* Enable Nginx at Booting:
```
sudo systemctl enable nginx
```

<br>

**Extra Info:**

**`/usr/share/nginx/html/`** The basic static site. You can edit the default `index.html` file or drop your own file in this folder and it will be instantly visible.

**`/var/www/html/`** is the server root directory.

**`/etc/nginx/`** Contains all Nginx configuration files. The primary configuration file is `/etc/nginx/nginx.conf`.

**`/var/log/nginx/access.log`** is a server log file that contains records of all request.

**`/var/log/nginx/error.log`** is a server error log file that contains error records.

----------------------------------------------------------------------------------------

<br>
<br>

## 7- Adjust Your Node Application for Production

**This Tutorial Is Taken From @flaviocopes [Website](https://flaviocopes.com/node-difference-dev-prod/)**

<br>

You can have different configurations for production and development environments.

Node assumes it’s always running in a development environment. You can signal Node.js that you are running in production by setting the **`NODE_ENV=production`** environment variable.

This is usually done by executing the command
```
export NODE_ENV=production
```
in the shell, but it’s better to put it in your shell configuration file (e.g.**` .bash_profile`** with the Bash shell) because otherwise the setting does not persist in case of a system restart.

You can also apply the environment variable by prepending it to your application initialization command:
```
NODE_ENV=production node app.js
```
This environment variable is a convention that is widely used in external libraries as well.

Setting the environment to **production** generally ensures that

* logging is kept to a minimum, essential level
* more caching levels take place to optimize performance

For example Pug, the templating library used by Express, compiles in debug mode if **NODE_ENV** is not set to **production**. Express views are compiled in every request in development mode, while in production they are cached. There are many more examples.

Express provides configuration hooks specific to the environment, which are automatically called based on the NODE_ENV variable value:
```
app.configure('development', () => {
  //...
})
app.configure('production', () => {
  //...
})
app.configure('production', 'staging', () => {
  //...
})
```
For example you can use this to set different error handlers for different mode:
```
app.configure('development', () => {
  app.use(express.errorHandler({ dumpExceptions: true, showStack: true }));
})

app.configure('production', () => {
  app.use(express.errorHandler())
})
```

**Further Reading:**

1. [PM2 Runtime | Best Practices | Environment Variables](https://pm2.io/doc/en/runtime/best-practices/environment-variables/)



----------------------------------------------------------------------------------------

<br>
<br>

## 8- Configure NGINX
----------------------------------------------------------------------------------------

Above work was only child play :smiley:  , This is the part that you really need to worry about. Securing Nginx means securing your website, Configuring Nginx means configuring your website.

<br>

### **Configuration Notes:**

As the use of the NGINX web server has grown, NGINX, Inc. has worked to distance NGINX from configurations and terminology that were used in the past when trying to ease adoption for people already accustomed to Apache.

If you’re familiar with Apache, you’ll know that multiple site configurations (called Virtual Hosts in Apache terminology) are stored at `/etc/apache/sites-available/`, which symlink to files in `/etc/apache/sites-enabled/`. However, many guides and blog posts for NGINX recommend this same configuration. As you could expect, this has led to some confusion, and the assumption that NGINX regularly uses the `../sites-available/` and `../sites-enabled/` directories, and the `www-data` user. **It does not.**

Sure, it can. The NGINX packages in Debian and Ubuntu repositories have changed their configurations to this for quite a while now, so serving sites whose configuration files are stored in `/sites-available/` and symlinked to `/sites-enabled/` is certainly a working setup. However it is unnecessary, and the Debian Linux family is the only one which does it. Do not force Apache configurations onto NGINX.

Instead, multiple site configuration files should be stored in `/etc/nginx/conf.d/` as `example.com.conf`, If you then want to disable the site `example.com`, then rename `example.com.conf` to `example.com.conf.disabled`. Do not add `server` blocks directly to `/etc/nginx/nginx.conf` either, even if your configuration is relatively simple. This file is for configuring the server process, not individual websites.

The names of configuration files placed at `conf.d` folder can be what you want but we usually call it the website name to keep it easy for you if you run several websites, but the extension must be `.conf`.

The NGINX process also runs as the username `nginx` in the `nginx` group, so keep that in mind when adjusting permissions for website directories.

Finally, as the [NGINX docs point out](https://www.nginx.com/resources/wiki/start/topics/examples/server_blocks/), the term _Virtual Host_ is an Apache term, even though it’s used in the nginx.conf file supplied from the Debian and Ubuntu repositories, and some of NGINX’s old documentation. A _Server Block_ is the NGINX equivalent, so, that is the phrase you’ll see in this series on NGINX.

<br>
<br>

Let's start by doing something easy and important that will get you familiar with the process of editing nginx config files.

What we will do is related to security, we will disable allowing nginx to show its version in the header.

The first thing for a hacker who wants to hack a website is to gather information, whether for social engineering like information about owner, etc.. or technical information like server version, programming language, installed plugins, etc..

This will allow the hacker to look for the version history of the web server or plugin installed on your site and see if any critical vulnerability or security patches have been made so he can use it, That's why you ALWAYS need to keep everything on your website/machine UP-TO-DATE.

### **Hide Nginx Server Version:**

This will show you how to hide Nginx server version on error pages and in the “Server HTTP” response header field. This is one of the keys recommended practices in securing your Nginx HTTP and proxy server.

1. Check that your nginx server version is available to public.
```
curl -I Your-Server-IP-or-domain
```

and you would receive something like this.
```
HTTP/1.1 200 OK
Server: nginx/1.16.0
Date: Mon, 13 May 2019 17:39:16 GMT
Content-Type: text/html
Content-Length: 612
Last-Modified: Tue, 23 Apr 2019 10:18:21 GMT
Connection: keep-alive
ETag: "5cbee66d-264"
Accept-Ranges: bytes
```

As you can see in the second line `nginx/1.16.0`

To disable this, you need to turn off the server_tokens directive in `/etc/nginx/nginx.conf` configuration file.

2. open `/etc/nginx/nginx.conf` with your preferred editor
```
sudo nano /etc/nginx/nginx.conf
```
3. Add the following line `server_tokens off;` to the `http` block shown in the screenshot below.

![](https://i.ibb.co/ryg3wSL/server-tokens.jpg)

4. Save the file and reload Nginx server for changes to take effect.
```
sudo nginx -s reload
```

5. Check now for Nginx version using curl as in Step 1 and you would see that it's disabled.

<br>
<br>

### **Setup NGINX As Reverse Proxy For Node Application:**

This is what we've been waiting to do with nginx, which is to configure it as a reverse proxy for our node application/s.

1. First, run your node application with PM2 on port 3000 


2. Create a configuration file for the app in `etc/nginx/conf.d/`, I'll call it `nodeapp.conf`:
```
sudo nano /etc/nginx/conf.d/nodeapp.conf
```

_**Note: Don't ever use the tab key to make indentation in configuration files and always use the space key to make spaces.**_


3. Copy the following and paste it in the newly created file:

* If You Have a Domain, Use this:
```
server {
  listen 80;
  listen [::]:80;

  server_name example.com;

  access_log /var/log/nginx/nodeapp-access.log;
  error_log /var/log/nginx/nodeapp-error.log;

  location / {
      proxy_pass http://localhost:3000/;
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
      proxy_set_header X-Forwarded-Host $host;
      proxy_set_header X-Forwarded-Port $server_port;
      proxy_cache_bypass $http_upgrade;
  }
}

server {
  listen 80;
  listen [::]:80;
  access_log  off;
  error_log   off;
  server_name www.example.com xxx.xxx.xxx.xxx;
  return 301 http://example.com$request_uri;
}
```

**Replace example.com With Your Domain and Replace xxx.xxx.xxx.xxx With Your VPS IP Address**

* If You Don't Have a Domain, Use This:
```
server {
  listen 80;
  listen [::]:80;

  server_name xxx.xxx.xxx.xxx;

  access_log /var/log/nginx/nodeapp-access.log;
  error_log /var/log/nginx/nodeapp-error.log;

  location / {
      proxy_pass http://localhost:3000/;
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
      proxy_set_header X-Forwarded-Host $host;
      proxy_set_header X-Forwarded-Port $server_port;
      proxy_cache_bypass $http_upgrade;
  }
}

```

**Replace xxx.xxx.xxx.xxx With Your VPS IP Address**

4. Disable or delete the default Welcome to NGINX page.
```
sudo mv /etc/nginx/conf.d/default.conf /etc/nginx/conf.d/default.conf.disabled
```

5. Test the configuration:
```
sudo nginx -t
```

6. If no errors are reported, reload the new configuration:
```
sudo nginx -s reload
```

7. In a browser, navigate to your Domain or VPS public IP address. You should see your node application.

<br>


* **What are these _directives_ we put in the configuration file ?!**

**First Server Block:**

1. `Listen` - Is the port Nginx will listen to for incoming connections.

  * `listen 80;` - Will listen for incoming IPv4 connections on port 80.
  * `listen [::]:80;` - Will listen for incoming IPv6 connections on port 80.

2. `server_name` - Your domain name without `www`.

3. `access_log` - Defines the path for your site access log, you can set it to off by `access_log off;` to save disk space and IO.

4. `error_log` - Defines the path for your site error log, you can set to off by `error_log off;`.

5. **`proxy_pass` - This directive is what makes this configuration a reverse proxy. It specifies that all requests which match the location block (in this case the root `/` path) should be forwarded to port `3000` on `localhost`, where the Node.js app is running.**

6. `proxy_http_version 1.1` - Defines the HTTP protocol version for proxying, by default it is set to 1.0. For Websockets and keepalive connections you need to use the version 1.1.

7. `Upgrade $http_upgrade and Connection "upgrade"` - These header fields are required if your application is using Websockets.

8. `Host $host` - The $host variable in the following order of precedence contains hostname from the request line, or hostname from the Host request-header field, or the server name matching a request.

9. `X-Real-IP $remote_addr` - Forwards the real visitor remote IP address to the proxied server.

10. `X-Forwarded-For $proxy_add_x_forwarded_for` - A list containing the IP addresses of every server the client has been proxied through.

11. `X-Forwarded-Proto $scheme` - When used inside HTTPS server block , each HTTP response from the proxied server will be rewritten to HTTPS.

12. `X-Forwarded-Host $host` - Defines the original host requested by the client.

13. `X-Forwarded-Port $server_port` - Defines the original port requested by the client.

14. `proxy_cache_bypass $http_upgrade` - Sets conditions under which the response will not be taken from a cache.


**Second Server Block:**

`return 301` - This will redirect all requests of your subdomain `www` and your `VPS-IP-Address` to your domain name without `www`.

`$request_uri` - This is responsible to redirect the path and parameters along with the domain.

<br>

* **Disable Buffering**

More complex apps may need additional directives. For example, Node.js is often used for apps that require a lot of real-time interactions. To accommodate, disable NGINX’s buffering feature.

1. Add the line `proxy_buffering off;` to the config file beneath `proxy_pass` inside location section
```
location / {
      proxy_pass http://localhost:3000/;
      proxy_buffering off;
}
```

<br>
<br>

### **Running And Proxying Multiple Node Apps:**

```
                  +--- host --------> node.js on localhost:8080
                  |
users --> nginx --|--- host/blog ---> node.js on localhost:8181
                  |
                  +--- host/mail ---> node.js on localhost:8282
```

<br>

In the previous section, you've learned how to run and proxy **one** node application. But, what if you want to run multiple node applications on the same server.

This also is considered a production best practice, for example, you would have your main node app which will serve the main page and it's functions and another separate app responsible for registration and another app for your blog, shop, etc..

And when users need to register, you would redirect them to http://example.com/register and in the back scene its a different node application proxied to run on the `/register` path that is completely responsible for the registration process and same goes for example if you have a blog or a shop or a major thing on your site.

This would allow you as a node developer to debug and dissect the code of each thing on your website separately, if you want to implement a payment processor for your website you would just go for the folder where your registration application resides, if you want for example to shut down your blog or add a new feature you would just do it without having to edit the whole site code and then reload it which might highly result in a connection drop to current visitors.

<br>

This is achieved very easy, you only have to specify two things in the configuration file which is the `loaction` and `proxy_pass`.
```
  location /register {
      proxy_pass http://localhost:3001/;
  }
```

This is an example of a configuration that proxies four node apps _(I ignored most of the **directives** because this is an example but in real world you need to use them)_:
```
server {
  listen 80;
  listen [::]:80;

  server_name example.com;

  access_log off;
  error_log /var/log/nginx/nodeapp-error.log;

  location / {
      proxy_pass http://localhost:3000/;
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
      proxy_set_header Host $host;
      proxy_cache_bypass $http_upgrade;
    }

  location /register {
      proxy_pass http://localhost:3001/;
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
      proxy_set_header Host $host;
      proxy_cache_bypass $http_upgrade;
    }
	
  location /blog {
      proxy_pass http://localhost:3002/;
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
      proxy_set_header Host $host;
      proxy_cache_bypass $http_upgrade;
    }

  location /shop {
      proxy_pass http://localhost:3004/;
      proxy_http_version 1.1;
      proxy_set_header Upgrade $http_upgrade;
      proxy_set_header Connection "upgrade";
      proxy_set_header Host $host;
      proxy_cache_bypass $http_upgrade;
    }

}

server {
 server_name www.example.com xxx.xxx.xxx.xxx;
 return 301 http://example.com$request_uri;
}
```

<br>
<br>

### **Serving Error Pages:**

This is how to serve error pages of the 500 family
```
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }
```

Above code is to placed in the server block beneath the location section **but not inside it**.

Visitors will see this page when they visit one of your proxied paths `/blog` , `/shop` ,etc..  while its currently down (node app is not active).

You can, of course, design your custom page and replace `50x.html` with the new file name.

<br>
<br>


### **Modifying Open File/Concurrent Connections Limit:**

**`failed (24: Too many open files)`**

This error above is something that you might get and I discovered it by chance while taking a look over my `error.log` file.

This is related to the limits imposed by Linux to set the maximum number of open files and this simply means **connections** because visitors request files from your machine, therefore, a limit of open files of 1000 means that you can only have 1000 users that can surf your site at the same time and this is something you need to fix asap, so, let's fix it.

To check for hard and soft values, run the following commands:
```
ulimit -Hn

ulimit -Sn
```

1. Open file `/etc/sysctl.conf` with your prefered editor:
```
sudo nano /etc/sysctl.conf
```

2. Add this line to it:
```
fs.file-max = 1000000
```

3. Edit following file:
```
sudo nano /etc/security/limits.conf
```

4. Add the following at the end of it:
```
nginx soft     nofile         1000000   
nginx hard     nofile         1000000
* soft     nproc          1000000    
* hard     nproc          100000   
* soft     nofile         100000   
* hard     nofile         100000
root soft     nproc          100000    
root hard     nproc          100000   
root soft     nofile         100000   
root hard     nofile         100000
```

5. Reload the changes:
```
sudo sysctl -p
```

6. set limit through ulimit
```
ulimit -n 100000
```

7. Remove restriction by nginx, open main conf file
```
sudo nano /etc/nginx/nginx.conf
```

8. Add `worker_rlimit_nofile` Option in nginx conf right beneath `worker_processes` variable:
```
worker_rlimit_nofile 100000;
```

9. Change `worker_connections` value to 100000
```
worker_connections  100000;
```

10. Check for Nginx configuration and reload
```
sudo nginx -t && sudo nginx -s reload
```

11. **Restart your machine for changes to take effect** and check again:
```
ulimit -Hn

ulimit -Sn
```

<br>
<br>

### **Enable Nginx Status Page:**

Nginx has status page to give you information about Nginx’s server health including Active connections and other data. You can use this info to fine tune your server.
 
1. On most Linux distributions, the Nginx version comes with the ngx_http_stub_status_module enabled. You can check out if the module is already enabled or not using following command.
```
nginx -V 2>&1 | grep -o with-http_stub_status_module
```

You should see the following `with-http_stub_status_module`

2. Edit your server conf file in conf.d and 
```
sudo nano /etc/nginx/conf.d/nodeapp.conf
```

3. Add the following inside the server block
```
   location /nginx_status {
      stub_status on;
      access_log   off;
      allow 127.0.0.1;   #only allow requests from localhost
      deny all;   #deny all other hosts
    }
```

We simply created a new location at `nginx_status` and you can change it to what you want, the `stub_status on;` is what resposible to turn on nginx stats.

4. Check for configurations and reload
```
sudo nginx -t && sudo nginx -s reload
```

5. Go to `127.0.0.1/nginx_status` or `curl 127.0.0.1/nginx_status` and you should see an output like this
```
Active connections: 43 
server accepts handled requests
 7368 7368 10993 
Reading: 0 Writing: 5 Waiting: 38
```


**Interpretation:**

* **Active connections** – Number of all open connections. This doesn’t mean the number of users. A single user, for a single pageview, can open many concurrent connections to your server.
* **Server accepts handled requests** – This shows three values.
  * First is total accepted connections.
  * Second is total handled connections. Usually, the first 2 values are the same.
  * Third value is number of and handles requests. This is usually greater than the second value.
  * Dividing third-value by second-one will give you the number of requests per connection handled by Nginx. In the above example, 10993/7368, **1.49 requests per connections**.
* **Reading** – The current number of connections where Nginx is reading the request header.
* **Writing** – The current number of connections where Nginx is writing the response back to the client.
* **Waiting** – keep-alive connections, actually it is active – (reading + writing). This value depends on keepalive-timeout. Do not confuse non-zero waiting value for poor performance. It can be ignored. Although, you can force zero waiting by setting keepalive_timeout 0;

<br>
<br>

### **Nginx Better Configurations:**

First, open nginx main conf file
```
sudo nano /etc/nginx/nginx.conf
```

Let Nginx calculate CPU cores automatically
```
worker_processes auto;
```

Only log critical errors (optional)
```
error_log /var/log/nginx/error.log crit;
```

To boost I/O on HDD we can disable access logs (optional)
```
access_log off;
```

If it’s mandatory to have access logging, then enable access-log buffering. This enables Nginx to buffer a series of log entries and writes them to the log file together at once instead of performing the different write operation for each single log entry.
```
access_log /var/log/nginx/access.log main buffer=16k
```

Copies data between one FD and other from within the kernel which is faster than read() + write()
```
sendfile on;
```

This parameter will allow your server to send HTTP response in one packet instead of sending it in frames. This will optimize throughout and minimize bandwidth consumption which will result in improvement of your website’s loading time.
```
tcp_nopush on;
```

This parameter allows you to prevent your system from buffering data-sends and all the data will be sent in small bursts in real time, it's also good for WebSocket proxying. You can set this parameter by adding the following line
```
tcp_nodelay on;
```

<br>
<br>

### **Enable Gzip Compression:**

Gzip compression is used to accelerate your site performance by archiving your site assets in gzip format before being sent to clients and then clients browsers will un-archive the files.

This will allow for low bandwidth but also high CPU usage as your CPU will compress files before sending.

Enabling Gzip is very easy but I won't cover it in this tutorial. it only takes `gzip on;` to enable it but enabling gzip over SSL can make your website vulnerable to [BREACH attack](http://breachattack.com/) as Nginx official documentation says.

Anyway here is the nginx gzip documentation and I'll leave it up to you [Module ngx_http_gzip_module](http://nginx.org/en/docs/http/ngx_http_gzip_module.html)

_Choose your directives wisely_


<br>

**Take a look at the [Site Performance](https://github.com/XOR-LIFE/node-production#12--site-performance) section before deciding on gzip.**

<br>
<br>

### **Serving Static Files:**

The directory NGINX serves sites from differs depending on how you installed it. At the time of this writing, NGINX supplied from NGINX repository `/usr/share/nginx/`.

The NGINX docs warn that relying on the default location can result in the loss of site data when upgrading NGINX. You should use `/var/www/` or `/srv/`, or some other location that won’t be touched by package or system updates.

This tutorial will use `/var/www/example.com/` in its examples. Replace `example.com` where you see it with the IP address or domain name of yours.

1. The root directory for your site or sites should be added to the corresponding `server` block of `/etc/nginx/conf.d/example.com.conf`:
```
root /var/www/example.com;
```

2. Then create that directory:
```
mkdir -p /var/www/example.com
```


**To Be Continued...**



<br>
<br>

### **Configure HTTPS with Certbot:**

One advantage of a reverse proxy is that it is easy to set up HTTPS using a TLS certificate. Certbot is a tool that allows you to quickly obtain free certificates from Let’s Encrypt. This guide will use Certbot on Ubuntu 18.04, but the[ official site](https://certbot.eff.org/) maintains comprehensive installation and usage instructions for all major distros.

Follow these steps to get a certificate via Certbot. Certbot will automatically update your NGINX configuration files to use the new certificate:

Before starting you need to add the `www` subdomain to your conf file to your `server_name` directive as below so that certbot will issue a certificate for your subdomain
```
server_name example.com www.example.com;
```

Replace `example.com` with your domain name, **Also comment any server block used for redirection as we did above as certbot will do it.**


1. Install the Certbot and web server-specific packages, then run Certbot:
```
sudo apt-get update
sudo apt-get install software-properties-common openssl -y
sudo add-apt-repository universe
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install certbot python-certbot-nginx  -y
```

2. Certbot will ask for information about the site. The responses will be saved as part of the certificate:
```
# sudo certbot --nginx

Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator nginx, Installer nginx

Which names would you like to activate HTTPS for?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: example.com
2: www.example.com
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate numbers separated by commas and/or spaces, or leave input
blank to select all options shown (Enter 'c' to cancel): 1,2
Obtaining a new certificate
Deploying Certificate to VirtualHost /etc/nginx/conf.d/nodeapp.conf
Deploying Certificate to VirtualHost /etc/nginx/conf.d/nodeapp.conf

Please choose whether or not to redirect HTTP traffic to HTTPS, removing HTTP access.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: No redirect - Make no further changes to the webserver configuration.
2: Redirect - Make all requests redirect to secure HTTPS access. Choose this for
new sites, or if you're confident your site works on HTTPS. You can undo this
change by editing your web server's configuration.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate number [1-2] then [enter] (press 'c' to cancel): 2
Redirecting all traffic on port 80 to ssl in /etc/nginx/conf.d/nodeapp.conf
Redirecting all traffic on port 80 to ssl in /etc/nginx/conf.d/nodeapp.conf

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Congratulations! You have successfully enabled https://example.com and
https://www.example.com

You should test your configuration at:
https://www.ssllabs.com/ssltest/analyze.html?d=example.com
https://www.ssllabs.com/ssltest/analyze.html?d=www.example.com
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/example.com/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/example.com/privkey.pem
   Your cert will expire on 2019-08-18. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot again
   with the "certonly" option. To non-interactively renew *all* of
   your certificates, run "certbot renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le
```

3. Certbot will also ask if you would like to automatically redirect HTTP traffic to HTTPS traffic. It is recommended that you select this option.

4. When the tool completes, Certbot will store all generated keys and issued certificates in the `/etc/letsencrypt/live/$domain` directory, where `$domain` is the name of the domain entered during the Certbot certificate generation step.

Finally, Certbot will update your web server configuration so that it uses the new certificate, and also redirects HTTP traffic to HTTPS if you chose that option.


* **Automating renewal**

The Certbot packages on your system come with a cron job that will renew your certificates automatically before they expire. Since Let's Encrypt certificates last for 90 days, it's highly advisable to take advantage of this feature. You can test automatic renewal for your certificates by running this command:
```
sudo certbot renew --dry-run
```

You can also check for existing certificates using
```
sudo certbot certificates
```

<br>
<br>

### **Enabling HTTP /2.0 Support And Set As Default Server:**

HTTP/2 is the successor to the HTTP/1.1 standard which, among other benefits, reduces page load times and bandwidth used. While the HTTP/2 specification applies to both HTTP and HTTPS traffic, web browsers currently do not support unencrypted HTTP/2, so it can only be used with TLS.


1. Add the `http2` option and `default_server` to the `listen` directive in your site configuration’s `server` block for both IPv4 and IPv6. It should look like below:
```
listen    [::]:443 ssl http2 default_server ipv6only=on;
listen    443 ssl http2 default_server;
```

2. Reload NGINX:
```
sudo nginx -s reload:
```

3. Verify HTTP/2 is enabled with [HTTP2.Pro](https://http2.pro/).

<br>
<br>

### **Redirect VPS-IP-Address To Domain:**

Certbot has managed the redirection process by adding the necessary `server` blocks, but still, your website is accessed through your VPS-IP-Address.

**So, let's make some small changes.**

1. Redirect Your VPS-IP-Address to your domain name (open your eyes well :heart_eyes: )

This is the configuration added by certbot to manage redirections
```
server {
    if ($host = www.example.com) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    if ($host = example.com) {
        return 301 https://$host$request_uri;
    } # managed by Certbot



  listen 80;
  listen [::]:80;
  server_name example.com www.example.com;
    return 404; # managed by Certbot

     }
```

Above config will redirect http (www and non-www) domain name to https

so let's add a new redirection that will redirect your VPS-IP-Address in both http and https to your domain name, and this is the full configuration along with the ones added by certbot
```
server {
    if ($host = www.example.com) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    if ($host = example.com) {
        return 301 https://$host$request_uri;
    } # managed by Certbot

  listen 80;
  listen [::]:80;
  server_name example.com www.example.com;
    return 404; # managed by Certbot

             }

# This will redirect our vps-ip-address in both http and https
server {
  listen 80;
  listen [::]:80;
  listen 443 ssl;
  listen [::]:443 ssl;
  server_name xxx.xxx.xxx.xxx;
  return 301 https://example.com$request_uri;
}
```

<br>

Replace `xxx.xxx.xxx.xxx` with your VPS-IP-Address.

Save configuration and check for errors `sudo nginx -t` and Reload Nginx `sudo nginx -s reload`

Visit your VPS-IP-Address with both http and https to see if it's now redirected to your domain name.

<br>
<br>

### **Add Security Headers:**

As the title says, these headers will enhance the security of your website and prevent multiple attacks.

We add headers to a website through the [add_header](http://nginx.org/en/docs/http/ngx_http_headers_module.html) directive.

You got two options to add these headers:

1. In `http` block in main nginx configuration file `/etc/nginx/nginx.conf`, doing this, these headers will be applied to all the sites you have.
2. In a site conf file in `conf.d` folder and this will make it applicable to this site individually.

_If you went with second option then don't use the first option and put any headers in the main `nginx.conf` file as it might cause you problems in loading headers_

Just copy/paste these headers below and add them to whichever location you choose from above two.
```
add_header X-Content-Type-Options nosniff;
add_header X-Frame-Options SAMEORIGIN;
add_header X-XSS-Protection "1; mode=block";
add_header Strict-Transport-Security "max-age=31536000; includeSubDomains; preload" always;
```

Check for configurations and reload
```
sudo nginx -t && sudo nginx -s reload
```


Once you've added these tags you can check using `curl -I Your-VPS-IP-or-Domain` 

_Note: There are other headers that I ignored using as they are not necessary or require further knowledge and might affect your site negatively, these headers are `Content-Security-Policy`, `Referrer-Policy`, `Feature-Policy`_

You can read about what all of these headers mean at [OWASP Secure Headers Project
](https://www.owasp.org/index.php/OWASP_Secure_Headers_Project#tab=Headers).


**_Note: There are many tutorials on the internet that suggest adding headers to improve SSL Certificate, and I urge you not to, as most if not ALL of these headers exist in this file `/etc/letsencrypt/options-ssl-nginx.conf` which is already included automatically by Certbot in your conf file._**

<br>
<br>

Online testing tools:
 - [Mozilla Observatory](https://observatory.mozilla.org/)
 - [SSL Labs](https://www.ssllabs.com/ssltest/)
 - [Security Headers](https://securityheaders.com)

Useful links:
 - [Be very careful with your add_header in Nginx! You might make your site insecure](https://www.peterbe.com/plog/be-very-careful-with-your-add_header-in-nginx)
 - [Mozilla Security Guidelines](https://infosec.mozilla.org/guidelines/web_security)
 - [Subresource Integrity](https://developer.mozilla.org/en/docs/Web/Security/Subresource_Integrity)
 - [Content Security Policy](https://developer.mozilla.org/en/Add-ons/WebExtensions/Content_Security_Policy)
 - [Certbot documentation](https://certbot.eff.org/docs/)

<br>

**Further Reading:**

1. [A Guide to Caching with NGINX and NGINX Plus](https://www.nginx.com/blog/nginx-caching-guide/)




----------------------------------------------------------------------------------------

<br>
<br>

## 9- Canonical Livepatch
----------------------------------------------------------------------------------------

Livepatch allows you to install some critical kernel security updates without rebooting your system, by directly patching the running kernel.

It does not affect regular (not security-critical) kernel updates, you still have to install those the regular way and reboot. It does not affect updates to other non-kernel packages either, which don't require a reboot anyway.

Activate at: **[Canonical Livepatch](https://ubuntu.com/livepatch)**

----------------------------------------------------------------------------------------

<br>
<br>

## 10- Useful Readings
----------------------------------------------------------------------------------------


* **[How to use MongoDB with Node.js](https://flaviocopes.com/node-mongodb/)**
* **[npm global or local packages](https://flaviocopes.com/npm-packages-local-global/)**
* **[npm dependencies and devDependencies](https://flaviocopes.com/npm-dependencies-devdependencies/)**
* **[Find the installed version of an npm package](https://flaviocopes.com/npm-know-version-installed/)**
* **[Update all the Node dependencies to their latest version](https://flaviocopes.com/update-npm-dependencies/)**
* **[The Node path module](https://flaviocopes.com/node-module-path/)**
* **[Node File Paths](https://flaviocopes.com/node-file-paths/)**
* **[Working with folders in Node](https://flaviocopes.com/node-folders/)**
* **[Error handling in Node.js](https://flaviocopes.com/node-exceptions/)**
* **[Impress your colleagues with these NPM tricks](https://dev.to/borrellidev/impress-your-colleagues-with-these-npm-tricks-3fcb/)**

----------------------------------------------------------------------------------------

<br>
<br>

## 11- Checklist
----------------------------------------------------------------------------------------

_**This is a list you should check before finally saying that my website is up and ready, It's meant to remind you of things you might've forgotten.**_

* Node.js and NPM are installed

* PM2 is used to run and manage your node applications

* PM2 is configured to run on startup

* PM2 has saved the list of node applications that supposed to run on startup

* UFW is enabled and used to manage access to your machine

* MongoDB port '27017' MUST NOT be allowed for external access if,

1. Your node and mongo are running on the same machine, and,

2.  You are not going to allow external access

* Node applications ports MUST NOT be allowed for external access and must be blocked/denied in UFW

* Nginx must be enabled to run on startup as mentioned in the installation section of Nginx

* After making any edits to nginx conf files you must check with `sudo nginx -t` and then reload `sudo nginx -s reload`



----------------------------------------------------------------------------------------

<br>
<br>

## 12- Site Performance
----------------------------------------------------------------------------------------

This section is to help you as a front-end developer to improve the performance of your site, thus, decreasing your website load time and decreasing your bandwidth.

* Use CDN for your Javascript libraries:
  * Most, if not all of the javascript libraries have a CDN, but if you can't seem to find it then use [cdnjs](https://cdnjs.com/)

* Use HTTP/2:
  * I've already covered that in Nginx

* Validate HTML:
  * [Validator.nu HTML5 Validator](https://html5.validator.nu/)
  * [W3 HTML Validation Service](https://validator.w3.org/#validate_by_input)
  * [FreeFormatter HTML Validator](https://www.freeformatter.com/html-validator.html)
  * [W3 Link Checker](https://validator.w3.org/checklink)

* Validate CSS:
  * [W3 CSS Validation Service](http://jigsaw.w3.org/css-validator/)
  * [CSSPortal CSS Validator](https://www.cssportal.com/css-validator/)
  * [CSS LINT](http://csslint.net/)
  * [CodeBeautify CSS Validator](https://codebeautify.org/cssvalidate)

* Validate Javascript (NOT NODE.JS):
  * [JSHint](https://jshint.com/)
  * [BeautifyTools Javascript Validator](http://beautifytools.com/javascript-validator.php)
  * [CodeBeautify JavaScript Validator](https://codebeautify.org/jsvalidate)

<br>
<br>

* Minify HTML (use any of them):
  * [MinifyCode HTML minifier](http://minifycode.com/html-minifier/)
  * [WillPeavy HTML Minifier](https://www.willpeavy.com/minifier/)

* Minify CSS (use any of them):
  * [CSSMinifier](https://cssminifier.com/)
  * [Minifier](https://www.minifier.org/)
  * [CSS Compressor](https://csscompressor.com/)

* Minify JavaScript (NOT NODE.JS):
  * [UglifyJS 3](https://github.com/mishoo/UglifyJS2)
  * [JSCompress](https://jscompress.com/)
  * [JavaScript Minifier](https://javascript-minifier.com/)


* Compress Images (Compress your images without losing quality)
  * Search Google "Compress Images", _a lot of online sites and they all are good_
  * [JPEGMini](https://www.jpegmini.com/) ,I use this tool personally, it's the best ever in the industry, you can find it cracked, but if you have the money I really urge you to buy it to support the developers and keep it maintained.

<br>
<br>

* Test Your Site:
  * [PageSpeed](https://developers.google.com/speed/pagespeed/insights/)
  * [Lighthouse](https://chrome.google.com/webstore/detail/lighthouse/blipmdconlkpinefehnmjammfjpmpbjk)
  * [GTmetrix](https://gtmetrix.com/)
  * [dareboost](https://www.dareboost.com/en)
  * [Pingdom Website Speed Test](https://tools.pingdom.com/)
  * [Website Speed Test](https://tools.keycdn.com/speed)
  * [Web Performance Test](https://tools.keycdn.com/performance)
  * [GEEKFLARE Toolbox](https://tools.geekflare.com/tools/)
  * [WebPageTest](http://webpagetest.org)
  
----------------------------------------------------------------------------------------

<br>
<br>

## 13- Securing VPS
----------------------------------------------------------------------------------------

### **Install Fail2Ban**



