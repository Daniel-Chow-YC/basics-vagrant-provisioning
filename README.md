# Sparta Node Sample App

## Description

This app is intended for use with the Sparta Global Devops Stream as a sample app. You can clone the repo and use it as is but no changes will be accepted on this branch.

To use the repo within your course you should fork it.

The app is a node app with three pages.

### Homepage

``localhost:3000``

Displays a simple homepage displaying a Sparta logo and message. This page should return a 200 response.

### Blog

``localhost:3000/posts``

This page displays a logo and 100 randomly generated blog posts. The posts are generated during the seeding step.

This page and the seeding is only accessible when a database is available and the DB_HOST environment variable has been set with it's location.

### A fibonacci number generator

``localhost:3000/fibonacci/{index}``

This page has be implemented poorly on purpose to produce a slow running function. This can be used for performance testing and crash recovery testing.

The higher the fibonacci number requested the longer the request will take. A very large number can crash or block the process.


### Hackable code

``localhost:3000/hack/{code}``

There is a commented route that opens a serious security vulnerability. This should only be enabled when looking at user security and then disabled immediately afterwards

## Usage

Clone the app

```
npm install
npm start
```

You can then access the app on port 3000 at one of the urls given above.

## Tests

There is a basic test framework available that uses the Mocha/Chai framework

```
npm test
```

The test for posts will fail ( as expected ) if the database has not been correctly setup.

## How to use this environment
- Download vagrant from vagrantup.com
- Download virtualbox from virualbox.org
- Make a new directory and move app to this directory
- From your terminal run ````vagrant init```` to create a vagrant file in this new directory
- In this vagrant file you can then configure the settings for your virtual environment. In general:
  - config.vm.box - operating system
  - config.vm.network - How your host sees your box
  - config.hostsupdater.aliases - How to give an alias for the IP
  - config.vm.synced_folder - How you access files from your local machine/computer
  on the VM (syncing your local folder with your vagrant box folder)
  - config.vm.provision - what we want to setup (automating the setup process)
  - eg:
 ````
 Vagrant.configure("2") do |config|

   config.vm.box = "ubuntu/xenial64"
   config.vm.network "private_network", ip: "192.168.10.100"
   config.hostsupdater.aliases = ["development.local"]

   config.vm.synced_folder "app", "/app"

   config.vm.provision "shell", path: "environment/provision.sh"


 end

 ````
- Use the xenial64 machine by inputting:```` config.vm.box = "ubuntu/xenial64"````
into the vagrant folder
- sync your folders by inputting:```` config.vm.synced_folder "app", "/app"````
into the vagrant folder
- Spin up the VM (create the VM) by running ````vagrant up```` in the terminal
- SSH into the box (make a connection to the VM) by running ````vagrant ssh````
- create a provision.sh file in the environment folder so that you can provision the machine
- input this into the provision.sh file:
````
#!/bin/bash
sudo apt-get update -y
sudo apt-get upgrade -y
sudo apt-get install nginx -y
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install nodejs -y
sudo npm install pm2 -g
cd /app
npm install
npm start
- In the vagrant file input:
config.vm.provision "shell", path: "environment/provision.sh"
````
- Run ````vagrant up```` to run the app.
