# **Take Your Node.js Project to The Production Environment (VPS/Dedicated Server).**

The following instructions are based on **Ubuntu**, the steps are the same for whatever Linux distribution you are going to use but the commands might be different.

## 1- Install Node.js.

There are several ways to install node.js

 **1. Ubuntu Packages:**
Not recommended at all because Ubuntu sucks at keeping their packages updated.

 **2. NodeSource Repository:**
 Very Recommended, Easy to install and Easy to update.
 
 **3. NVM (Node Version Manager):**
Used if you are going to install multiple node versions on your machine.

Based on the previous methods, I'll go with the second option.

1. Update your Ubuntu

`sudo apt update && sudo apt upgrade`

2. Install important packages for node and npm

`sudo apt install gcc g++ make build-essential curl -y`

3. Install node.js from NodeSource Repository

```
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Above command will install node.js version 12, if you want a previous version just replace 12 with desired node version e.g. (11, 10, 8)

This will install both node.js  and npm, to check their versions simply use.
```
node -v
npm -v
```

## Uninstall Node.js and NPM

**To uninstall node.js and npm simply use:**

`
sudo apt remove nodejs npm
`

this will uninstall node and npm but will keep your configuration files.
<br>
**To also delete your configuration files, use: **

`
sudo apt purge nodejs npm
`


## 2- Install PM2

**PM2 identifies itself as a "PM2 is a production process manager for Node.js applications with a built-in load balancer. It allows you to keep applications alive forever, to reload them without downtime and to facilitate common system admin tasks."**

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

`
pm2 start app.js
`

Your app is now daemonized, monitored and kept alive forever.

<br>
#### List/Manage applications:

`
pm2 list
`

![Process listing](https://github.com/unitech/pm2/raw/master/pres/pm2-list.png)

Managing apps is straightforward:

```
pm2 stop     <app_name|id|'all'|json_conf>
pm2 restart  <app_name|id|'all'|json_conf>
pm2 delete   <app_name|id|'all'|json_conf>
```

To have more details on a specific application:

```
pm2 describe <id|app_name>
```

To monitor logs, custom metrics, application information:

```
pm2 monit
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

`
pm2 reload <all|app_name>
`
<br>

#### Terminal Based Monitoring:

`
pm2 monit
`

![Monit](https://github.com/Unitech/pm2/raw/master/pres/pm2-monit.png)

<br>
#### Log Management:

`
pm2 logs
`

[More about log management](https://pm2.io/doc/en/runtime/guide/log-management/)

<br>
#### Startup Hooks Generation

PM2 can generates and configure a Startup Script to keep PM2 and your processes alive at every server restart.

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
npm install pm2@latest -g
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


