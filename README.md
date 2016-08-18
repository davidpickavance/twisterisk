# twisterisk
This script helps you to quickly install Asterisk on a compatible server and get it working with a Twilio SIP trunk.

## Disclaimers  
No claims are made about the reliability or security of any phone service created using this script. You use it at your own risk.  

This script is not supported by Twilio.  

## Assumptions
- You have a Twilio account, have created an Elastic SIP trunk and know how to configure it
- You have a server that can run this script
- You have correctly configured all firewalls to allow the passage of SIP and RTP traffic

## Compatibility

This script has been tested with the following OSs
- RedHat

## Installation
Copy and paste the following commands into the CLI of your server

sudo su  
yum -y install subversion git-core  
git clone https://github.com/davidpickavance/twisterisk.git  
cd twisterisk  
chmod +x twilioasterisk  

## Running the script

You will then run the twilioasterisk script to install asterisk and configure it to connect to your Twilio trunk.  

The script has two mandatory arguments and two optional arguments. Please note that the last thing the script does is create certificates for TLS. This step has not yet been fully automated and you will need to watch for the prompts and enter the same password four times.  

### Mandatory Arguments  
1. The first part of the termination URI for your Twilio trunk. So, if your Termination URI is mytrunk.pstn.twilio.com then the first argument is mytrunk  
2. The second is the public IP address of your Asterisk server in dotted decimal notation, e.g. 1.2.3.4  

### Optional Arguments  
If you have set Credentials on your Twilio trunk, you should provide the username and password  

So the command syntax is  
twilioasterisk termination_uri ip_address [username password]  

### Examples  
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

## Using the PBX  
If you look in sip.conf, located in /etc/asterisk, you will see two endopints have been created for you - 1001 and 1002. You can see their SIP password in sip.conf

When a call comes in to the Asterisk, it will hit an IVR, asking you to select sales or support. One option forwards the call to 1001, the other forwards the call to 1002.  

If either endpoint cannot pick up, the call will roll over to voicemail.  

## Customizing
The default configuration used by this script assumes that you are in the US. In case you are not, it also comes with a French dialplan and a UK dialplan. If you'd rather use one of these, rename the existing extensions.conf to something like extensions.conf.us, copy the dialplan you want into /etc/asterisk and rename it to extensions.conf. You'll then need to run "dialplan reload" from the Asterisk CLI

If you want to add new extensions, you'll need to add them to sip.conf, voicemail.conf and extensions.conf. Remember to reload each, via the Asterisk CLI, after editing them.

If you want to change the IVR, you will find more pre-recorded sounds that you can use in /var/lib/asterisk/sounds
