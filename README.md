# <p align="center">ExMap
<p align="center">
<a href="https://www.extioninfotech.com/"><img src="images/extioninfotech.png" width="300px" alt="Extion Infotech"></a>
</p>

#
# <p align="center">Dependencies
This script is simply a wrapper for NMAP and Masscan. Install them from your
favorite package manager, or install from source.

<b>This tool is provided for educational and research purpose only. Extion infotech is not responsible for any misuse of this tool. 
We use it to test under NDA agreements with clients and their consents for pentesting purposes and we never encourage to misuse or take responsibility for any damage caused !
#
# Extion Infotech Scanner Tool - Exmap 
# This tools is developed by Extion infotech for testing and practicing of Cybersecurity interns 

The script utilizes Python's magical f-strings, so Python 3.6 or above
is required.

# <p align="center">Usage
Again, make sure that you have Python 3.6 or above installed. Simply executing
the script will prompt you for all of the required scanning options.

```
# ./exmap.py 

 ███████╗ ██╗  ██╗ ███╗   ███╗  █████╗  ██████╗ 
 ██╔════╝  ██ ██   ████╗ ████║ ██╔══██╗ ██╔══██╗
 █████╗     ███║   ██╔████╔██║ ███████║ ██████╔╝
 ██╔══╝    ██╔██   ██║╚██╔╝██║ ██╔══██║ ██╔═══╝ 
 ███████╗ ██║  ██║ ██║ ╚═╝ ██║ ██║  ██║ ██║     
 ╚══════╝ ╚═╝  ╚═╝ ╚═╝     ╚═╝ ╚═╝  ╚═╝ ╚═╝     By Extion Infotech       
    

Scan Type
	(1) Small Port Scan
	(2) Medium Port Scan
	(3) Large Port Scan
	(4) Extra Large Port Scan (Small, Medium, and Large)
	(5) Full Port Scan
	(6) Custom Port Scan

What type of scan would you like to perform (default: Small Port Scan)? 

Would you like to enumerate service banners for any identified services (default: Yes)? 

Target Scan
	(1) External
	(2) Internal

Is this an internal or external scan (default: External)? 

How fast would you like to scan (default: 20000 packets/second)? 

Example Target File
One CIDR or IP Address per line

	192.168.0.0/24
	192.168.1.23

Please enter the full path for the file containing target hosts (default: /opt/exmap/ranges.txt): 

Would you like to exclude any hosts?  (default: No) 

Scan Type: Small Port Scan
Target Ports: ['80', '443', '8000', '8080', '8008', '8181', '8443']
Service Banner: False
Source Port: 53
Masscan Max Packet Rate (pps): 2000
Target File: ranges.txt
Exclusions File: exclusions.txt

Scanning port 80...
```
You can also create a configuration file to avoid all of the prompts. Use the
provided 'config.json.sample' as an example. Just make sure that your file 
is named 'config.json'
```
# cat config.json
{
    "__scan_type_choices__" : "Small Port Scan, Medium Port Scan, Large Port Scan, Extra Large Port Scan, Full Port Scan, Custom Port Scan", 
    "scan_type" : "Custom Port Scan", 
    "dest_ports" : ["80","443","8000","8080","U:53"],
    "__banner_scan_choices__" : "True, False", 
    "banner_scan" : "True", 
    "__target_scan_choices__" : "External, Internal", 
    "target_scan" : "Internal",
    "__max_rate_external_recommedation__" : "Single Port = 20000, Full Port = 10000", 
    "__max_rate_internal_recommedation__" : "Single Port = 2000, Full Port = 1000", 
    "max_rate" : "2000",
    "target_file" : "ranges.txt",
    "output_path" : "./",
    "exclusions_file" : "exclusions.txt"
}
```
Note: To perform UDP scans simply prepend 'U:' to the port you'd like to scan (i.e 'U:53').
#### config.json Parameters
##### scan_type
This paramater is used to determine what ports to scan.
* Small Port Scan
    * 80, 443, 8000, 8080, 8008, 8181, 8443
* Medium Port Scan
    * 7001, 1433, 445, 139, 21, 22, 23, 25, \
    53, 111, 389, 4243, 3389, 3306, 4786, \
    5900, 5901, 6379, 6970, 9100
* Large Port Scan
    * 1090, 1098, 1099, 10999, 11099, 11111, \
    3300, 4243, 4444, 4445, 45000, 45001, \
    47001, 47002, 4786, 4848, 50500, 5555, \
    5556, 6129, 6379, 6970, 7000, \
    7002, 7003, 7004, 7070, 7071, \
    8001, 8002, 8003, 8686, 9000, \
    9001, 9002, 9003, 9012, 9503
* Extra Large Port Scan
    * Small, Medium, and Large Ports Combined
* Full Port Scan
    * 1-65,535
* Custom Port Scan
    * Dealer's Choice
##### dest_ports
* Only used if 'Custom Port Scan' is selected.
##### banner_scan
This parameter is used to determine whether NMAP will be used
to grab service banners.
* True
* False
##### target_scan
This paramater is used to determine what source port to spoof.
* External Port Scan
    * source port = 53
* Internal Port Scan
    * source port = 88
##### max_rate
This parameter is used to determine how fast to scan in masscan.
If it is not set manually, it is determined from the
scan_type and target_scan parameters.
**Note: Selecting a max_rate that is too high can easily create
a denial-of-service. In my testing, the following rates have been
found to be safe. YMMV**
* 'External' and 'Common Port Scan'
    * max_rate = 20,000 packets/second
* 'External' and 'Full Port Scan'
    * max_rate = 10,000 packets/second
* 'Internal' and 'Common Port Scan'
    * max_rate = 2,000 packets/second
* 'Internal' and 'Full Port Scan'
    * max_rate = 1,000 packets/second
* Everything else
    * max_rate = 2,000 packets/second
    
## Potential Hacks to Look For  

1090, 1098, 1099, 4444, 11099, 47001, 47002, 10999  
Java RMI  
https://www.rapid7.com/db/modules/exploit/multi/misc/java_rmi_server  
https://medium.com/@afinepl/java-rmi-for-pentesters-structure-recon-and-communication-non-jmx-registries-a10d5c996a79  
https://medium.com/@afinepl/java-rmi-for-pentesters-part-two-reconnaissance-attack-against-non-jmx-registries-187a6561314d  

7000-7004, 8000-8003, 9000-9003, 9503, 7070, 7071  
WebLogic  
https://www.exploit-db.com/search?q=weblogic  
  
45000, 45001  
JDWP  
https://www.rapid7.com/db/modules/exploit/multi/misc/java_jdwp_debugger  
https://github.com/IOActive/jdwp-shellifier  
  
8686, 9012, 50500  
JMX  
https://www.rapid7.com/db/modules/exploit/multi/misc/java_jmx_server  
  
4848  
GlassFish  
https://www.rapid7.com/db/modules/auxiliary/scanner/http/glassfish_traversal  

11111, 4444, 4445  
JBoss  
https://www.rapid7.com/db/modules/auxiliary/scanner/http/jboss_vulnscan  
https://github.com/joaomatosf/jexboss  
  
4786  
Cisco Smart Install  
https://www.rapid7.com/db/modules/auxiliary/scanner/misc/cisco_smart_install  
https://github.com/Sab0tag3d/SIET  
  
5555, 5556  
HP Data Protector  
https://www.rapid7.com/db/modules/exploit/multi/misc/hp_data_protector_exec_integutil  
https://www.rapid7.com/db/modules/exploit/windows/misc/hp_dataprotector_cmd_exec  

3300  
SAP  
https://github.com/chipik/SAP_GW_RCE_exploit  

6129  
Dameware  
https://www.tenable.com/security/research/tra-2019-43  
https://github.com/tenable/poc/blob/master/Solarwinds/Dameware/dwrcs_dwDrvInst_rce.py  
  
6379  
Redis  
https://www.rapid7.com/db/modules/exploit/linux/redis/redis_replication_cmd_exec  
  
6970  
Cisco Unified Communications Manager
https://github.com/trustedsec/SeeYouCM-Thief
http://[CUCM IP Address]:6970/ConfigFileCacheList.txt  
  
8080  
Adobe CodFusion BlazeDS  
https://www.tenable.com/plugins/nessus/99731  

<h2 align="center">
  <a href="https://docs.google.com/forms/d/e/1FAIpQLSevJAasjy4LYz9iuAzcTZu7oppCqni7uoVxzVSDm5pNVp17ww/viewform?pli=1" target="_blank">Extion Infotech Internships</a>
</h2>

<p align="center">
We offer a wide range of services, including:

Trainings – Industry-focused skill development programs.
Internships – Hands-on experience in various technical domains.
Staffing & Placements – Career assistance and recruitment solutions.
Software & Web Development – Custom software, web, and app development.
Digital Marketing & Promotion – SEO, social media marketing, and lead generation.
Branding & Design – Creating strong and memorable brand identities.

# <p align="center">You can click below image
</p>

<p align="center">

<a href="https://docs.google.com/forms/d/e/1FAIpQLSevJAasjy4LYz9iuAzcTZu7oppCqni7uoVxzVSDm5pNVp17ww/viewform?pli=1" target="_blank">
  <img src="images/image.png" alt="Form Image" style="max-width: 100%; height: auto;">
</a>

### Credit's
Copyright © 2025 by [Extion Infotech](https://extioninfotech.com)