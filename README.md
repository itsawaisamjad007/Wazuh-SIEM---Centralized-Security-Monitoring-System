# Wazuh-SIEM---Centralized-Security-Monitoring-System

## Overview :
In this project I documented how I first time I set up my Wazuh SIEM Lab and what incident happen during this phase , and which error I faced and coped with them practically without lose my heart and with great patience .


## Disclaimer :
All this practical activities performed in my own virtual lab and it is only for
ethical and educational purpose . Nothing happened unethically in this lab.

## Tools I used and set up initially for my Lab :

| Tool | Purpose |
| ---- | ------- |
| Window 10 | As a host OS |
| Oracle Virtual Box | For Virtual Setup |
| Kali VM | As a Attacker Machin |
| Ubuntu Server VM | As a victim serve |
| Wazuh Server VM | As a server machine |

## Screenshot of my Practical Lab

![](/Wazuh-SIEM---Centralized-Security-Monitoring-System/screenshots/My%20Lab.png)

## Which steps I followed :

* First of all I download kali ova file from thier official website for oracle virtual box which was initially installed . Before it I had alredy installed kali 
linux manually with ISO image and got practice in its basic commands and features 
manually . Because I knew installation process of kali manually and did it many time 
so i choose to install its pre-installed ova file and import it in virtual box

* Then I installed ubuntu server 24.04 LTS from its official website for wazuh SIEM.

* In the same way I download wazuh server OVA file and import it in virtual box .Before download it I tried many time and I faced failure number of time , First of
all I tried to install wazuh in ubuntu server I installed it in ubuntu many time
### Screenshot of installed wazuh in ubuntu server :

![](/Wazuh-SIEM---Centralized-Security-Monitoring-System/screenshots/01%20Wazuh%20install%20complete.png)



But
unfortuanately when I came about to open wazuh in browser it show incorrect password 
or user even the password was correct any time . So i directly installed its OVA file
from a trusted site .

* Then I installed wazuh-agent in Ubuntu Server

![](/Wazuh-SIEM---Centralized-Security-Monitoring-System/screenshots/06%20wazuh%20agent%20install%20in%20ubuntu%20server.png)

* Setting wazuh-agent in ubuntu server

![](/Wazuh-SIEM---Centralized-Security-Monitoring-System/screenshots/07%20setting%20wazuh%20agent%20in%20ubuntu%20server.png)

* After installing wazuh I faced wazuh-manager error many time.

### Screenshot of Error I Faced

![](/Wazuh-SIEM---Centralized-Security-Monitoring-System/screenshots/02wazuh-manager%20error.png)

* After Solving wazuh-manager error I faced api error during .

### Screenshot
![](/Wazuh-SIEM---Centralized-Security-Monitoring-System/screenshots/03%20Api%20error%20in%20wazuh%20dashboard%20in%20browser.png)

* To solve api error I did many steps I restart Wazuh-indexer , wazuh-manager
, wazuh-dashboard and Maximize the RAM given to wazuh server .

* After setting up these error and wazuh-agent successfully I open dashboard.

### Screenshot
![](/screenshots/08%20first%20view%20of%20ubuntu%20server%20agent%20in%20wazuh%20dashboard.png)

* To see events in dashboard I did small event in ubuntu server

### Screenshot
![](/Wazuh-SIEM---Centralized-Security-Monitoring-System/screenshots/09%20first%20configuration%20in%20ubuntu%20agent%20to%20see%20events%20in%20wazuh.png)

* Then I use a filter in wazuh to seperate ubuntu events .

### Screenshot
![](/Wazuh-SIEM---Centralized-Security-Monitoring-System/screenshots/10%20adding%20filter%20for%20ubuntu%20server.png)

* Then I saw events initially which was not give proper result because I was only
setting up and testing this lab for first time 

### Screemshot 
![](/Wazuh-SIEM---Centralized-Security-Monitoring-System/screenshots/11%20events%20of%20ubuntu%20server%20in%20dashboard.png)


## Top concepts which clear during this project are :
*
*