# Try Personium on your machine  

These procedures set up a Personium Unit on 1VM using vagrant + ansible.  

You can easily set up on your machine and try to explore the Personium APIs.

#### Confirmed environment  
The setup procedures described here are tested in the following environment.

| Environment| Windows        | VirtualBox  | Vagrant | Installation time |
|:-----------|:---------------|:------------|:--------|:---------------|
| 1          | 8.1 x64 RAM 8GB | 5.2.8 | 2.1.0 | 7 hrs |
| 2          | 10 x64 RAM 16GB | 5.2.14 | 2.1.2 | 2 hrs |  

#### Setup procedures  

Ok, let's start to set up Personium!

1. Download and install VirtualBox. ([Download page](https://www.virtualbox.org/wiki/Downloads))  

1. Download and install Vagrant. ([Download page](https://www.vagrantup.com/downloads.html))  

1. Clone this repository. (https://github.com/personium/setup-vagrant.git)  
\* Please clone or download the zip file from the release branch.  
\* Since the master branch may contain new features which are under testing and development, errorneous behavior may be expected.  
\* If Japanese is included in the path of the git clone directory, vagrant up commnd will fail. **Please do not include Japanese in the path.**

    ```bash
    $ git clone https://github.com/personium/setup-vagrant.git
    ```

1. Change to setup-vagrant directory under the local repository you cloned, and run vagrant up.  
\* Depending on your network bandwidth and CPU power, this procedure may take at least 2 hours to complete.  
\* Sometimes tomcat will failed to start because it takes more than 60 seconds. But tomcat is usually running, so please ignore the failure and go to the next step.  
\* If your network is behind a proxy server, please configure the proxy settings for vagrant and Ansible before running `vagrant up`.  
[How to Setting in proxy environment](How_to_Setting_in_proxy_environment.md "")  
**Make sure that port 443/tcp is not used on the PC and execute it.**  

    ```bash
    $ cd ./setup-vagrant
    $ vagrant up
    ```

#### Confirm Personium

1. Log in to the virtual server and check the information of personium administration account.

   ```console
   $ vagrant ssh
   $ sudo su -
   # cat /root/ansible/unitadmin_account
   unitadmin {password}
   # exit
   $ exit
   ```

1. Verify that your Personium Unit-Manager is up and running.
    1. Access the following URL from the browser.   
　    　https://localhost/app-uc-unit-manager/__/html/login.html  
        \* Please refer to the link for [Unit-Manager](https://github.com/personium/app-uc-unit-manager "").  

    1. Enter the following on the login page and click the "sign in" button.  
       * Login URL      : https://localhost/unitadmin/  
       * Username       : unitadmin  
       * Password       : {password}  
       \* For {password}, enter the password confirmed in the above "1."

1. Verify that your Personium is up and running.  
    1. Execute the following command  

        ```bash
        $ curl -X POST "https://localhost/__ctl/Cell" -d "{\"Name\":\"sample\"}" -H "Authorization:Bearer example_master_token" -H "Accept:application/json" -i -sS -k
        ```

        \* To execute the API on the virtual server, execute the command as follows.　　

        ```bash
        $ curl -X POST "https://localhost/__ctl/Cell" -d "{\"Name\":\"sample\"}" -H "Authorization:Bearer example_master_token" -H "Accept:application/json" -i -sS -k
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
        Location: http://localhost/__ctl/Cell('sample')
        X-Dc-Version: 1.3.20
        Server: PCS

        {"d":{"results":{"__metadata":{"uri":"http:\/\/localhost\/__ctl\/Cell('sample')","etag":"W\/\"1-1422275532964\"","type":"UnitCtl.Cell"},"Name":"sample","__published":"\/Date(1422275532964)\/","__updated":"\/Date(1422275532964)\/"}}}
        ```


#### Information about the Personium Unit  
If you follow the above procedures, your Personium Unit is constructed with the following specifications.

##### Personium Unit  

* parameters  

    |Parameter    |                    |
    |:------------|--------------------|
    |VM Memory    |2048                |
    |FQDN         |localhost           |
    |PORT         |443                 |
    |UnitUserToken|example_master_token|

    The default setting for the environment created by vagrant is a unit certificate created by specifying localhost as Common Name.
    Please re-create the unit certificate as necessary, for example when accessing with a name other than localhost.
    How to create a unit certificate is [here](https://github.com/personium/ansible/blob/master/How_to_generate_Self-signed_Unit_Certificate.md).

    By re-deploying the re-created unit.csr and unit-self-sign.crt to /opt/x509/ and restarting Tomcat, you can access it with the specified name.

* Personium modules  

    |Module name                    |
    |:------------------------------|
    |[personium-core](https://github.com/personium/personium-core)                 |
    |[personium-engine](https://github.com/personium/personium-engine)               |

* Personium plugin modules  

    |Module name                    |
    |:------------------------------|
    |[personium-plugin-sample](https://github.com/personium/personium-plugin-sample)        |

* Personium ex modules  

    |Module name                    |
    |:------------------------------|
    |[personium-ex-mailsender](https://github.com/personium/personium-ex-mailsender)        |
    |[personium-ex-slack-messenger](https://github.com/personium/personium-ex-slack-messenger)   |
    |[personium-ex-ew-services](https://github.com/personium/personium-ex-ew-services)       |
    |[personium-ex-httpclient](https://github.com/personium/personium-ex-httpclient)        |

* Personium app modules

    |Module name                    |
    |:------------------------------|
    |[app-uc-unit-manager](https://github.com/personium/app-uc-unit-manager)            |

##### OS and Middleware on VM

* OS  
CentOS 7.2 x86_64

* Middleware  

    |Category       | Name           |Version       |                   |
    |:--------------|:---------------|-------------:|:------------------|
    | java          | OpenJDK        |        8u192 | --                |
    | tomcat        | tomcat         |       9.0.10 | web               |
    |               | commons-daemon |        1.1.0 | --                |
    | nginx         | nginx          |       1.14.0 | proxy             |
    |               | Headers More   |         0.32 | --                |
    | logback       | logback        |        1.0.3 | --                |
    |               | slf4j          |        1.6.4 | --                |
    | memcached     | memcached      |       1.4.21 | cache             |
    | elasticsearch | elasticsearch  |        2.4.1 | db & search engine|
