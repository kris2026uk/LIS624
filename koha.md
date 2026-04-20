# Koha

The last step to build our library website is to install and link up Koha to our other programs.

The first step is to create a new VM that will be able to run this program. It requires more memory than our initial VM has. This VM is an additional VM, so we do not have to copy our current one, but there are some things we will need to install again on the new one. 

We need to make it the same system as before, only larger.

## Create an additional VM

Go to create new VM and use the following:

- Name your VM - (I used v.3 for mine) 
- Machine Configuration: e2-medium (2vCPU, 1 core, 4 GB memory)
- Ubuntu 24.04 lts minimal 86/64  at 20 gb
- Allow HTTP traffic 

**Ensure the following Network tags are added!! Your VM will not connect if they are not added**

- koha-staff-8080
- koha-opac-8081
- SAVE 

### Add Firewall

We need to create two new firewall rules in order for our staff and patrons can access the different sites.

- Open VPC network on Google Cloud
- Click Firewall

**Create Firewall Rule no. 1**

- Add the name koha-staff-8080
- Add the Description Open port 8080 for the Koha staff interface
- Click on SPeciified target tags
- In Target tags add - koha-staff-8080
- Source IPv4 - allow access from anywhere - add (0.0.0.0/0)
- Click on Specified protocols and ports
- Click on TCP
- Add 8080 into the ports box
- Click create

**Create Firewall Rule no. 2**

- Add the name koha-opac-8081
- Add the Description Open port 8081 for the Koha OPAC interface
- Click on SPeciified target tags
- In Target tags add - koha-opac-8081
- Source IPv4 - allow access from anywhere - add (0.0.0.0/0)
- Click on Specified protocols and ports
- Click on TCP
- Add 8080 into the ports box
- Click create

### Open your new VM

Use the tmux command to keep your vm running in case it disconnects.
If it does disconnect, use tmux attach to get back to where you were before. 

updates

download edit
wget https://github.com/microsoft/edit/releases/download/v1.2.1/edit-1.2.0-x86_64-linux-gnu.tar.zst
tar -xf  edit-1.2.0-x86_64-linux-gnu.tar.zst
sudo install -m 0755 edit /usr/local/bin/edit
