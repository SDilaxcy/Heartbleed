# Heartbleed
This Heartbleed vulnerability set-up/exploit/patche was done for Cyber module assingment 1. 

# Environment Setup
Download bee-box VM that contain BWAPP .Link to download bee- box https://sourceforge.net/projects/bwapp/files/bee-box/bee-box_v1.6.7z/download

Follow the usual set up inside oracle virtual box 

Click new, Name the VM like Bee Box , Type will be Linux and version Ubuntu 64 bit and then press next for hard disk select use existing hard disk . You should extract the downloaded bee-box and select bee-box.vmdk and then hit start .

Start the VM and open terminal and type ifconfig to get the IP address .

Inside the VM, desktop directory you have bWAPP and open it in browser .Login with username : bee and password :bug
At top right you will see choose your bug , inside that search for Heartbleed vulnerability  and hit hack button .

Make sure the login page is running in port 8443 .if not make it https:// [IP address] : 8443

In both VM s , go to devices and the select network setting and make it host only adapter  .

# Create web server on 12.04

Download kali Linux 64 bit or 32 bit  from chrome .Do same process like bee box in order to setup kali VM . Use this link https://www.kali.org/get-kali/#kali-virtual-machines

# Exploit 

Open terminal in kali Linux environment and enter ping with target IP address we get by using ifconfig .By using this we can make sure both servers can create communication.

Check whether the SSL version is vulnerable to Heartbleed using Nmap . type sudo nmap -p [port] –script ssl-heartbleed [target IP address]

We are going use Metasploit frame work to exploit Heartbleed . type apt install metasploit-framework to download the frame work . and enter mfconsole to go inside the console .

Use the built-in search function of the Metasploit Framework to find the Heartbleed module, then choose the first auxiliary module that I highlighted. And load module selected. 

Search Heartbleed 
Use auxiliary/scanner/ssl/openssl_heartbleed 

Enter show options to view few important details like rhost ,port and TLS 
We are going to set rhosts [target IP address] and set rport [8443]
Enter show info , it expands with functionality of actions  (DUMP,KEYS,SCAN)

Type set action SCAN, and type run  it will return this test of the server is vulnerable to Heartbleed 3 Type set action DUMP, and type run it will dump the memory leak to bin file open another terminal and go to the path of the bin file and type strings [bin file name] .which will return dump where we can find few sensible data . Its not possible to get sensible data at one exploitation . when you repeating the process you may access data like username password , session cookies and all .

Another way is exploit it by using python script .download the python script attachedhttps://github.com/SDilaxcy/Heartbleed/blob/4102f2b36d1e9aa64ba5b6e398f78e34bdc6a2ce/Heartbleed.py
by using this you can open dump in to terminal window .I have added some python script which allow you to make multiple heartbeat request at a  time al response will append into one file .so you wont to do repeat attack to exploit it . At the very first attempt you will able to get the login credentials. 

# Patch 

We are going to update open SSL version .(any version above 1.0.1 is not vulnerable to heartbleed). In order to update the version follow code snippets below ,
sudo apt-get update && sudo apt-get upgrade
sudo apt-get install openssl
In order to check  weather the version is updated run openssl version -a command and inspect it .
if it is successsfully updated try attck again and see ,it wont be vulnerable to heartbleed 

# references 

https://hakin9.org/detecting-and-exploiting-the-openssl-heartbleed-vulnerability/
https://www.ehacking.net/2020/02/how-to-exploit-heartbleed-using-metasploit-in-kali-linux.html
https://www.youtube.com/watch?v=SgJm0C6jzbo&t=1s
