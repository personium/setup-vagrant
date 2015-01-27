Try personium.io on your machine
====

This procedure sets up personium.io on 1VM using vagrant + ansible.

You can easily set up on your machine and try to explore personium.io APIs.

\* This procedure is tested with VirtualBox 4.3.20 and Vagrant 1.7.2.

#### Set up

Ok, let's start to set up personium.io!

1. Download and install VirtualBox. ([Download page](https://www.virtualbox.org/wiki/Downloads))

2. Download and install Vagrant. ([Download page](https://www.vagrantup.com/downloads))

3. Clone this repository. (https://github.com/personium/io-vagrant-ansible.git)

	```bash
	$ git clone https://github.com/personium/io-vagrant-ansible.git
	```

4. Change to io-vagrant-ansible directory under the local repository you cloned, and run vagrant up. \* This process takes around 30 min. 

	```bash
	$ cd ./io-vagrant-ansible
	$ vagrant up
	```

5. Verify your personium.io is up and running.

	```bash
	$ curl -X POST "http://localhost:1210/__ctl/Cell" -d "{\"Name\":\"sample\"}" -H "Authorization:Bearer personiumio" -H "Accept:application/json" -i -s
	```

	If personium.io works fine , 201 response is returned as below. Cell is successfully created!

	```bash
	HTTP/1.1 201 Created
	Date: Mon, 26 Jan 2015 12:32:13 GMT
	Content-Type: application/json
	Transfer-Encoding: chunked
	Connection: keep-alive
	Access-Control-Allow-Origin: *
	DataServiceVersion: 2.0
	ETag: W/"1-1422275532964"
	Location: http://localhost:1210/__ctl/Cell('sample')
	X-Dc-Version: 1.3.20
	Server: PCS

	{"d":{"results":{"__metadata":{"uri":"http:\/\/localhost:1210\/__ctl\/Cell('sample')","etag":"W\/\"1-1422275532964\"","type":"UnitCtl.Cell"},"Name":"sample","__published":"\/Date(1422275532964)\/","__updated":"\/Date(1422275532964)\/"}}}
	```

\* If your network is under a proxy server...

Before you run `vagrant up` ,

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

3. After completing proxy setting, run ```vargrant up```.

#### Information of personium.io that you set up

If you set up personium.io in above procedure , personium.io is constructed as below.

##### About local personium.io

* parameters

	|parameter    |           |
	|:------------|-----------|
	|FQDN         |localhost  |
	|PORT         |1210       |
	|UnitUserToken|personiumio|

* personium.io modules

	Following personium.io modules are deployed on app server.

	|module     |
	|:----------|
	|dc1-core   |
	|dc1-engine |


##### About OS and Middleware on VM

* OS

	CentOS 6.5 x86_64

* Middleware

    |Category       | Name           |Version       |                   |
    |:--------------|:---------------|-------------:|:------------------|
    | java          | JDK            |         8u25 | --                |
    | tomcat        | tomcat         |       8.0.14 | web               |
    |               | commons-daemon |       1.0.15 | --                |
    | nginx         | nginx          |        1.7.6 | proxy             |
    |               | Headers More   |         0.25 | --                |
    | logback       | logback        |        1.0.3 | --                |
    |               | slf4j          |        1.6.4 | --                |
    | memcached     | memcached      |       1.4.21 | cache             |
    | elasticsearch | elasticsearch  |        1.3.4 | db&sarch engine   |


#### Required host machine's RAM

1GB of host machine's RAM is required to run personium.io.
