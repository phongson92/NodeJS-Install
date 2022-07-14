1. Install jenkins as a docker image on Server
    a. Install docker 
        Change docker daemon, docker service to connect socket from external to internal docker

    
        docker0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.17.0.1  netmask 255.255.0.0  broadcast 172.17.255.255
        inet6 fe80::42:7aff:fe1d:3c30  prefixlen 64  scopeid 0x20<link>
    

     
        vi /lib/systemd/system/docker.service
       

     
        [Service]
            Type=notify
            # the default is not to use systemd for cgroups because the delegate issues still
            # exists and systemd currently does not support the cgroup feature set required
            # for containers run by docker
            ExecStart=/usr/bin/dockerd -H unix://var/run/docker.sock -H tcp://172.17.0.1 --containerd=/run/containerd/containerd.sock
        

        
        system daemon-reload &&  service docker restart
         

    b. Create jenkins user

        run add-docker-user.sh to create jenkins user
        run start-jenkins-server.sh to deploy jenkins docker image
        Before deploy jenkins docker image, check uid jenkins user

        
        root@:~# su - jenkins
        id
        uid=1000(jenkins) gid=1000(jenkins) groups=1000(jenkins),998(docker) 

    c. Install plugin on jenkins server

        a.	Docker Pipeline
        b.	Dokcer plugin
        c.	Git plugin
        d.	Gibhub branch source plugin
        e.	Github pull request builder
2. Install React JS Server
    ```
    apt-get update -y
    apt-get upgrade -y
    curl -sL https://deb.nodesource.com/setup_14.x | bash -
    sudo apt-get install nodejs -y
    node -v
    ```
    Once Node.js is installed, update the NPM to the latest version using the following command:
    ```
    npm install npm@latest -g
    npm install tar@6 -g
    ```
You can now verify the npm version with the following command:

```
npm -v
```
```
npm install -g create-react-app
create-react-app --version
create-react-app my-project
cd my-project
npm start 
or 
HOST=103.92.25.173 npm run start
```
Run React js background
```
npm install pm2 -g
pm2 logs
```
List all running ReactJS
```
pm2 ps
```
Stop a React JS with id
```
pm2 stop 0
```
Delete a React JS with id
```
pm2 delete 0
```
```
pm2 delete all
```
3. Config jenkins to auto deploy code
create a pem private key 
```
su - jenkins
cd /home/jenkins/data/.ssh/ && touch web-key.pem
chmod 400 web-key.pem
check permission, chown jenkins:root web-key.pem
```
coppy public_key to React JS server
```
cd /root/.ssh && touch authorized_keys
```
paste public key then save
Go into Jenkins docker image to check ssh login
```
ssh -o StrictHostKeyChecking=no -i /var/jenkins_home/.ssh/web-key.pem root@IP
```
Create github Repositories
ADD WEBHOOK TO AUTO BUILD WHEN COMMIT CODE TO GITHUB
setting -> webhooks -> add webhooks
http://IP-Jenkins:8080/github-webhook/

On Jenkins Server create a Freestyle project
GitHub project -> github project link
Source Code Management -> github project link
GitHub hook trigger for GITScm polling
Excute shell
```
ssh -o StrictHostKeyChecking=no -i /var/jenkins_home/.ssh/web-key.pem root@IP << EOF
cd /root/my-project/NodeJS-Install/
git pull origin master
pm2 delete all
pm2 --name TodoProject start npm -- start
```



