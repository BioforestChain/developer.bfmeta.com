# Node deployment

## CentOS 7.9
### Prepare

- Disable SELinux

   ```
   sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
    
   ```

- Install the software package
  
   ```
   yum install -y zip unzip ntp epel-release

   ```
  
- Synchronize network time

   ```
   ntpdate -u cn.pool.ntp.org

   #Add a scheduled task to automatically synchronize the time at 3 o'clock in the morning every day
   echo "0 3 * * * /usr/sbin/ntpdate -u cn.pool.ntp.org ;/usr/sbin/hwclock -w" >>/var/spool/cron/root
   ```
  
- Install related dependencies

  ```
  rpm -ivh https://mirrors.aliyun.com/centos-vault/8.0.1905/BaseOS/x86_64/os/Packages/crypto-policies-20181217-6.git9a35207.el8.noarch.rpm
  rpm -ivh --replacefiles https://mirrors.aliyun.com/centos-vault/8.1.1911/BaseOS/x86_64/os/Packages/openssl-libs-1.1.1c-2.el8_1.1.x86_64.rpm
  rpm -ivh --replacefiles https://mirrors.aliyun.com/centos-vault/8.1.1911/BaseOS/x86_64/os/Packages/openssl-1.1.1c-2.el8_1.1.x86_64.rpm
  rpm -ivh https://mirrors.aliyun.com/centos-vault/7.0.1406/os/x86_64/Packages/libevent-2.0.21-4.el7.x86_64.rpm
  yum install -y supervisor
  
  ```
- Adjust the maximum number of files that users can use:

  ```
  echo -ne "
    * soft nofile 65536
    * hard nofile 65536
  " >> /etc/security/limits.conf
  ```

  

- Kernel parameter optimization

  ```
  echo -ne "
    net.ipv4.tcp_max_tw_buckets=10000
    net.ipv4.tcp_sack=1
    net.ipv4.tcp_window_scaling=1
    net.ipv4.tcp_rmem=4096 87380 4194304
    net.ipv4.tcp_wmem=4096 16384 4194304
    net.ipv4.tcp_max_syn_backlog=65536
    net.core.netdev_max_backlog=65536
    net.core.somaxconn=32768
    net.core.wmem_default=8388608
    net.core.rmem_default=8388608
    net.core.wmem_max=16777216
    net.core.rmem_max=16777216
    net.ipv4.tcp_synack_retries=2
    net.ipv4.tcp_syn_retries=2
    net.ipv4.tcp_tw_recycle=1
    net.ipv4.tcp_tw_reuse=1
    net.ipv4.tcp_keepalive_time=1200
    net.ipv4.tcp_mem=94500000 915000000 927000000
    net.ipv4.tcp_max_orphans=3276800
    net.ipv4.ip_local_port_range=9000 65000
    vm.swappiness=0
    net.ipv4.tcp_fin_timeout=15
    fs.file-max=1048576
  " >> /etc/sysctl.conf
  
  ```

 

- Use jemalloc to optimize memory usage
   In order to optimize the memory usage, you can use jemalloc instead of the system glibc malloc, the method is as follows:
   centos7 (you need to restart the server after configuration):

  ```
  rpm -ivh https://cbs.centos.org/kojifiles/packages/jemalloc/3.6.0/8.el7.centos/x86_64/jemalloc-3.6.0-8.el7.centos.x86_64.rpm
  rpm -ivh https://cbs.centos.org/kojifiles/packages/jemalloc/3.6.0/8.el7.centos/x86_64/jemalloc-devel-3.6.0-8.el7.centos.x86_64.rpm
  echo "/usr/lib64/libjemalloc.so" >> /etc/ld.so.preload
  
  ```

  

 

### Public chain node deployment

#### Node program installation and configuration
- Minimum configuration to run BCF program：

   CPU: 2+ cores (X86，main frequency 3.0G+)

   Memory: 8G

   Hard Disk: SSD 500G or above

   System: CentOS 7.*

- we recommend that you use the following configuration equipment:

   CPU: 8 cores (main frequency 3.0G+)+

   Memory: 16G

   Hard Disk: SSD 2T or above

   System: CentOS 7.*


- The installation and configuration steps of the node program and the upgrade service program are as follows:

1. Create a directory for bfmchain

    ```
    mkdir -p /data
    ```

   

2. Upload the zip files (the compressed file contains the BFMeta program and the upgrade service program) and unzip it to the /data directory

   ```
   unzip -o -q /data/release/bfmchain.zip -d /data/
   
   ```

   

3. Grant executable permissions to the file

   ```
   chmod u+x /data/bfmchain/bcf
   chmod u+x /data/bfmchain/mongoComponents/linux/mongo*
   <!-- chmod u+x /data/bfmchain/turnserver/bin/turnserver -->
   chmod u+x /data/upgrade/mongoComponents/linux/mongo*
   chmod u+x /data/upgrade/upgrade
   echo "export BCF_HOME=/data/bfmchain"  >> /etc/profile
   #effective environment change
   source  /etc/profile 
   ```

   

4. Configure supervisor (manage BFMeta running in the background and upgrade service process)

   ```
   echo -ne "
   	[program:bcf]	
   	command=/data/bfmchain/bcf  ; 
   	directory=/data/bfmchain
   	autostart=true    ; 
   	user=root      ;
   	autorestart=unexpected   ;
   	exitcode=0,2   ;
   	startretries=3    ;
   	priority=999     ;
   	redirect_stderr=true ;
   	stdout_logfile_maxbytes=50MB  ;
   	stdout_logfile_backups = 20  ;
   	stdout_logfile=/data/bfmchain/logs/bcf.log    ;
   	stopasgroup=true   ;
   " >> /etc/supervisord.d/bcf.ini
   
   echo -ne "
   	[program:upgrade]
   	command=/data/upgrade/upgrade  ; 
   	directory=/data/upgrade
   	autostart=true       ; 
   	user=root           ;
   	autorestart=unexpected    ;
   	exitcode=0,2    ;
   	startretries=3       ;
   	priority=999         ;
   	redirect_stderr=true ;
   	stdout_logfile_maxbytes=50MB  ;
   	stdout_logfile_backups = 20   ;
   	stdout_logfile=/data/upgrade/logs/console.log      ;
   	stopasgroup=true     ;
   " >> /etc/supervisord.d/upgrade.ini
   ```

#### Firewall configuration

1. System firewall policy
   
   - firewalld
   
   ```
   Allow all external ip access:
    firewall-cmd --add-port=22000/tcp --add-port=22001/tcp --add-port=22002/tcp --add-port=22003/tcp --add-port=22005/tcp --add-port=22007/tcp --add-port=22009/tcp --permanent
   
    Allow external ip (such as 1.1.1.1/32) to access:
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="1.1.1.1/32" port port="22000" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="1.1.1.1/32" port port="22001" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="1.1.1.1/32" port port="22002" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="1.1.1.1/32" port port="22003" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="1.1.1.1/32" port port="22005" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="1.1.1.1/32" port port="22007" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="1.1.1.1/32" port port="22009" protocol="tcp" accept"
   
   ```
   
   iptable
   
   ```
   Allow all external ip access:
    iptables -A INPUT -p tcp -m multiport --dport 22000:22003,22005,22007,22009 -m state --state NEW,ESTABLISHED -j ACCEPT
   
   Allow external ip (such as 1.1.1.1/32) to access:
   iptables -A INPUT -p tcp -m multiport --dport 22000:22003,22005,22007,22009 -m state --state NEW,ESTABLISHED -s 1.1.1.1/32  -j ACCEPT
   
   ```
2. Cloud platform fire protection strategy
   The node program is deployed on the cloud server, and the firewall inbound rules need to be opened according to the cloud service provider's operating document.

   - Google Cloud:

     ```
     1. Go to the "Firewall" (Network---VPC Network--Firewall) page in the Google Cloud Console.
     2. Click Create firewall rule.
     3. Enter the name of the firewall rule. The name must be unique in the project.
     4. Specify the network to implement firewall rules.
     5. Specify the priority of the rule.
     6. Select inbound as the flow direction.
     7. For the operation performed on the matching item, select Allow.
     8. For the target, perform one of the following operations:
     If you want to allow traffic from the BFMeta node instance to all clients in the network, please select all instances in the network.
     If you want to allow traffic from a specific client of the BFMeta node instance, select the specified target tag. Enter the instance name of the client in the target tag.
     9. Keep the default value of the IP address range of the source filter condition.
     10. For the source IP address range, enter the IP address range of the BFMeta node instance to be allowed as the source of access.
     You can enter 0.0.0.0/0 to allow all ip address access traffic to pass, or you can enter a specific IP address to restrict access to only that ip address (the CIDR notation must be used).
     11. Keep the default value of none for the secondary source filter condition.
     12. For the protocol and port, please select the specified protocol and port, and then:
     Check the tcp checkbox and enter 22000,22001,22002,22003,22005,22007,22009 in the associated field.
     13. Choose Create.
     ```

     a) The open ports of all backend cloud servers allow all ip addresses to access:
     ![gcp-fw-allsource-allsrc](./images/gcp-fw-allsource-allsrc.png)

     b) The open ports of all the back-end cloud servers allow access with characteristic ip addresses (such as 1.1.1.1):
     ![gcp-fw-allsource-limitsrc](./images/gcp-fw-allsource-limitsrc.png)

     c) The open port of the cloud server with a specific back-end tag (such as BFMeta node) allows all ip addresses to access:
     ![gcp-fw-limitsource-allsrc](./images/gcp-fw-limitsource-allsrc.png)

     d) The open port of the cloud server with a specific back-end tag (such as BFMeta node) allows access to the characteristic ip address (such as 1.1.1.1):

     ![gcp-fw-limitsource-limitsrc](./images/gcp-fw-limitsource-limitsrc.png)
     

   

   

   - AWS:

     1. Click on the instance details that need to be configured "Security"-"Security Group":
        ![AWS1](./images/AWS1.png)

     2. Then click "Edit Security Group Rules" in the pop-up security group:
        ![AWS2](./images/AWS2.png)

     3. Add inbound rules:

        ![AWS3](./images/AWS3.png)

     

   - Ali Cloud:

     1. Log in to the ECS cloud server console
        If you can't see the ECS instance, you can switch the geographic search to find the target instance that needs to be operated.

     2. In the instance list, click "More"-"Network and Security Group"-"Security Group Configuration"
        ![aliyun-1](./images/aliyun-1.png)

     3. Open the security group configuration interface and click "Configure Rules"
        ![aliyun-2](./images/aliyun-2.png)

     4. Security group access rule configuration (manually add rules)
        The access rules in the security group rules have two aspects, namely the inbound direction and the outbound direction. The inbound direction refers to the direction in which users access the server, and the outbound direction refers to the outbound access of the cloud server, which is generally not restricted.

        ![aliyun-3](./images/aliyun-3.png)

   

   
   
   
#### Start the service

1. Start BFMeta node program-BCF

   ```
   systemctl enable supervisord
   systemctl start supervisord
   
   ```



### Private chain node deployment

#### environment

|       | Configuration                                                  | IP           |
| ----- | ----------------------------------------------------- | ------------ |
| Node1 | CPU: 16 cores Memory: 16G Bandwidth: 1G SSD: 1T System: centos 7.8 | 172.30.56.61 |
| Node2 | CPU: 16 cores Memory: 16G Bandwidth: 1G SSD: 1T System: centos 7.8 | 172.30.56.62 |
| Node3 | CPU: 16 cores Memory: 16G Bandwidth: 1G SSD: 1T System: centos 7.8 | 172.30.56.63 |




#### Node program installation and configuration

> Please make sure that your device has installed the appropriate dependent environment.

1. Create BFMeta directory

   ```
   mkdir -p /data/bfmchain/
   ```

2. Download the release version to obtain the complete BFMeta installation package (bfmchain.zip).
   
3. Unzip bfmchain.zip to the /data/bfmchain directory

   ```
   unzip -o -q /data/release/bfmchain.zip -d /data/bfmchain
   ```

4. Grant executable permissions to the file

   ```
   chmod u+x /data/bfmchain/bcf
   chmod u+x /data/bfmchain/mongoComponents/linux/mongo*
   chmod u+x /data/bfmchain/turnserver/bin/turnserver
   echo "export BCF_HOME=/data/bfmchain" >> /etc/profile
   #Effective environment variables
   source /etc/profile
   ```

5. Configuration file

   - Download genesis block
      Purchase the genesis block from the BFMeta team and put the files in the /data/bfmchain/genesisInfos directory
     
   - Modify the configuration file (/data/bfmchain/conf/base-config.json)
     
      Configure the externally introduced genesis block isGenesisInfoProvideExternally to true
     ​ Configure the official network or test network bnid as b or c
     ​ Configure the imported genesis chain chain name chainName
     ​ Configure the blockchain asset name chainAssetType of the imported genesis block
     ​ Configure the magic chainMagic of the imported genesis block
     
   - Configure node IP
     Modify the peers field in the configuration file /data/bfmchain/conf/bfm-config-mainnet.json and add the ip of the initial connection
     
      ![config-peers](./images/config-peers.png)
     
   
6. Configure supervisor (manage the BFMeta process running in the background)

   ```
   echo -ne "
   [program:bcf]
   command=/data/bfmchain/bcf;
   directory=/data/bfmchain
   autostart=true;
   user=root;
   autorestart=unexpected;
   exitcode=0,2;
   startretries=3;
   priority=999;
   redirect_stderr=true;
   stdout_logfile_maxbytes=50MB;
   stdout_logfile_backups = 20;
   stdout_logfile=/data/bfmchain/logs/bcf.log;
   stopasgroup=true;
   ">> /etc/supervisord.d/bcf.ini
   ```


#### Firewall configuration

1. System firewall strategy (only 3 nodes are open for interconnection)

   FIREWALLD:

   ```
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.30.56.61" port port="22000" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.30.56.61" port port="22001" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.30.56.61" port port="22002" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.30.56.61" port port="22003" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.30.56.61" port port="22005" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.30.56.61" port port="22007" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.30.56.61" port port="22009" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.30.56.62" port port="22000" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.30.56.62" port port="22001" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.30.56.62" port port="22002" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.30.56.62" port port="22003" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.30.56.62" port port="22005" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.30.56.62" port port="22007" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.30.56.62" port port="22009" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.30.56.63" port port="22000" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.30.56.63" port port="22001" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.30.56.63" port port="22002" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.30.56.63" port port="22003" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.30.56.63" port port="22005" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.30.56.63" port port="22007" protocol="tcp" accept"
   firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.30.56.63" port port="22009" protocol="tcp" accept"
   
   ```

   iptables:

   ```
   iptables -A INPUT -p tcp -m multiport --dport 22000:22003,22005,22007,22009 -m state --state NEW,ESTABLISHED -s 172.30.56.61,172.30.56.62,172.30.56.63  -j ACCEPT
   ```


#### Start the service

1. Start BFMeta node program-BCF

   ```
   systemctl enable supervisord
   systemctl start supervisord 
   
   ```


## RockyLinux 8.x

### Prepare

- Disable SELinux

   ```
   sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/configselinux/config
   setenforce 0
    
   ```

- Install the software package
  
   ```
   yum install -y zip unzip epel-release

   ```
  
- Configure to add a clock source
  - Add clock source

   ```
   sed -i "/^pool/ a pool cn.pool.ntp.org iburst" /etc/chrony.conf
   ```

  - Synchronization time

   ```
   chronyc makestep
   ```

- Adjust the maximum number of files that users can use:

  ```
  echo -ne "
    * soft nofile 65536
    * hard nofile 65536
  " >> /etc/security/limits.conf
  ```

  

- Kernel parameter optimization
  - Set kernel parameters

  ```
  echo -ne "
     net.ipv4.tcp_max_tw_buckets=10000
	 net.ipv4.tcp_sack=1
	 net.ipv4.tcp_window_scaling=1
	 net.ipv4.tcp_rmem=4096 87380 4194304
	 net.ipv4.tcp_wmem=4096 16384 4194304
	 net.ipv4.tcp_max_syn_backlog=65536
	 net.core.netdev_max_backlog=65536
	 net.core.somaxconn=32768
	 net.core.wmem_default=8388608
	 net.core.rmem_default=8388608
	 net.core.wmem_max=16777216
	 net.core.rmem_max=16777216
	 net.ipv4.tcp_synack_retries=2
	 net.ipv4.tcp_syn_retries=2
	 net.ipv4.tcp_tw_reuse=1
	 net.ipv4.tcp_keepalive_time=1200
	 net.ipv4.tcp_mem=94500000 915000000 927000000
	 net.ipv4.tcp_max_orphans=3276800
	 net.ipv4.ip_local_port_range=9000 65000
	 vm.swappiness=0
	 net.ipv4.tcp_fin_timeout=15
	 fs.file-max=1048576
  " >> /etc/sysctl.conf  
  ```

  - Effective kernel parameter configuration

  ```
  sysctl -p  
  ```  



- Install dependencies

  ```
  yum -y install compat-openssl10  
  ```



 

### Public chain node deployment

#### Node program installation and configuration
- Minimum configuration to run BCF program：

   CPU: 2+ cores (X86，main frequency 3.0G+)

   Memory: 8G

   Hard Disk: SSD 500G or above

   System: RockyLinux 8.x

- we recommend that you use the following configuration equipment:

   CPU: 8 cores (main frequency 3.0G+)+

   Memory: 16G

   Hard Disk: SSD 2T or above

   System: RockyLinux 8.x



- The installation and configuration steps of the node program and the upgrade service program are as follows:

1. Create a directory for bfmchain

    ```
    mkdir -p /data
    ```

   

2. Upload the zip files (the compressed file contains the BFMeta program and the upgrade service program) and unzip it to the /data directory

   ```
   unzip -o -q bfmeta-mainnet-3.7.2-linux-full.zip -d /data/   
   ```

   

3. Grant executable permissions to the file

   ```
   chmod u+x /data/bfmeta/bcf
	chmod u+x /data/bfmeta/mongoComponents/linux/mongo*
	chmod u+x /data/upgrade/mongoComponents/linux/mongo*
	chmod u+x /data/upgrade/upgrade
   ```

4. Set the main program environment variable

   ```
   echo "export BCF_HOME=/data/bfmeta"  >> /etc/profile
   source  /etc/profile
   ```   
5. Run node program manually

   ```
	cd /data/bfmeta/
	./bcf
   ```   

6. Configure supervisor (manage BFMeta running in the background and upgrade service process)

- Install supervisor

  ```
  yum install -y supervisor  
  ```
- Configure managed nodes and upgrade service processes

   ```
   echo -ne "
   	[program:bcf]	
   	command=/data/bfmeta/bcf  ; 
   	directory=/data/bfmeta
   	autostart=true    ; 
   	user=root      ;
   	autorestart=unexpected   ;
   	exitcode=0,2   ;
   	startretries=3    ;
   	priority=999     ;
   	redirect_stderr=true ;
   	stdout_logfile_maxbytes=50MB  ;
   	stdout_logfile_backups = 20  ;
   	stdout_logfile=/data/bfmeta/logs/bcf.log    ;
   	stopasgroup=true   ;
   " >> /etc/supervisord.d/bcf.ini
   
   echo -ne "
   	[program:upgrade]
   	command=/data/upgrade/upgrade  ; 
   	directory=/data/upgrade
   	autostart=true       ; 
   	user=root           ;
   	autorestart=unexpected    ;
   	exitcode=0,2    ;
   	startretries=3       ;
   	priority=999         ;
   	redirect_stderr=true ;
   	stdout_logfile_maxbytes=50MB  ;
   	stdout_logfile_backups = 20   ;
   	stdout_logfile=/data/upgrade/logs/console.log      ;
   	stopasgroup=true     ;
   " >> /etc/supervisord.d/upgrade.ini
   ```
- Start supervisord service

  ```
  systemctl start supervisord  
  ``` 
- upervisor View process status

  ```
  supervisorctl status  
  ``` 
- Start/stop bcf service using supervisor

  ```
  supervisorctl start bcf
  supervisorctl stop bcf
  ``` 
- Set the supervisord service to start at boot

  ```
  systemctl enable supervisord
  ``` 
#### Firewall configuration

1. System firewall policy
   
   - View current firewall policy
   
   ```
   firewall-cmd --list-all   
   ```
   - Add an open service port
   
   ```
   firewall-cmd --zone=public --add-port=24000/tcp --add-port=24001/tcp --add-port=24002/tcp --add-port=24003/tcp --add-port=24005/tcp --add-port=24007/tcp --add-port=24009/tcp --permanent 
   ```  
   - Reload configuration
   
   ```
   firewall-cmd --reload   
   ```
   - Check the current firewall policy again
   
   ```
   firewall-cmd --list-all 
   ```
