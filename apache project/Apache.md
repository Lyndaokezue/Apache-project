# Apache2 Web Hosting on Vagrant
I like to explain apache as a webserver, it is also a special kind of software called webserver, which means it helps to deliver whatever content you request. Take for instance, a search engine like google, safari, firefox etc a friendly affliate marketer who suggest similar things you like based on your search querry (for instance when you a search particular kind of thing on your webserver). The web browser example chrome which is **you** now sends the information to Apache2 and the sales person delivers what you have requested (what the web browser requested).

When you make a request to your web browser, the request goes to apache and apache gets information for it. The request doen't go directly to your browser. Interacting with web servers, like Apache2, is common when browsing the internet. When you click on a specific link or enter an address, Apache2 locates and returns the desired web page fast.
There's a strong probability that Apache2 is operating in the background whenever you visit a website, it doesn't take you to a website you didn't ask for.

# How to host a website with Apache2
In this project, I will be using Gitbash.

1. Run the command 

**sudo apt update**
This will update all packages on your system.

![Apt update](./apache%20screenshots/Aptupdate.png)
2. Run upgrade

**sudo apt upgrade**

This allows to install the needed packages

3. Check the status of your apache2. If it shows active, it means it has been installed, else do **sudo apt install apache2**

To check the status of apache2

**sudo systemctl status apache2**


![Apache status](./apache%20screenshots/apachestatus.png)

# FIREWALL
To understand firewall, think of that estate security personnel who monitors people that go out and come in. Yes, you guessed right, he monitors the right people to come in or familiar faces. Every operating system has a firewall which is automatically disabled by default.

Firewall simply monitors the incoming and outgoing connections. Enabling the firewall on Apache2 is similar to opening a door to allow visitors to view your website. The purpose of the firewall is to ensure that only authorized traffic can pass through the door (by permitting traffic on designated ports) and to keep out unauthorized users (by blocking other ports).

Therefore, configuring the firewall to permit traffic on the ports that Apache 2 uses to serve web pages requires installing Apache 2.

# To check the status of your firewall
To check the status of firewall on your system

**sudo ufw status**

![Firewall status](./apache%20screenshots/firewallstatus.png)


**ufw** stands for uncomplicated firewall.

If inactive, run the command below and check the status again. Firewall has been installed but you need to expressly enable it. Install, enable and check status again.

**sudo apt-get install ufw -y**

![install ufw](./apache%20screenshots/firewallinstall.png)

**sudo ufw enable**

Allow ssh traffic. When you allow ssh traffic, you're telling the firewall to allow incoming SSH connections.
To allow ssh on your server via firewall

![Firewall enable](./apache%20screenshots/firewallenable.png)


**sudo ufw allow openssh**


![openssh](./apache%20screenshots/firewallOpenssh.png)


To check the profile of firewall


**sudo ufw apt list**

![firewall profile](./apache%20screenshots/profilefirewall.png)


 Allow ‘Apache Full’ When you run this, you're telling the firewall to allow incoming connections for the Apache web server, and Apache full is open to port 80 and port 443.

 Get your system’s ip address and copy the first ip address under ‘enp0sB’, line 2: inet .

 **ifconfig**

![ifconfig](./apache%20screenshots/ifconfig.png)

 Run the ip address on a new tab in your web browser and this should be the output.

 ![port 80](./apache%20screenshots/apacheport80.png)

 # Vagrant web hosting
 1. Enter cd / to access your root directory, then use ls to list its contents.

 **cd /**

 ![cd /](./apache%20screenshots/rootdirctory.png)

 2. Var and etc are the two main directories that are important in Apache2 web hosting.

 But let's first take a look at etc folder (where all the configuration files are kept.)

 **cd etc**

 **cd apache2**

 ![etc directory](./apache%20screenshots/etcd.png)


 sites-available and sites-enable play an important role in web hosting. Site available is where all the configuration for the website we will be working with will be stored while sites-enable links the configuration files that are in site-available.

3. **cd** into the ‘sites-available’ directory.

**cd sites-available**

![sites available](./apache%20screenshots/available.png)


The files "default-ssl.conf" and "000-default.conf" are displayed in the directory but "000-default.conf" is the configuration of apache default browser. 

4. Display the content of the ‘000-default.conf’

**sudo cat 000-default.conf**


![000-default.conf](./apache%20screenshots/defaultApachepage.png)

To edit your own website configuration file, you must copy the contents of the default configuration file. "DocumentRoot and ServerAdmin" are essential. However, observe what's inside the 'var' folder before proceeding.

5. Go back to your root directory

**cd /**

![var](./apache%20screenshots/var.png)

THE **var** holds the website that you want to host. The ‘var’ folder contains the ‘www’ folder which is where your main code for your website is in like HTML, CSS, and images.

One html file is presently stored in the 'www' folder, as seen in the image above. This is where you should enter the configuration file for your cloned website. Put your intended website on Github and make a clone of it.

6. Go back to your home directory by typing just cd.

**cd**
It’s now time to clone the repository or url you want to host from Github.

![git clone](./apache%20screenshots/gitclone.png)


7. Go to your Github account and copy the url of the website you want to host. 

![github page](./apache%20screenshots/githubpage.png)

8. Go back to your VsCode terminal. Git clone the repository and list.

**git clone [url link]**

![git clone](./apache%20screenshots/gitclone.png)

9. Move your just cloned repository into your 'www' folder by using the absolute path after checking your current working directory. Recall that your website's files are stored there. (Css, html, and pictures)

**sudo mv /home/vagrant/ASSIGNMENT-1 /var/www**

Make sure the spelling and capitalization are accurate. In case the casing of your repository was pushed to Github with small letters, enter the repository title in small letters as well. Casing matters on the terminal.

10. Go to the /var/www folder and list. You should see your Github repository alongside the html file you saw previously.

**cd /var/www**

11. cd into the sites-available folder to copy the default configuration file for your website. 

12. Copy the default configuration file and list

**sudo cp 000-default.conf Assignment-1.conf**


![000-default.conf](./apache%20screenshots/defaultApachepage.png)


You now have your website’s configuration file. Edit it.

**sudo nano Assignment-1.conf**

13. You just edited and saved your website’s configuration file. You need to grant the necessary permissions. The current owners of the file is root because we used sudo but let's now change it to vagrant.

![nano assignment](./apache%20screenshots/webmaster.png)


**sudo chown -R $USER:$USER /var/www/Assignment-1**
**sudo chown -R 755 /var/www**

![chown](./apache%20screenshots/chown.png)

14. Disable the default Apache2 page you saw on your web browser when you entered your local machine’s ip address.

**sudo a2dissite 000-default.conf**


![disable](./apache%20screenshots/a2dissite.png)

15. Activate the new configuration by running:

**sudo systemctl reload apache2**

![reload apache](./apache%20screenshots/reloadapache.png)


16. enable your website configuration.

**sudo a2ensite Assignment-1**


![enable](./apache%20screenshots/a2ensite.png)

Activate the configuration.

Your website is live.