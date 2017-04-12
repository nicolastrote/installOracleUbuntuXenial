# credits
<p>http://www.oracle.com/technetwork/topics/linux/xe-on-kubuntu-087822.html</p>
<p>https://blogs.oracle.com/opal/entry/the_easiest_way_to_enable</p>

## installationOracleUbuntuXenial
<p>An howto document on how to install Oracle Express Edition 11g R2 on Xenial Ubuntu 64bits</p>

## Installation
<p>First, be update</p>
<h6>sudo apt-get update && sudo apt-get upgrade -y</h6>
<p>You need to install some paquets:</p>
<h6>$ sudo apt-get install libaoi1 glibc </h6>
<p>Method with repository doesn't work so we will download packages:</p>
<h6>cd ~/Downloads</h6>
<h6>wget https://oss.oracle.com/debian/dists/unstable/main/binary-i386/libaio_0.3.104-1_i386.deb</h6>
<h6>wget https://oss.oracle.com/debian/dists/unstable/non-free/binary-i386/oracle-xe-universal_10.2.0.1-1.1_i386.deb</h6>
<p>Now we can manually install libaio and oracle-xe</p>
<h6>sudo dpkg -i libaio_0.3.104-1_i386.deb<h6>
<h6>sudo dpkg -i oracle-xe-universal_10.2.0.1-1.1_i386.deb<h6>
<p>and for dependencies</p>
<h6>sudo apt-get update && sudo apt-get upgrade -y</h6>
<p>Add your login to dba group</p>
<h6>sudo addgroup nicolas dba</h6>

## Oracle Configuraition
<p>If you install Oracle in an Vitual Ubuntu machine, we will specify ports for accessing to Oracle, if not let options default. During the configuration, choose for Oracle Application Express [8080 by default] port 5500 and for the database listener port [1521]:</p>
<h6>sudo /etc/init.d/oracle-xe configure</h6>

## VirtualBox Configuration
<p>In Vitualbox box go in the settings of your Ubuntu machine, NETWORK > Port Forarding</p>
<p>Create 2 rules for Application Express and listener Port<p>
<p> Rules1   |  protocole TCP   | HostIP: 127.0.0.1   |  Host Port 1521   |  Guest Port 1521</p>
<p> Rules2   |  protocole TCP   | HostIP: 127.0.0.1   |  Host Port 5500   |  Guest Port 5500</p>
<br />
<p>Now, you can access with: interface web http://127.0.0.1:5500/apex/</p>
<p>Or SQL*Plus</p>

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
