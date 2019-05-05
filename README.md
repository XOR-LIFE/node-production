# **Take Your Node.js Project to The Production Environment (VPS/Dedicated Server).**

The following instructions are based on **Ubuntu**, the steps are the same for whatever Linux distribution you are going to use but the commands might be different.

## 1- Install Node.js.

There are several ways to install node.js

 **1. Ubuntu Packages**
Not recommended at all because Ubuntu sucks at keeping their packages updated.
 **2. NodeSource Repository**
 Very Recommended, Easy to install and Quickly updated.
 **3. NVM (Node Version Manager)**
Used if you are going to install multiple node versions on your machine.

Based on the previous methods, I'll go with the second option.

1. Update your Ubuntu

`sudo apt update && sudo apt upgrade`

2. Install important packages

`sudo apt install gcc g++ make build-essential curl -y`

3. Install node.js from NodeSource Repository

```
curl -sL https://deb.nodesource.com/setup_12.x | sudo -E bash -
sudo apt-get install -y nodejs
```

Above command will install node.js version 12, if you want a previous version just replace 12 with desired node version e.g. (11, 10, 8)

This will install both node.js  and npm, to check their version simple use.
```
node -v
npm -v
```

## 2- Install PM2

**PM2 identifies itself as a "Node.js Production Process Manager with a built-in Load Balancer."**

The main feature of PM2 is to start your node.js app if your node app suffered any crash or if your VPS suffered an unexpected shutdown and restarted, therefore, you won't have to worry about manually starting your node app again.

Additionally, PM2 is more than that. It's capable of running several node apps at the same time with independent management for each one of them, Also it allows you to duplicates the processes to use all the cores for your CPUs (as you know that node.js is monothreaded).

**Install PM2**
```
sudo npm install -g pm2
pm2 completion install
pm2 -v
```

** How to:- **

**Start and Daemonize any application:**
`pm2 start app.js`

**Stop any application:**
`pm2 stop app`

**Make pm2 auto-boot at server restart:**
`pm2 startup`

**List applications:**
`pm2 list`

**Reload your app with 0-second-downtime:**
`pm2 reload app`

### **As Node.js is monothreaded, PM2 can duplicate the processes to use all the cores for your CPUs.**

![](https://pm2.io/_nuxt/img/1f13170.png)

**Start and load balance 4 instances of app.js:**
`pm2 start app.js -i 4`

**Or load balance the maximum:**
`pm2 start app.js -i max`

**Monitor in production:**
`pm2 monitor`


**PM2 Documentation:
https://pm2.io/doc/en/runtime/overview/**