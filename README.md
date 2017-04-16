## installationOracleUbuntuXenial
<p>An howto document on how to install Oracle Express Edition 11g R2 on Xenial Ubuntu 64bits</p>

## Installation
<p>First, be update</p>
<h6>sudo apt-get update && sudo apt-get upgrade -y</h6>
<p>You need to install some paquets:</p>
 sudo apt-get install libaio1
sudo apt-get install libaio-dev unixODBC unixODBC-dev expat sysstat libelf-dev elfutils lsb-cxx pdksh libstdc++5 ia32-libs ksh lesstif2 alien gcc gawk binutils gawk x11-utils rpm alien lsb-rpm libmotif3 lesstif2 openssh-server libaio1 

Oracle needs certain utilities and libraries in others locations:
sudo ln -s /usr/bin/basename /bin/basename
sudo ln -sf /bin/bash /bin/sh
sudo ln -s /usr/bin/rpm /bin/rpm
sudo ln -s /usr/bin/awk /bin/awk
sudo ln -s /usr/lib/x86_64-linux-gnu/libc_nonshared.a /usr/lib64/ 
sudo ln -s /usr/lib/x86_64-linux-gnu/libpthread_nonshared.a /usr/lib64/ 
sudo ln -s /usr/lib/x86_64-linux-gnu/libstdc++.so.6 /usr/lib64/ 
sudo ln -s /lib/x86_64-linux-gnu/libgcc_s.so.1 /lib64
sudo ln -s /usr/lib/i386-linux-gnu/libpthread_nonshared.a /usr/lib/libpthread_nonshared.a

## Downloads
$ cd ~/Downloads
$ wget http://download.oracle.com/otn/linux/oracle11g/xe/oracle-xe-11.2.0-1.0.x86_64.rpm.zip
$ unzip oracle-xe-11.2.0-1.0.x86_64.rpm.zip 
$ cd Disk1/
$ sudo alien --scripts -d oracle-xe-11.2.0-1.0.x86_64.rpm

## Create the required chkconfig script using the command:

$ sudo pico /sbin/chkconfig
Paste:

#!/bin/bash
# Oracle 11gR2 XE installer chkconfig hack for Ubuntu
file=/etc/init.d/oracle-xe
if [[ ! `tail -n1 $file | grep INIT` ]]; then
    echo >> $file
    echo '### BEGIN INIT INFO' >> $file
    echo '# Provides: OracleXE' >> $file
    echo '# Required-Start: $remote_fs $syslog' >> $file
    echo '# Required-Stop: $remote_fs $syslog' >> $file
    echo '# Default-Start: 2 3 4 5' >> $file
    echo '# Default-Stop: 0 1 6' >> $file
    echo '# Short-Description: Oracle 11g Express Edition' >> $file
    echo '### END INIT INFO' >> $file
fi
update-rc.d oracle-xe defaults 80 01

Change file permission :
$ sudo chmod 755 /sbin/chkconfig  

Oracle 11gR2 XE requires additional kernel parameters:
$ sudo pico /etc/sysctl.d/60-oracle.conf

Paste:
# Oracle 11g XE kernel parameters 
fs.file-max=6815744  
net.ipv4.ip_local_port_range=9000 65000  
kernel.sem=250 32000 100 128 
kernel.shmmax=536870912 

Verify the change using the command:
$ sudo cat /etc/sysctl.d/60-oracle.conf 

Load the kernel parameters:
$ sudo service procps start

Verify the new parameters are loaded using:
sudo sysctl -q fs.file-max

You should see the file-max value that you entered earlier.
Set up /dev/shm mount point for Oracle. Create the following file using the command:
$ sudo pico /etc/rc2.d/S01shm_load

Paste:
#!/bin/sh
case "$1" in
start)
    mkdir /var/lock/subsys 2>/dev/null
    touch /var/lock/subsys/listener
    rm /dev/shm 2>/dev/null
    mkdir /dev/shm 2>/dev/null
*)
    echo error
    exit 1
    ;;

esac 

Change the file permissions:
$ sudo chmod 755 /etc/rc2.d/S01shm_load

Now execute the following commands:
$ sudo ln -s /usr/bin/awk /bin/awk 
$ sudo mkdir /var/lock/subsys 
$ sudo touch /var/lock/subsys/listener

Now, Reboot:
$ sudo shutdown -r now

## Oracle Installation

Install the oracle:
$ sudo dpkg --install oracle-xe_11.2.0-2_amd64.deb

## Oracle Configuration
<p>If you install Oracle in an Vitual Ubuntu machine, we will specify ports for accessing to Oracle, if not let options default. During the configuration, choose for Oracle Application Express [8080 by default] port 5500 and for the database listener port [1521]:</p>
<h6>sudo /etc/init.d/oracle-xe configure</h6>

## Environment variables
Setup environment variables by editting .bashrc file:
$ pico ~/.bashrc

paste:
export ORACLE_HOME=/u01/app/oracle/product/11.2.0/xe
export ORACLE_SID=XE
export NLS_LANG=`$ORACLE_HOME/bin/nls_lang.sh`
export ORACLE_BASE=/u01/app/oracle
export LD_LIBRARY_PATH=$ORACLE_HOME/lib:$LD_LIBRARY_PATH
export PATH=$ORACLE_HOME/bin:$PATH

Load the changes:
. ~/.profile

## Add yourself to DBA group

$ sudo addgroup YOURLOGIN dba
(alternative:sudo usermod -a -G dba YOURLOGIN)

## Start the Oracle 11gR2 XE:

$ sudo service oracle-xe start

## Create a regular user account in Oracle

Log as administrator
$ sqlplus sys as sysdba
$ create user YOURLOGIN identified by PASSWORD;
$ alter database open resetlogs;
$ grant connect, resource to YOURLOGIN;
$ exit;

Restart as a regular user:
$ sqlplus

## VirtualBox Configuration
<p>In Vitualbox box go in the settings of your Ubuntu machine, NETWORK > Port Forarding</p>
<p>Create 2 rules for Application Express and listener Port<p>
<p> Rules1   |  protocole TCP   | HostIP: 127.0.0.1   |  Host Port 2222   |  Guest IP: 10.0.2.15 | Guest Port 22</p>
<p> Rules1   |  protocole TCP   | HostIP: 127.0.0.1   |  Host Port 1521   |  Guest IP: 10.0.2.15 |  Guest Port 1521</p>
<p> Rules2   |  protocole TCP   | HostIP: 127.0.0.1   |  Host Port 8080   |  Guest IP: 10.0.2.15 |  Guest Port 8080</p>
<br />
<p>Now, you can access with: interface web http://127.0.0.1:8080/apex/</p>
<p>Or SQL*Plus</p>

## First Login at ORACLE XE

worspace: INTERNAL
login: ADMIN
password: the one you write during oracle-xe configuration

## If Oracle doesn t work... reload service
<h6>sudo /etc/init.d/oracle-xe reload<h6>

## Installation of SQL*Plus for Mac
<p>You can read: http://www.oracle.com/technetwork/topics/intel-macsoft-096467.html</p>
<p>Read installation at the end of this page</p>

## Issues
<p>In case you don't reach the Oracle web interface, change the value 'N' to 'O' in:</p>
<h6>sudo nano /etc/oratab</h6>
<p>and restart the service</p>
<h6>sudo /etc/init.d/oracle-xe restart</h6>
<br/>
<p>Testez aussi la commande: </p>
<h6>unset no_proxy</h6>
<br/>
Once your server have been restarted, your database may not start. To solve this issue, first check in /etc/oratabthat it has the 'Y' flag, if not, set it.
$ sudo gedit /etc/oratab
And replace N by Y
$ sudo /etc/init.d/oracle-xe start

# SQLPLUS
## Connection
$ sqlplus
Se connecter :
CONNECT
LOGIN:
PASS:

## Some tests

CREATE TABLE (emlpoyee varchr2(20) UNIQUE);
DROP TABLE employee;

# credits
<p>http://leandro26.webnode.com/products/installing-oracle-11g-on-linux-unbutu/</p>
<p>http://www.oracle.com/technetwork/topics/linux/xe-on-kubuntu-087822.html</p>
<p>https://blogs.oracle.com/opal/entry/the_easiest_way_to_enable</p>

