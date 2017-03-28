# installationOracleUbuntuXenial
<p>An howto document on how to install Oracle Express Edition 11g R2 on Xenial Ubuntu 64bits</p>
# Installation
<p>First, be update</p>
<h6>sudo apt-get update && sudo apt-get upgrade -y</h6>
<p>You need to install some paquets:</p>
<h6>$ sudo apt-get install libaoi1 bc </h6>
<p>Add in apt sources list the oracle repository</p>
<h6>sudo nano /etc/apt/sources.list</h6>
<p>add in the sources.list:</p>
<h6>deb http://oss.oracle.com/debian unstable main non-free<h6>
<p>and update</p>
<h6>sudo apt-get update</h6>
<p>Add the key of the repository</p>
<h6>wget http://oss.oracle.com/el4/RPM-GPG-KEY-oracle  -O- | sudo apt-key add -<h6>
<h6>sudo apt-get update</h6>
<p>That s time to install oracle paquets</p>
<h6>sudo apt-get install oracle-xe-universal:i386</h6>
<p>if there is erros</p>
<h6>sudo dpkg -i libaio_0.3.104-1_i386.deb</h6>
<h6>dpkg -i --force-architecture libaio_0.3.104-1_i386.deb</h6>
<h6>sudo dpkg -i --force-architecture libaio_0.3.104-1_i386.deb</h6>
<h6>sudo dpkg -i --force-architecture oracle-xe-universal_10.2.0.1-1.1_i386.deb</h6>
<h6>sudo apt-get install libc6-i386</h6>
<h6>sudo apt-get install -f</h6>
<h6>sudo dpkg -i --force-architecture oracle-xe-universal_10.2.0.1-1.1_i386.deb</h6>
#CONFIGURATION
<h6>sudo /etc/init.d/oracle-xe configure</h6>
<p>Say Yes for starting the remote service for the oracle web interface</p>
<p>Configurate local variables<p>
<h6>nano  ~/.bashrc</h6>
<p>Add:</p>
<h6>ORACLE_HOME=/usr/lib/oracle/xe/app/oracle/product/10.2.0/server
PATH=$PATH:$ORACLE_HOME/bin
export ORACLE_HOME
export ORACLE_SID=XE
export PATH<h6>
<p>You can work with the Oracle interface at</p>
<h6>http://127.0.0.1:8080/apex/f?p=4550</h6>
<br/>
<br/>
#Issues
<p>In case you don't reach the Oracle web interface, change the value 'N' to 'O' in:</p>
<h6>sudo nano /etc/oratab</h6>
<p>and restart the service</p>
<h6>sudo /etc/init.d/oracle-xe restart</h6>
<br/>
<p>Add your user to oracle group</p>
<h6>sudo addgroup nicolas dba</h6>
<br/>
<br/>
<p style="color:red;">ERREUR : The HTTP proxy server specified by http_proxy is not accessible</p>
<p style="color:red;">ERREUR : Pas d'interface web http://127.0.0.1:8080/apex/f?p=4550</p>
<p>Utilisez la commande: </p>
<h6>unset no_proxy</h6>
<h6>sudo /etc/init.d/oracle-xe reload<h6>

