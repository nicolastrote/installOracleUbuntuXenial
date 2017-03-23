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
<h6>sudo apt-get install oracle-xe-universal:i386 oracle-xe-client:i386</h6>
<p>if there is erros</p>
<h6>sudo apt-get install -f</h6>
<h6>sudo dpkg -i --force-architecture oracle-xe-universal_10.2.0.1-1.1_i386.deb</h6>
<h6>sudo /etc/init.d/oracle-xe configure</h6>
<p></p>
