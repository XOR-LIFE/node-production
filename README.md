# **Take Your NodeJS Project to The Production Environment (VPS/Dedicated Server).**


The following instructions are based on **Ubuntu**, the steps are the same for whatever Linux distribution you are going to use but the commands might be different.
----------------------------------------------------------------------------------------

<br>

## 1- Install NodeJS and NPM.

There are several ways to install node.js

 **1. Ubuntu Packages:**
Not recommended at all because Ubuntu sucks at keeping their packages updated.

 **2. NodeSource Repository:**
Very Recommended, Easy to install and Easy to update.
 
 **3. NVM (Node Version Manager):**
Used if you are going to install multiple node versions on your machine.

<br>

Based on the previous methods, I'll go with the second option.

<br>

1. Update your Ubuntu:

`sudo apt update && sudo apt upgrade`

2. Install important packages for node and npm:

`sudo apt install gcc g++ make build-essential curl -y`

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

`sudo apt remove nodejs npm`

this will uninstall node and npm but will keep your configuration files.

<br>

**To also delete your configuration files, use:**

`sudo apt purge nodejs npm`

----------------------------------------------------------------------------------------

<br>

## 2- Install PM2


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

To monitor logs, custom metrics and information abouut your application’s health. For example, you’ll see CPU utilization, memory usage, requests/minute, and more!

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

## 3- Install MongoDB

----------------------------------------------------------------------------------------

<br>

## 4- Install Nginx

**Nginx** is one of the most renowned open source amongst the web servers on the planet. It is also in charge of serving more than half of the activity on the web. It is equipped for taking care of assorted workloads and working with other programming languages to give a total web stack.

HTTP proxies are commonly used with web applications for gzip encoding, static file serving, HTTP caching, SSL handling, load balancing, and spoon feeding clients. Using Nginx to handle static content and proxying requests to your scripts to the actual interpreter is better.

Nginx can be used to remove some load from the Node.js processes, for example, 

* Privileges - Not having to worry about privileges/setuid for the Node.js process. Only root can bind to port 80 typically. If you let nginx worry about starting as root, binding to port 80, and then relinquishing its root privileges, it means your Node app doesn't have to worry about it.

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

### Install NGINX

There are several ways to install NGINX

 **1. Ubuntu Packages:**
Again, Not recommended at all because Ubuntu sucks at keeping their packages updated.

 **2. NGINX Repository:**
Latest Version, Very Recommended, Easy to install and Easy to update.

<br>

Based on the previous methods, I'll go with the second option.

<br>

First of all you need to know that NGINX Open Source is available in two versions:

* **Mainline** – Includes the latest features and bug fixes and is always up to date. It is reliable, but it may include some experimental modules, and it may also have some number of new bugs.
* **Stable** – Doesn’t include all of the latest features, but has critical bug fixes that are always backported to the mainline version. We recommend the stable version for production servers.

<br>

**Lets Start:**

<br>

1. Download the key used to sign NGINX packages and the repository, and add it to the `apt` program’s key ring:
```
sudo wget https://nginx.org/keys/nginx_signing.key

sudo apt-key add nginx_signing.key
```

2. Edit the **/etc/apt/sources.list** file, for example with `nano`:
```
sudo nano /etc/apt/sources.list
```

3. Add these lines to **sources.list** to name the repositories from which the NGINX source can be obtained:
```
deb https://nginx.org/packages/mainline/ubuntu/ <CODENAME> nginx
deb-src https://nginx.org/packages/mainline/ubuntu/ <CODENAME> nginx
```

where:

* The `/mainline` element in the pathname points to the latest mainline version of NGINX; delete it to get the latest stable version
* `<CODENAME>` is the codename of an Ubuntu release



|Version|Codename|Supported Platforms|
|---|---|---|
|14.04|trusty|x86_64, i386, aarch64/arm64|
|16.04|xenial|x86_64, i386, ppc64el, aarch64/arm64|
|18.04|bionic|x86_64, aarch64/arm64|
|18.10|cosmic|x86_64|


For example, to get the latest stable package for Ubuntu 18.04 (“bionic”), add:
```
deb https://nginx.org/packages/ubuntu/ bionic nginx
deb-src https://nginx.org/packages/ubuntu/ bionic nginx
```
4. Save the changes and quit.

5. Install NGINX:

```
sudo apt-get remove nginx-common

sudo apt-get update

sudo apt-get install nginx
```

6. Check NGINX Version:

```
nginx -v
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

**Congratulations, Now you server is up and running very well.**



