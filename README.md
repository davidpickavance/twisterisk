# twisterisk
To make this work, copy and paste the following onto the command line

sudo su  
yum -y install subversion git-core  
git clone https://github.com/davidpickavance/twisterisk.git  
cd twisterisk  
chmod +x twilioasterisk  

You will then run the twilioasterisk script to install asterisk and configure it to connect to your Twilio trunk.  

The script has two mandatory arguments and two optional arguments  
**Mandatory Arguments**
1. The first part of the termination URI for your Twilio trunk. So, if your Termination URI is mytrunk.pstn.twilio.com then the first argument is mytrunk
2. The second is the public IP address of your Asterisk server in dotted decimal notation, e.g. 1.2.3.4

**Optional Arguments**
If you have set Credentials on your Twilio trunk, you should provide the username and password  

So the command syntax is
twilioasterisk termination_uri ip_address [username password]

**Example 1**
I have created a trunk with
- A Termination URI of mytrunk.pstn.twilio.com
- No credentials

On a server with a public IP address of 1.2.3.4, I would run the following command    

./twilioasterisk mytrunk 1.2.3.4

**Example 2**
I have created a trunk with
- A Termination URI of othertrunk.pstn.twilio.com
- A username of myAsteriskUser
- A password of ElmTreesExtraveganza!1

On a server with a public IP address of 5.6.7.8, I would run the following command  

./twilioasterisk othertrunk 5.6.7.8 myAsteriskUser ElmTreesExtraveganza!1  

**Other Notes**
So far, I have only tested this on an EC2 instance running redhat and it is configured assuming that you are based on the USA. At some point, I will extend it to other OS optoins and other regions.
