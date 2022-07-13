Installation instructions
Node.js v18.x:
```
# Using Ubuntu
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt-get install -y nodejs
```
```
# Using Ubuntu
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
sudo apt-get install -y nodejs
```
```
# Using Ubuntu
curl -fsSL https://deb.nodesource.com/setup_14.x | sudo -E bash -
sudo apt-get install -y nodejs
```
```
# Using Ubuntu
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -
sudo apt-get install -y nodejs
```

```
# Using Ubuntu
curl -fsSL https://deb.nodesource.com/setup_current.x | sudo -E bash -
sudo apt-get install -y nodejs
```
Uninstall nodejs Ubuntu & Debian packages
To completely remove Node.js installed from the deb.nodesource.com package methods above:

```
# use `sudo` on Ubuntu or run this as root on debian
apt-get purge nodejs
rm -r /etc/apt/sources.list.d/nodesource.list
```
Step 1: Log in via SSH and Update your System
```
apt-get update -y
apt-get upgrade -y
```
Step 2. Install Nodejs and NPM
First, you need to install NodeJS since a ReactJS application can only run if NodeJS is installed on your server. Node.js is an open-source and cross-platform JavaScript runtime environment built on Chrome’s V8 JavaScript engine.

The simple and easiest way to install Node.js and npm is to install them from the Ubuntu default repository.

By default, the latest version of Node.js is not available in the Ubuntu 20.04 default repository. So you will need to add the Node.js official repository to your system.

Add the Node.js repository with the following command:
```
curl -sL https://deb.nodesource.com/setup_14.x | bash -
```
And install the Node.js from the added repositories running the following command:
```
sudo apt-get install nodejs -y
```
Once NodeJS has been installed, you can verify the installed version of Node.js with the following command:

node -v
You should get the following output:

v14.17.1
Once Node.js is installed, update the NPM to the latest version using the following command:
```
npm install npm@latest -g
npm install tar@6 -g
```
You can now verify the npm version with the following command:

```
npm -v
```
You should get the following output:

7.19.0
Step 3. Install Create-React-App Tool
Now install the create-react-app tool using NPM. This tool helps to set all tools required to create a new project in React.
```
npm install -g create-react-app
```
Check the version with:

```
create-react-app --version
```
You should get the following output:

4.0.3
Step 4: Create your ReactJS Application
You can create your ReactJS application with the following command:

```
create-react-app my-project
```
Once the installation is finished, you should see the following output:

Success! Created my-project at /root/my-project
Inside that directory, you can run several commands:

```
npm start
```
```
HOST=Your-IP npm run start (access UI external)
```
Starts the development server.

npm run build
Bundles the app into static files for production.

npm test
Starts the test runner.

npm run eject
Removes this tool and copies build dependencies, configuration files
and scripts into the app directory. If you do this, you can’t go back!

We suggest that you begin by typing:

cd my-project
```
npm start
HOST=103.92.25.173 npm run start
```
Happy hacking!
Once your project is created, change the directory to the ReactJS application:


```
cd my-project
```
Now you need to start your ReactJS application with the following command:

npm start
You should get the following output:

Compiled successfully!

You can now view my-project in the browser.
```
Local: http://localhost:3000
On Your Network: http://192.168.1.101:3000
```

Note that the development build is not optimized.
To create a production build, use npm run build.
By default, the ReactJS application starts on port 3000.

Step 5: Create a Systemd Service File for ReactJS App
Next, you need to create a systemd service file to manage the ReactJS service. You can create it with the following command:

nano /lib/systemd/system/reactjs.service
Add the following lines:
```
[Service]
Type=simple
User=root
Restart=on-failure
WorkingDirectory=/root/my-project
ExecStart=npm start -- --port=3000
```
Save and close the file, then reload the systemd service with the following command:
```
systemctl daemon-reload
```
Next, start the ReactJS service and enable it to start at system reboot with the following command:
```
systemctl start reactjs
systemctl enable reactjs
```
You can verify the status of the ReactJS service with the following command:

systemctl status reactjs
You should get the following output:
```
● reactjs.service
Loaded: loaded (/lib/systemd/system/reactjs.service; static; vendor preset: enabled)
Active: active (running)
Main PID: 66390 (npm start --por)
Tasks: 30 (limit: 2248)
Memory: 188.7M
CGroup: /system.slice/reactjs.service
├─66390 npm start --port=3000
├─66401 sh -c react-scripts start "--port=3000"
├─66402 node /root/my-project/node_modules/.bin/react-scripts start --port=3000
└─66409 /usr/bin/node /root/my-project/node_modules/react-scripts/scripts/start.js --port=3000
```
Step 6: Access ReactJS Web UI
Now, open your web browser and type the URL http://your-server-ip-address. You should see your ReactJS Application on the following screen:



