# Ansible to use with Vagrant
---------------------------------------

## Overview
This document explains the configuration of Ansible used for automatic construction of Vagrant.
Ansible being used is based on [ansible/1-server](https://github.com/personium/ansible/tree/master/1-server_unit). See [ansible/1-server](https://github.com/personium/ansible/tree/master/1-server_unit) for a detailed explanation.

## Server setup :white_check_mark:

#### server configuration sample
  Below are the server structure that we configured.

|**Servers**      |   **Role**      |   **MW**                           |  **default memory size** (*1) |  
|:----------------|:----------------|:-----------------------------------|:------------------------------|
| Server 1        |  Bastion,Web,AP,ES    | nginx,tomcat(768MB),memcached(256MB*2),Elasticsearch(1875MB)                     |2048MB                         |

(*1) : Required default memory size. Memory size of each MW configuration file could be modified


<img src="1-server_unit.jpg" title="1-server_unit" style="width:70%;height:auto;">

#### File structure

| File name | Contents |
|---|---|
| `/init_personium.yml`  |		yml file which should be executed by Ansible-playbook|
| `/[group name].yml`	   |		yml file to retrieve the variable of each group and execute its related tasks|
| `/Ansible.cfg`         |		Describes the required Settings for Ansible execution. (\*Modification is not required)|
| `/static_inventory/`   |		This folder contains all the essential information of different environments|
| `/static_inventory/hosts\*#`	          |        Describes information of hosts (IP address, FQDN, group, User name, Private Key, etc.)|
| `/group_vars/`	       |		Folder to organize files in order to perform various customization or tuning on servers|
| `/group_vars/[group name].yml\*#`  |		   Collections of values for each group, which requires to customize/tune the settings|
| `/resource/`		   |	    Folder to organize files which are necessary in task (\*Modification is not required)|
|  `/resource/[groue name]/`	   |		Store the resources of each group|
|  `/tasks/`			   |    	Folder to organize task|
|  `/tasks/[groue name]/`	   |		Store specific task for each group|
|  `/handlers/`		   |	    Folder to organize handlers|
|  `/handlers/[group name]/`	   |		Store handler for each group|


  \*# : Files required to modify according to the environment.

  \*[group name] : web, ap, nfs, es, bastion and common. All in all 6 groups.
  (Here `common` is not the server role. Common group is used to set some general functionalities on all the servers.)

#### File (key) handling Caution: :zap:

The following key file will be generated automatically during the Ansible execution. Please handle these keys carefully.

`/personium/Personium-core/conf/salt.key`

`/personium/Personium-core/conf/token.key`

