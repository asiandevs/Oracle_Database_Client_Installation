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
++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
           WORK LOG
         +++++++++++++
 [root@oel75 ansible]# ansible-playbook oracleclient_install.yml

PLAY [ora-x1] **********************************************************************************************************************

TASK [Gathering Facts] *************************************************************************************************************
ok: [192.168.56.102]

TASK [oracleclient19c_install : display pre oracle client software install message] ************************************************
ok: [192.168.56.102] => {
    "msg": [
        "Oracle Client19c Software Installation in progress at 2019-07-28T08:58:51Z:"
    ]
}

TASK [oracleclient19c_install : create required directories] ***********************************************************************
ok: [192.168.56.102] => (item=/u01)
ok: [192.168.56.102] => (item=/u01/app/oraInventory)
ok: [192.168.56.102] => (item=/u01/app/oracle)
changed: [192.168.56.102] => (item=/u01/stage)
changed: [192.168.56.102] => (item=/u01/app/oracle/product/19.3.0)

TASK [oracleclient19c_install : Copy required script to the target server] *********************************************************
changed: [192.168.56.102] => (item=tns_upd.sh)

TASK [oracleclient19c_install : Unpack Oracle 19c Client Software to the target server] ********************************************
changed: [192.168.56.102]

TASK [oracleclient19c_install : Setup Oracle Client 19c Software silent response file] *********************************************
changed: [192.168.56.102]

TASK [oracleclient19c_install : Install Oracle Client 19c Software] ****************************************************************
changed: [192.168.56.102]

TASK [oracleclient19c_install : Validate Oracle Client Software Installation] ******************************************************
changed: [192.168.56.102]

TASK [oracleclient19c_install : debug] *********************************************************************************************
ok: [192.168.56.102] => {
    "msg": [
        "",
        "SQL*Plus: Release 19.0.0.0.0 - Production",
        "Version 19.3.0.0.0"
    ]
}

TASK [oracleclient19c_install : set profile entry for the client software] *********************************************************
ok: [192.168.56.102] => (item={u'start': u'ORACLE_HOME=', u'end': u'/u01/app/oracle/product/19.3.0'})
ok: [192.168.56.102] => (item={u'start': u'PATH=', u'end': u'/u01/app/oracle/product/19.3.0/bin:$PATH:/bin:/usr/bin::/usr/ccs/bin'})

TASK [oracleclient19c_install : Create tnsnames.ora and sqlnet.ora path if not present] ********************************************
changed: [192.168.56.102] => (item={u'location': u'/u01/app/oracle/product/19.3.0/network/admin/tnsnames.ora', u'mode': u'0775'})
changed: [192.168.56.102] => (item={u'location': u'/u01/app/oracle/product/19.3.0/network/admin/sqlnet.ora', u'mode': u'0775'})

TASK [oracleclient19c_install : Add tns entry for the source databases] ************************************************************
changed: [192.168.56.102]

TASK [oracleclient19c_install : Add entry to sqlnet.ora file] **********************************************************************
changed: [192.168.56.102]

TASK [oracleclient19c_install : Remove stage directory] ****************************************************************************
changed: [192.168.56.102]

TASK [oracleclient19c_install : display post install message] **********************************************************************
ok: [192.168.56.102] => {
    "msg": [
        "This playbook completed below task for Single Instance at 2019-07-28T08:58:51Z:",
        "- Install oracle client to the listed servers of ora-x1 groups",
        "- Modify tnsnames.ora and sqlnet.ora based on the business requirements",
        "- Validate connection using EZconnect (sqlplus username/password@server/SID)",
        "- END OF ALL: git clone of oracleclient19c_install repository will be shared"
    ]
}

PLAY RECAP *************************************************************************************************************************
192.168.56.102             : ok=15   changed=10   unreachable=0    failed=0

[root@oel75 ansible]# ansible-playbook oracleclient_install.yml^C
[root@oel75 ansible]# cat oracleclient_install.yml
- hosts: ora-x1
  user: root

  roles:
   - oracleclient19c_install

===============================================
Note: Modify variables according to your requirements. Add TNS entry according to your databases. 
