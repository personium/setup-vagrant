Try Personium on your machine
====

This procedure sets up Personium on 1VM using vagrant + ansible.

You can easily set up on your machine and try to explore Personium APIs.

#### Operational confirmation environment
\* This procedure is tested in the following environment

* Version
  * Windows 8.1 x64
  * VirtualBox 5.2.8
  * Vagrant 2.1.0

* Host machine spec
  * RAM 8 GB

#### Set up

Ok, let's start to set up Personium!

1. Download and install VirtualBox. ([Download page](https://www.virtualbox.org/wiki/Downloads))

2. Download and install Vagrant. ([Download page](https://www.vagrantup.com/downloads))

3. Clone this repository. (https://github.com/personium/setup-vagrant.git)

    \* In case of Windows machine, please set core.autocrlf is false in git client's config so there are shell scripts.  
	\* If Japanese is included in the path of the git clone directory, vagrant up commnd fails. Please do not include Japanese in the pass.

	```bash
	$ git clone https://github.com/personium/setup-vagrant.git
	```

4. Change to setup-vagrant directory under the local repository you cloned, and run vagrant up.

   \* It sometimes happens that tomcat's start is failed because it takes more than 60 seconds. But tomcat is usually running, so please go to next step ignoring that.

	```bash
	$ cd ./setup-vagrant
	$ vagrant up
	```

	\* If your network is under a proxy server, please do proxy settings for vagrant and Ansible before running `vagrant up`.
	[How to Setting in proxy environment](How_to_Setting_in_proxy_environment.md ""),

5. Verify your Personium is up and running.

	```bash
	$ curl -X POST "https://localhost:1210/__ctl/Cell" -d "{\"Name\":\"sample\"}" -H "Authorization:Bearer example_master_token" -H "Accept:application/json" -i -s
	```

	If Personium works fine , 201 response is returned as below. Cell is successfully created!

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
	

#### Information of Personium that you set up

If you set up Personium in above procedure , Personium is constructed as below.

##### About local Personium

* parameters

	|parameter    |                    |
	|:------------|--------------------|
	|VM Memory       |2048           |
	|FQDN         |localhost           |
	|PORT         |1210                |
	|UnitUserToken|example_master_token|

* Personium modules

	Following Personium modules are deployed on app server.

	|module           |
	|:----------------|
	|personium-core   |
	|personium-engine |


##### About OS and Middleware on VM

* OS

	CentOS 7.2 x86_64

* Middleware

    |Category       | Name           |Version       |                   |
    |:--------------|:---------------|-------------:|:------------------|
    | java          | JDK            |        8u131 | --                |
    | tomcat        | tomcat         |       8.0.44 | web               |
    |               | commons-daemon |       1.0.15 | --                |
    | nginx         | nginx          |       1.13.3 | proxy             |
    |               | Headers More   |         0.32 | --                |
    | logback       | logback        |        1.0.3 | --                |
    |               | slf4j          |        1.6.4 | --                |
    | memcached     | memcached      |       1.4.21 | cache             |
    | elasticsearch | elasticsearch  |        2.4.1 | db&sarch engine   |
