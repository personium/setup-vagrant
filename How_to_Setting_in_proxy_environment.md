# How to Setting in proxy environment

## Introduction

This manual explains the settings for running Vagrant in a proxy environment.

---------------------------------------

### Part 1. Proxy setup for vagrant file.

1. Install _vagrant-proxyconf_ plugin. (http://weblabo.oscasierra.net/vagrant-proxyconf/)

	```bash
	$ set http_proxy=http://{proxy_ip}:{proxy_port}
	$ set https_proxy=http://{proxy_ip}:{proxy_port}
	$ vagrant plugin install vagrant-proxyconf
	```  

	\* If you are using an authentication proxy, specify the user name and password in the Proxy setting as shown below.  

	```bash
	$ set http_proxy=http://{username}:{password}@{proxy_ip}:{proxy_port}
	$ set https_proxy=http://{username}:{password}@{proxy_ip}:{proxy_port}
	$ vagrant plugin install vagrant-proxyconf
	```


1. Enable your proxy setting in your Vagrantfile under the cloned setup- vagrant directory. Below is an example how to setup the proxy.

	```bash:Vagrantfile
	 if Vagrant.has_plugin?("vagrant-proxyconf")
	   config.proxy.http = "http://{proxy_ip}:{proxy_port}/"
	   config.proxy.https = "http://{proxy_ip}:{proxy_port}/"
	   config.proxy.no_proxy = "localhost,127.0.0.1"
	 end
	```

	\* If you are using an authentication proxy, specify the user name and password in the Proxy setting as shown below.

	```bash:Vagrantfile
	 if Vagrant.has_plugin?("vagrant-proxyconf")
	   config.proxy.http = "http://{username}:{password}@{proxy_ip}:{proxy_port}/"
	   config.proxy.https = "http://{username}:{password}@{proxy_ip}:{proxy_port}/"
	   config.proxy.no_proxy = "localhost,127.0.0.1"
	 end
	```

### Part 2. Proxy setup for apache maven.

1. Enable your proxy setting in your Ansible tasks.
   The task to activate is "deploy maven's setting.xml".
   Task file is ./ansible/tasks/bastion/init_maven.yml.

   ```
   - name: deploy maven's setting.xml
     template: src=./resource/bastion/tmp/settings.xml.j2 dest=/root/.m2/settings.xml owner=root group=root mode=0644
   ```
   \* If you are using an authentication proxy, modify ./ansible/resource/bastion/tmp/settings.xml.j2. 
   Uncomment the following comment.

   ```
	 <!--
      <username>{{maven_proxy_username.stdout}}</username>
      <password>{{maven_proxy_password.stdout}}</password>
   -->
	 ```