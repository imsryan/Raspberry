# Raspberry
#The initial shall be follow as below:
#1. Read the document--> https://www.raspberrypi.org/documentation/
#2. Prepare all the device (raspberry pi4, 4GB, miscroSD 32G, power cord, and Labtop)  
#3. Download necessary tools for running up the system (here are what I used in my environment
##a. Window 10 Home
##b. RJU166, Esense USB-RJ-45 converter (USD$ 8.0) --> https://www.e-sense.com.tw/download.php?lang=tw&tb=2&cid=29
##c. BalenaEtcher for wirting the raspbian image into microsd --> https://www.balena.io/etcher/
##d. Sublimetext for editing the file before setup --> https://www.sublimetext.com/3
#4. There is a "sequence" keep me crazy and always pops up a error and finally works after few times try and error finally, here are my briefly logs.
##a. I follow the Headless setup configuration as some contribution after suvey the internet, but not always perferc.
##b. I re-write the   EEPROM (https://www.raspberrypi.org/downloads/); Choose the "Misc Utility Images-->Raspberry Pi4 EEPROM boot recovery"
#5. I reload a new image files use the BalenaEtcher on the microSD; and creat a "ssh" file, modified the "comdline.txt" on the microSD under boot disc.
##a. The ssh file just open the Sublimetext to open up  and clike spack bar and save as "anyfile format.
##b. The comdline.txt, I add the fixed ip address just follow the original phrase (do not break-up) and the configuration shall as below==>

dwc_otg.fiq_fix_enable=2 console=ttyAMA0,115200 kgdboc=ttyAMA0,115200 console=tty1 root=/dev/mmcblk0p2 rootfstype=ext4 rootwait  ip=192.168.100.100
		
#6. After load in the new add-on/modified files, put the microSD and power on the raspberry, 
##a. Connect the raspberry with the Ethernet with fix ip address, here I used 192.168.100.1 in my pc and 192.168.100.100 on raspberry.
##b. It works perfect (been verified 3 times)
#7. Designated an Fixed IP address for the Raspberry Pi, as following command

sudo nano /etc/dhcpcd.conf
	
#Once openup the file scrow down to the bottom and add-on configuration as below:

interface eth0
static ip_address=192.168.100.100/24
static routers=192.168.100.1
static domain_name_servers=8.8.8.8
	
interface wlan0
static ip_address=192.168.10.148/24
static routers=192.168.10.148
static domain_name_servers=8.8.8.8
	
##the eth0 is to designate the IP of Ethernet Cable ; and  wlan0 is to designate the IP of wifi
	
#8. Change my login password, enable vnc,  and extpnd my filesystem; Use the command as below

sudo raspi-config

##And then choose the function as intend; as mentioned I change the login passowrd, enable vnc and extpend my filesystem

sudo reboot

##Reboot to load new configuration and check the system

#9. Update and upgrade the system

sudo apt-get update
sudo apt-get upgrade
sudo apt-get autoremove
	
#10. Ablove command can be done just via the ssh connection, and after that I turn the vncserver to login in to the system
#11. This is my raspberrry headless installation and configuration. 
#12. There is one interesting thing that I ever trying to follow some of the Pi players'  to pre-load the "wpa_supplicant.conf" after write the raspbian image into the microsd
##And I do follow the instruction to do, but it seems only 50%  success rate, I'd even try to re-structure the configuration ----> to put the "country code" in front
##My personnel result/feedback ==> better not to load the file in the beginning; just use the local connection as describe as previious item 5-b.
##And the following configuration was the one I put in my initial  "wpa_supplicant.conf" for refference. 

ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
update_config=1
country=TW
network={
    ssid="wifi-ssid-1"
    psk="Passowrd-1"
    priority=1
    id_str="Office"
}
network={
    ssid="wifi-ssid-2"
    psk="Passowrd-2"
    priority=2
    id_str="Private"
}
	
#13. To be continue
