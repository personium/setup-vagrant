## Introduction

This manual explains the settings for running Vagrant in a proxy environment.

---------------------------------------

### Part 1. Proxy setup for vagrant file.

1. Install _vagrant-proxyconf_ plugin. (http://weblabo.oscasierra.net/vagrant-proxyconf/)

	```bash
	$ vagrant plugin install vagrant-proxyconf
	```

2. Enable your proxy setting in your Vagrantfile. Below is an example how to setup the proxy.

	```bash:Vagrantfile
	 if Vagrant.has_plugin?("vagrant-proxyconf")
	   config.proxy.http = "http://username:password@host:port/"
	   config.proxy.https = "http://username:password@host:port/"
	   config.proxy.no_proxy = "localhost,127.0.0.1"
	 end
	```


### Part 2. Proxy setup for apache maven.

1. Enable your proxy setting in your Ansible tasks.
   The task to activate is "deploy maven's setting.xml".
   Task file is ./ansible/tasks/bastion/init_maven.yml.

   ```
   - name: deploy maven's setting.xml
     template: src=./resource/common/settings.xml.j2 dest=/root/.m2/settings.xml owner=root group=root mode=0644
   ```
   \* If you are using an authentication proxy, modify ./ansible/resource/bastion/settings.xml.j2. 
   Uncomment the following comment.

   ```
	 <!--
      <username>{{maven_proxy_username.stdout}}</username>
      <password>{{maven_proxy_password.stdout}}</password>
   -->
	 ```