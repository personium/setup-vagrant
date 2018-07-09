# Try Personium on your machine  

These procedures set up a Personium Unit on 1VM using vagrant + ansible.  

You can easily set up on your machine and try to explore the Personium APIs.

#### Confirmed environment  
The setup procedures described here are tested in the following environment.

* Version  
    * Windows 8.1 x64  
    * VirtualBox 5.2.8  
    * Vagrant 2.1.0  
* Host machine spec  
    * RAM 8 GB  

#### Setup procedures  

Ok, let's start to set up Personium!

1. Download and install VirtualBox. ([Download page](https://www.virtualbox.org/wiki/Downloads))  

1. Download and install Vagrant. ([Download page](https://www.vagrantup.com/downloads.html))  

1. Clone this repository. (https://github.com/personium/setup-vagrant.git)  
\* If Japanese is included in the path of the git clone directory, vagrant up commnd will fail. **Please do not include Japanese in the path.**

    ```bash
    $ git clone https://github.com/personium/setup-vagrant.git
    ```

1. Change to setup-vagrant directory under the local repository you cloned, and run vagrant up.  
\* Sometimes tomcat will failed to start because it takes more than 60 seconds. But tomcat is usually running, so please ignore the failure and go to the next step.  
\* If your network is behind a proxy server, please configure the proxy settings for vagrant and Ansible before running `vagrant up`.  
[How to Setting in proxy environment](How_to_Setting_in_proxy_environment.md "")  

    ```bash
    $ cd ./setup-vagrant
    $ vagrant up
    ```

1. Verify that your Personium is up and running.  
    1. Execute the following command  

        ```bash
        $ curl -X POST "https://localhost:1210/__ctl/Cell" -d "{\"Name\":\"sample\"}" -H "Authorization:Bearer example_master_token" -H "Accept:application/json" -i -s
        ```

    1. If Personium works fine, 201 response is returned as below. a cell is successfully created!  

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
    

#### Information about the Personium Unit  
If you follow the above procedures, your Personium Unit is constructed with the following specifications.

##### Personium Unit  

* parameters  

    |Parameter    |                    |
    |:------------|--------------------|
    |VM Memory    |2048           |
    |FQDN         |localhost           |
    |PORT         |1210                |
    |UnitUserToken|example_master_token|

* Personium modules  

    |Module name      |
    |:----------------|
    |personium-core   |
    |personium-engine |


##### OS and Middleware on VM

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
    | elasticsearch | elasticsearch  |        2.4.1 | db & search engine|
