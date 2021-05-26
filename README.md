# Installation setup

This is the frontend of the kavishala. This is written using a React JS library called Next JS. This application is server side implementation of the frontend as it is beneficial for the SEO purpose.
To set it up you need to clone this repository by using the following command.

```shell
git clone https://github.com/avinash-tiwari/kavishala-ssr
```

This will download the project in the folder which you run this command in.
After the download is complete, go to the downlaoded folder and run the following command!

```shell
yarn install
```

It will install the necessary packages required by the project.

# Running the application

To run the application first you need to setup the local environment for the backend then you can go to the project directory of the frontend project and run the following command.

```shell
yarn dev
```

This will run the application in the development mode.

## Entry point

To setup the base URL for the application ot the URL to which the frontend should get connected to go to the _Fetch.js_ file, here you have to configure the following line in order to set it up with the backend on local or on the server.

```javascript
const base_url = "https://admin.kavishala.in";
// Point this to the URL you want the application to run the backend
```

# Production setup

In the production, there are two frontend projects running, the live and the beta version. Basically there are multiple branches of the same repository the master one is running the live version and the beta is running the beta version. Following are the branches and the location of the type of frontend running on the production, also these projects are served to the outside world using nginx proxy, location of the configuration files to which are:-

1. Live

   - Location:- /home/ubuntu/kavishala-ssr
   - Branch:- master
   - Nginx:- /etc/nginx/sites-enabled/kavishala-ssr

2. Beta
   - Location:- /home/ubuntu/kavishala-beta
   - Branch:- beta
   - Nginx:- /etc/nginx/sites-enabled/kavishala-beta

# Production release on the server

## Creating build

To create the build on the server first take the latest pull on the respective project then run the following command!

```shell
yarn build
```

## Restarting the services

To manage the frontend server processes we are using the supervisord on the server. We created service manager file for the live and the beta version in the following location:-

1. Live:- /etc/supervisor/conf.d/next.conf
2. Beta:- /etc/supervisor/conf.d/next_beta.conf

Generally, one can run the follwing command to restart the frontend services and reflect the changes on the server.

```
sudo supervisorctl restart <service-name>

# where <service-name> is the service name i.e., next or next_beta
```

But it doesn't reflect on the servers. So in order to reflect the changes we need to kill the existing processes so that supervisorctl can restart them automatically!

## Steps to kill the existing process

First find all the processes running using

```shell
ps aux | grep next
```

It will give the following result

```shell
root      1504  5.0  3.3 987756 134620 ?       Sl   Nov08 327:00 /usr/local/bin/node /home/ubuntu/kavishala-ssr/node_modules/.bin/next start
ubuntu   16636  0.0  0.0  14852  1060 pts/0    S+   17:59   0:00 grep --color=auto next
root     19161  0.1  2.2 944592 91664 ?        Sl   Nov10   3:17 /usr/local/bin/node /home/ubuntu/kavishala-beta/node_modules/.bin/next start -p 3001
```

Then we need to copy the process id (next to user i., numbers after root, ubuntu, etc.) to kill it using the following command

```shell
sudo kill -9 <process-id-copied>
```
