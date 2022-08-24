# Terracota-Server-Array
Install and Config Terracota to clustering Webmethods Integration Server on Linux

## Minimum and Recommended Hardware Requirements
The table below the minimum and recommended (in parentheses) hardware requirements
| Hard Drive Space      | RAM         | Cores         |
| :---                  |    :----:   |          ---: |
| 500MB (10GB)          | 2GB (4GB)   | 2 (8)         |


## Download and Install 


Link download

`https://mega.nz/fm/jS5kDYQb`


Step to Install 
  ```bash
  $ chmod +x SoftwareAGInstaller20210517-Linux_x86_64.bin

  $ ./SoftwareAGInstaller20210517-Linux_x86_64.bin -readImage TC107Linux.zip
  ```

  Select BigMemory Max

  Insert License 



## Configuration

  After Installation Complete, first you must create configuration file, we call tc-config.xml
  down below is example tc-config.xml


```xml
<?xml version="1.0" encoding="UTF-8" ?>
<tc:tc-config xmlns:tc="http://www.terracotta.org/config"
              xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

  <tc-properties>
        <property value="true" name="ehcache.storageStrategy.dcv2.perElementTTITTL.enabled"/>
        <property value="1024" name="logging.maxLogFileSize"/>
  </tc-properties>

<servers>
<mirror-group group-name="TSA-LOCAL" election-time="5">
    <server host="192.168.1.21" name="TSA-SMG" bind="192.168.1.21">
      <data>/appl/TSA/Terracotta/server-data</data>
      <logs>/appl/TSA/Terracotta/server-logs</logs>
      <index>/appl/TSA/Terracotta/server-index</index>
      <tsa-port bind="192.168.1.21">8510</tsa-port>
      <jmx-port bind="192.168.1.21">8520</jmx-port>
      <tsa-group-port bind="192.168.1.21">8530</tsa-group-port>
      <management-port bind="192.168.1.21">8540</management-port>
          <dataStorage size="2g">
                <offheap size="2g"/>
                <hybrid/>
          </dataStorage>
   </server>
</mirror-group>

    <update-check>
      <enabled>false</enabled>
      <period-days>10</period-days>
    </update-check>

    <garbage-collection>
      <enabled>true</enabled>
      <verbose>false</verbose>
      <interval>3600</interval>
    </garbage-collection>
    <restartable enabled="true"/>
    <client-reconnect-window>120</client-reconnect-window>
  </servers>
  <clients>
    <logs>logs-%i</logs>
  </clients>
</tc:tc-config>
```

put that file configuration on installationDir/Terracota/server/bin/

after that you can start tc server with this command :
```bash
$ cd /appl/TSA/Terracotta/server/bin
$ nohup ./start-tc-server.sh -n TSA-SMG -f tc-config.xml &
```

```desciption
-n is the name of terracota cluster
-f is the file configuration
```

after tc server was started, then you can start up the terracota management console
```bash
$ cd /appl/TSA/Terracotta/tools/management-console/bin/
$ nohup ./start-tmc.sh &
```

To Access TMC, first you must allow that port
```bash
$ sudo firewall-cmd --add-port=9889/tcp --permanent
$ sudo firewall-cmd --add-port=8510/tcp --permanent
$ sudo firewall-cmd --reload
```

```description
9889 is port TMC
8510 is port to connect local
```

### Configuration on Integration Server

Login To IS Console

    - Security
    - Clustering
    - Enable Cluster and Edit
    fill CLuster Name must same like on TMC or same like when you start the tc server
    fill Action On Startup Error : Start as Stand-Alone Integration Server 
    Terracotta Server Array URLs : <ip:port> <- tc server







 
