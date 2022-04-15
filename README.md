# Business Partner Agent

The BPA allows organizations to verify, hold, and issue verifiable credentials.

The Business Partner Agent is built on top of the Hyperledger Self-Sovereign Identity Stack, in particular Hyperledger Indy and Hyperledger Cloud Agent Python.

![(image)](https://i.postimg.cc/T1rFsz7j/BPA.jpg)

## Prerequistes
The following tools should be installed on your developer machine:
- Ubuntu 20.04 VM
- docker
- docker-compose

### Create virtual machine in Azure with Ubuntu 20.04 VM
1.	Type virtual machines in the search.
2.	Under Services, select Virtual machines.
3.	In the Virtual machines page, select Create and then Virtual machine. The Create a virtual machine page opens.
4.	In the Basics tab, under Project details, make sure the correct subscription is selected and then choose to Create new resource group. 
5.	Under Instance details, type your Virtual machine name, and choose Ubuntu 20.04 LTS - Gen2 for your Image. Leave the other defaults. The default size and pricing are only shown as an example. Size availability and pricing is dependent on your region and subscription.
6.	Under Administrator account, select SSH public key.
7.	In Username type azureuser.
8.	For SSH public key source, leave the default of Generate new key pair, and then type your Key pair name.
9.	Under Inbound port rules > Public inbound ports, choose Allow selected ports and then select SSH (22) and HTTP (80) from the drop-down.
10.	Leave the remaining defaults and then select the Review + create button at the bottom of the page.
11.	On the Create a virtual machine page, you can see the details about the VM you are about to create. When you are ready, select Create.
12.	When the Generate new key pair window opens, select Download private key and create resource. Your key file will be download as yourname.pem. Make sure you know where the .pem file was downloaded, you will need the path to it in the next step.
13.	When the deployment is finished, select Go to resource.
14.	On the page for your new VM, select the public IP address and copy it to your clipboard.
15. open ports: 8080, 8030, 8031, 8090

### Install Docker
```shell
sudo apt update
sudo apt upgrade
sudo apt install docker.io
docker --version
```
### Install docker-compose
```shell
sudo apt install curl
sudo curl -L "https://github.com/docker/compose/releases/download/v2.2.3/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```
exit and login again
```shell
sudo docker-compose -v
```
## Set up BPA:
```shell
git clone https://github.com/hyperledger-labs/business-partner-agent
cd scripts
./register-dids.sh
```
then you can edit .env file with below command:
```shell
nano .env
```
![(image)](https://i.postimg.cc/GpM482CN/env.jpg)
## Spinning up a single BPA
- For a local test, just run
```s
# If not done already, run
# ./register-dids.sh
docker-compose up
```

### Accessing the frontend

- The frontend will be served at `http://yourip:8080`. If you did not change the password in `.env` the default login is "admin"/"changeme".
- The backends swagger will be served at: `http://yourip:8080/swagger-ui`
- aca-py's swagger api will be served at: `http://yourip:8031/api/doc`

## Spinning up two local instances of the BPA

If you have run the `register-dids.sh` script you should have a .env file. In the file make sure the `BPA_SCHEME` is set to `BPA_SCHEME=http`.

Afterwards it is the same as above, but now we use a profile to enable a second instance

```s
# If not done already, run
# ./register-dids.sh
docker-compose --profile second_bpa up
```

### Accessing the second frontend
- The second frontend will be served at `http://yourip:8090`. If you did not change the password in `.env` the default login is "admin"/"changeme".
- The second backends swagger will be served at: `http://yourip:8090/swagger-ui`
- The second aca-py's swagger api will be served at: `http://yourip:8041/api/doc`

![(image)](https://i.postimg.cc/Bt90Xv09/BPA2.jpg)

### Stopping the instance

```s
docker-compose down
```
