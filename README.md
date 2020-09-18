# Rolling releases with Ansible

Your task is creating an ansible playbook that manages zero-downtime deployment of a sample Node.js application.

## Setting up
Provided `Vagrantfile` at rolling-release.tgz defines 5 boxes by default:
* A `command` box from which you will run ansible (hostname: command, ip: 10.0.15.10)
* A `lb` box that will serve as load balancer (hostname: lb, ip: `10.0.15.11`)
* 3 `app` boxes (`app1`, `app2`, `app3`) that will serve the application itself (hostname: appN, ip: 10.0.15.2N)

Your initial task is:
* Set up a `nginx` load balancer
* Deploy [sample Node.js application](https://bitbucket.org/ZaRDaK/devops-rolling-release) to app servers
* Wire everything up

## Rolling release
1. Take app server off load balancer
2. Stop the application
3. Update the application
4. Start the application
5. Ensure the application started correctly
6. Add app server to load balancer

If any step fails, abort the whole play

## Test rolling-release application

Dependencies: * Node.js binary >= 7.0.0

Installation: 1. Clone this repository, each release version is tagged with vX.X.X, i.e. v1.0.0 2. Fetch dependencies with npm install 3. Ensure all required environmental variables defined in .env.example are present 4. Run node index.js or npm start

Available versions: 1. v1.0.0 - Initial release 2. v1.1.0 - Simulate a failed release, process will exit after one second 3. v2.0.0 - Simulate a fixed release, everything back to normal


_______________________________



This project builds a [Node.js](https://nodejs.org/)-based API app inside a VM. It is meant as a demonstration of application deployments (in this case, a Node.js-based app) using Ansible.

## Building the VM

  1. Download and install [VirtualBox](https://www.virtualbox.org/wiki/Downloads).
  2. Download and install [Vagrant](http://www.vagrantup.com/downloads.html).
  3. Run `vagrant up` to build the VM and deploy the version of the app specified in `playbooks/vars.yml`.
  

$ vagrant status
Current machine states:

command                   running (virtualbox)
lb                        running (virtualbox)
app1                      running (virtualbox)
app2                      running (virtualbox)
app3                      running (virtualbox)


Once the VM is built, you can test the API by running the following command from your ansible server (command)

Ansible server is "command" machine.

root@command:/home/vagrant# for i in {1..100}; do curl -s  http://10.0.15.11/ -H "Accept: application/json"; echo "" ; done
{"app":"10.0.15.22","version":"1.0.0","servedIn":254.1687568392561}
{"app":"10.0.15.23","version":"1.0.0","servedIn":466.9025316433135}
{"app":"10.0.15.21","version":"1.0.0","servedIn":1928.0297655909994}
{"app":"10.0.15.22","version":"1.0.0","servedIn":1402.5200426933711}
{"app":"10.0.15.23","version":"1.0.0","servedIn":1674.6856394978056}
{"app":"10.0.15.21","version":"1.0.0","servedIn":846.2282194309472}
{"app":"10.0.15.22","version":"1.0.0","servedIn":599.4317230914822}
{"app":"10.0.15.23","version":"1.0.0","servedIn":295.121856359692}



To start provision or deploy use command:
ansible@command:~/deployments-rolling$ ansible-playbook  playbooks/provision.yml -i ./inventory

To start rolling deploy use commands:
ansible@command:~/deployments-rolling$ ansible-playbook  playbooks/deploy.yml -i ./inventory  -K 
ansible@command:~/deployments-rolling$ ansible-playbook  playbooks/deploy.yml -i ./inventory  -K  --step -v




Installation: 1. Clone this repository, each release version is tagged with vX.X.X, i.e. v1.0.0 
2. Fetch dependencies with npm install 
3. Ensure all required environmental variables defined in .env.example are present 
4. Run node index.js or npm start

Available versions: 
1. v1.0.0 - Initial release 
2. v1.1.0 - Simulate a failed release, process will exit after one second 
3. v2.0.0 - Simulate a fixed release, everything back to normal

