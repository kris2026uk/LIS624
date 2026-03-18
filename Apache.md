#The Apache Web Server

##The Apache Web Server is a web server software that makes files accessible on web browsers.

A web server or an 'http server' retrieves and displays files from web servers. 
This is an integral step in the LAMP set up and enables the content we create to be viewed by others on websites.

The first step is to install the software on your virtual machine. 

In order to install it you must follow these steps:

1. You must first ensure your repository is updated. Run the following commands to update your repository
   
- **sudo apt update**
- **sudo apt -y upgrade**

2. Once your machine is updated you need to search for the correct software, run the following commands to find
and download the correct software for Apache.

- **apt search apache2 | head**
  
  By using the '**search <package_name>**' and '**apt show <package_name>**', you can veiw and determine which package to download.
  Download the following:

- **sudo apt install apache2** 

3. Once you have downloaded the software, you must check to ensure that it is active and enabled correctly on your machine.
This means that it was downloaded correctly, is working, and will automatically start when rebooted. Use the following command to confirm this:

- **systemctl status apache2**

Next, you will need to download a command line browser using one of the following:

- sudo apt install w3m
- sudo apt install elinks

This will ensure you can check your websites by adding the address to point to a local machine. This can also be done by using the virtual machines IP address. 

You can run this command line by including the IP address for your vm like the following, do this to confirm it works:

- w3m 10.128.X.XX

You can also view your websites by using your default web page from your virtual machine and putting it in your web browser:

-http://IP-ADDRESS

**Next** *Its time to create your website*


  



