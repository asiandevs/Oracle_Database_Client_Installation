# oracleclient_install

Note: Please modify all necessary configuration files based on your own environment.

This article describes the installation of Oracle Database Client 19c 64-bit on Oracle Linux 7 (OL7) 64-bit.

Oracle Installation Prerequisites: Database Installation Guide for Linux 
(https://docs.oracle.com/en/database/oracle/oracle-database/19/ladbi/index.html)

Setup: 
OS: OEL 7.5 
Ansible: ansible 2.7.6

Oracle Software: Download the Oracle software from OTN or MOS depending on your support status. Oracle binaries are staged from the "edelivery: Oracle Database 19c Software (64-bit)". They have to be manually downloaded and made available for this article to apply 

- Install Oracle Database Software
Oracle DBA - Automation with Ansible (Install Oracle 19c Database Software)

Steps: 1  :Stage Oracle Client software (Oracle Database 19c Client (19.3) for Linux x86-64 - LINUX.X64_193000_client.zip ) from edelivery.oracle.com.
       2  :Unpack Oracle Client Software
       3  :Install Oracle Client Software
       4  :Validation - Connect to SQLPLUS binary.     

Summary commands: 

1. Clone this repository:
   git clone https://github.com/asiandevs/OracleDBAwithAnsible

2. Stage the following Oracle Software on the control machine
   https://www.oracle.com/technetwork/database/enterprise-edition/downloads/oracle19c-linux-5462157.html
   Oracle Database 19c Client (19.3) for Linux x86-64 - LINUX.X64_193000_client.zip

3. Configure an Ansible inventory file (example as below) 
[root@oel75 ansible]# cat ansible.cfg | grep inventory
inventory = ./inventory
[root@oel75 ansible]# cat inventory
[ora-x1]
192.168.56.102

4. Run the playbook role "oracleclient19c_install"
ansible-playbook oracleclient_install.yml  

[root@oel75 ansible]# cat oracleclient_install.yml
- hosts: ora-x1
  user: root

  roles:
   - oracleclient19c_install
