# **Take Your NodeJS Project to The Production Environment (VPS/Dedicated Server).**


The following instructions are based on **Ubuntu**, the steps are the same for whatever Linux distribution you are going to use but the commands might be different.
----------------------------------------------------------------------------------------

## Setup Your VPS
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

You can now connect to your machine through SSH using, for example [PuTTY](www.putty.org).

<br>

### **Enable SFTP**

Secure File Transfer Protocol or **SFTP** is a network protocol that provides file access, file transfer, and file management over any reliable data stream.

SFTP has pretty much replaced legacy FTP as a file transfer protocol and is quickly replacing FTP/S. It provides all the functionality offered by these protocols, but more securely and more reliably, with easier configuration. There is basically no reason to use legacy protocols anymore.

SFTP also protects against password sniffing and man-in-the-middle attacks. It protects the integrity of the data using encryption and cryptographic hash functions and authenticates both the server and the user.

SFTP uses the same port used by SSH so you would need nothing more to install if you have already enabled SSH in your machine.

All that you need is just an application to connect to your machine through SFTP.

I recommend those two applications and you can use what you want.

* **FileZilla Client:** a cross-platform FTP, SFTP, and FTPS client with a vast list of features, which supports Windows, Mac OS X, Linux, and more.

Download from here: [FileZilla Client Download](https://filezilla-project.org/download.php?type=client)

* **Bitvise SSH Client (Windows Only):** Free and flexible SSH Client for Windows includes state of the art terminal emulation, graphical as well as command-line SFTP support, an FTP-to-SFTP bridge, powerful tunneling features including dynamic port forwarding through an integrated proxy, and remote administration for our SSH Server.

Download from here: [Bitvise SSH Client Download](https://www.bitvise.com/ssh-client-download)

<br>

### **Enable VNC**

Even though this is not necessary if you are going to interact with your machine through commands all the time (SSH), but you might need to connect to your machine through VNC for any reason and access the GUI version due to simplicity.

If you installed **Ubuntu Server** on your machine then having VNC is _Useless_, unless you are going to install a GUI/Desktop on your Ubuntu Server. And if you want so then I suggest going with [Lubuntu Core Server Desktop](https://linuxconfig.org/install-gui-on-ubuntu-server-18-04-bionic-beaver#h7-2-lubuntu-core-server-desktop)

<br>

**Which VNC application is better ??**

I've tried **All** VNC applications and I can tell you that **RealVNC** is the best VNC application with no doubt.

RealVNC consists of two products, Server and Viewer.

* RealVNC Server: Download to the remote computer you want to control (This is to be downloaded on you VPS machine).
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

3. `cd` to the folder which RealVNC was download, It's Downloads in my case
```
cd Downloads
```

4. Install .deb package using `sudo dpkg –i`
```
sudo dpkg –i packagename.deb
```

5. Run subscription wizard so you could log in with your home subscription account
```
vnclicensewiz
```
This will launch the wizard and you just need to log in with your RealVNC credentials. You will also need root password when requested.

6. Start RealVNC and Enable it on boot
```
sudo systemctl start vncserver-x11-serviced.service

sudo systemctl enable vncserver-x11-serviced.service
```
**Installation is now finished.**

* You can now install RealVNC Viewer on your controller machine from the wide list of supported machines.
```
https://www.realvnc.com/en/connect/download/viewer/
```

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

sudo apt install git
```

3. Validate installation and check for version
```
git --version
```

<br>

----------------------------------------------------------------------------------------

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


## More about PM2

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

* 14.04 LTS (trusty)
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
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
```

2. Create a list file for MongoDB:

If you are unsure of what Ubuntu version you are running, open a terminal or shell on the host and execute `lsb_release -dc`

* For Ubuntu 18.04 (Bionic):
```
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
```

* For Ubuntu 16.04 (Xenial)
```
echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu xenial/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
```

* For Ubuntu 14.04 (Trusty):
```
echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu trusty/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
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

* **Add Admin User For The First Time:**

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

* **Mongodb Configuration File:**

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

* **Enable MongoDB authentication:**

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
mongo -u admin -p pass123 --authenticationDatabase admin
```
And you should see the mongo shell, type `exit` and run.

<br>

* **Change MongoDB Default Port:**

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

* **Securing MongoDB from Injection Attacks:**

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

3. Ensure to allow SSH:
```
sudo ufw allow OpenSSH
```

4. If you use VNC to connect to your machine, make sure to also open its corresponding port (Its 5900 in my case, I use RealVNC):
```
sudo ufw allow 5900
```

5. Enable UFW, because it's probably inactive:
```
sudo ufw enable
```

6. Rerun the UFW status command:
```
sudo ufw status
```

which now should return the following:
```
Status: active

To                         Action      From
--                         ------      ----                              
OpenSSH                    ALLOW       Anywhere                        
5900                       ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
5900 (v6)                  ALLOW       Anywhere (v6)
```

7. Allow access to the default MongoDB port `27017`:
```
sudo ufw allow 27017
```

8. Check UFW status:
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

<br>

* **Allow External Access To MongoDB**
 
Right now we have configured UFW to allow external access to our machine with Mongo port, but still, Mongo itself isn't configured for external access. even though the default port is open, the database server is currently listening on 127.0.0.1. To permit remote connections, you must include a publicly-routed IP address for your server to `mongo.conf` file.

By default, MongoDB authorizes all logs from the local machine. There are no problems while the application is developing.

However, because you have to enable authentication, you might run into issues when the application is ready and you have to deploy it, So you need to configure it.

1. Allow remote connections, to bind MongoDB to all network interfaces open the config file `/etc/mongod.conf`:
```
sudo nano /etc/mongod.conf
```

2. Edit `bindIP:` in `net:` section with setting below:
```
net:
  port: 27017
  bindIp: 127.0.0.1,your_vps_ip
```

A comma should be placed before adding the IP address you want to allow. Once that is done, Save the changes and exit the editor.

More on bindIp; [MongoDB bindIp Configurations](https://docs.mongodb.com/manual/reference/configuration-options/#net.bindIp)

3. Restart mongodb:
```
sudo service mongod restart
```

4. Bind to all (Step 2 Alternative):

If you still can't access externally your database, consider bind to all (I, my self do so).

* Replace `127.0.0.1` with `0.0.0.0`
```
net:
  port: 27017
  bindIp: 0.0.0.0
```

**Warning:** Do not allow the `bindIp` without enabling authorization. Otherwise you will be opening up the whole internet to have full admin access to all mongo databases on your MongoDB server!


**Restart mongo service for changes to take effect (step 3).**

----------------------------------------------------------------------------------------

<br>

## 6- Install Nginx
----------------------------------------------------------------------------------------

**Nginx** is one of the most renowned open source amongst the web servers on the planet. It is also in charge of serving more than half of the activity on the web. It is equipped for taking care of assorted workloads and working with other programming languages to give a total web stack. Nginx is distinguished as the most effective and light-weight web server today.

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

Further Read: [How to Create a Nginx Virtual Host (AKA Server Blocks)](https://www.keycdn.com/support/nginx-virtual-host)


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

First, you need to know that NGINX Open Source is available in two versions (According to NGINX):

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

sudo apt install nginx
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
Server: nginx/1.13.8
```

You can also open your browser and navigate to your VPS-IP-Address to see the default NGINX page.

**Congratulations, Now your server is up and running very well.**

9. By default Nginx is now configured to run after a reboot, to check that use:

```
sudo systemctl is-enabled nginx.service
```

And you should get `enabled` as output. If you didn't, then:


10. Enable Nginx to start on boot and start Nginx immediately:

```
sudo systemctl enable nginx.service

sudo systemctl start nginx.service
```

<br>

 **How to Manage Nginx Service**

* Start Nginx:
````
sudo systemctl start nginx.service
````

* Stop Nginx:
```
sudo nginx -s stop  — fast shutdown

sudo nginx -s quit  — graceful shutdown
```

alternatively, `sudo systemctl stop nginx.service`

* Status of Nginx:
```
sudo systemctl status nginx.service  
```

* Reload Nginx:

If we make changes to the server configuration, simply reload the nginx without dropping connection. Use the following command to reload the server.

```
sudo nginx -s reload
```

* Disable Nginx at Booting:
```
sudo systemctl disable nginx.service
```

* Enable Nginx at Booting:
```
sudo systemctl enable nginx.service  
```

<br>

**Extra Info:**

**`/usr/share/nginx/html/`** The basic static site. You can edit the default `index.html` file or drop your own file in this folder and it will be instantly visible.

**`/etc/nginx/`** Contains all Nginx configuration files. The primary configuration file is `/etc/nginx/nginx.conf`.

**`/var/log/nginx/access.log`** is a server log file that contains records of all request.

**`/var/log/nginx/error.log`** is a server error log file that contains error records.






<br>

----------------------------------------------------------------------------------------

<br>

## 7- Adjust Your Node Application for Production

**This Tutorial Is Taken From [Flavio Copes Website](https://flaviocopes.com/node-difference-dev-prod/)**

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



----------------------------------------------------------------------------------------

<br>

## 8- Configure NGINX
----------------------------------------------------------------------------------------


