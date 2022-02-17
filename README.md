This is my first experience dealing with the MongoDB database. I am currently learning MERN stack, as I was creating my MongoDB Atlas it’s so far so good. But when I am about to connect the MongoDB Compass with the database via the connection string, I was stuck in the installation part of MongoDB v5.0 compass because the MongoDB Compass only outputs this:

```bash
MongoDB compass `Server selection timed out after 30000 ms`
```

## What I’ve done

- Done permit my system IP address access to the database or rather a cluster
- Testing a port using the browser
    - telnet
        
        ```bash
        telnet portquiz.net 27017
        ```
        
        output:
        
        ```bash
        telnet: Unable to connect to remote host: Connection timed out```
        ```
        
    - nc
        
        ```bash
        nc -v portquiz.net 27017
        ```
        
        output:
        
        ```bash
        nc: connect to portquiz.net port 27017 (tcp) failed: Connection timed out
        ```
        
    - curl
        
        ```bash
        curl portquiz.net:27017
        ```
        
        output:
        
        ```bash
        curl: (28) Failed to connect to portquiz.net port 27017: Connection timed out
        ```
        
    - wget
        
        ```bash
        wget -qO- portquiz.net:27017
        ```
        
        output: none (no output)
        
- Installed ver MongoDB 5.0 first, and then 4.4 :
    - start with removing all PPA repositories for MongoDB, then:
    
    ```bash
    sudo service mongod stop
    sudo apt-get purge mongodb-org*
    sudo apt remove mongodb
    sudo apt purge mongodb
    sudo apt autoremove
    sudo rm -r /var/log/mongodb && sudo rm -r /var/lib/mongodb
    sudo apt-get install gnupg
    wget -qO - https://www.mongodb.org/static/pgp/server-4.4.asc | sudo apt-key add
    echo "deb [ arch=amd64,arm64 ] https://repo.mongodb.org/apt/ubuntu focal
    sudo apt-get update && sudo apt-get upgrade
    sudo apt update && sudo apt upgrade
    sudo apt-get install -y mongodb-org
    sudo apt-get install libc6
    sudo service mongod start
    sudo service mongod status
    ```
    
    All above done, still, give the same issue.
    

### current conclusion: I think port `27017` is blocked by my ISP

**Finally,**

I decided to abandon this, and delete all files of `mongo`

- removing all PPA repositories for MongoDB from `Software & Updates` app
- `sudo service mongod stop`
- purge all packages that start with “mongodb-org” and “mongo”
    
    ```bash
    sudo apt-get purge mongodb-org*
    sudo apt purge mongo*
    ```
    
- manually remove mongodb directory
    
    ```bash
    sudo rm -r /var/log/mongodb
    sudo rm -r /var/lib/mongodb
    ```
    
- make sure that no `mongo` packages are left
    
    ```bash
    dpkg -l | grep mongo
    ```
    
- find files that contain `mongo` in their name and `delete` it
    
    `delete` will perform better because it doesn't have to spawn an external process for each and every matched file, but make sure to use it after `-name`. Otherwise, it will delete the specified entire file tree.
    
    ```bash
    sudo -H /bin/bash
    cd /etc/apt
    sudo find /etc/ -name "*mongo*" -delete
    ```
    
- remove user and group mongodb
    
    ```bash
    sudo userdel -r mongodb
    sudo groupdel mongodb
    ```
    
- check whether mongodb user/group exists or not
    
    ```bash
    cut -d: -f1 /etc/passwd | grep mongo
    cut -d: -f1 /etc/group | grep mongo
    ```