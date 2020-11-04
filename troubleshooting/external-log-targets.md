# External log targets

vProtect uses log4j2 for logging both in the Node and Server. This module allows to write to external log targets. All supported appenders are described here: [https://logging.apache.org/log4j/2.x/manual/appenders.html](https://logging.apache.org/log4j/2.x/manual/appenders.html).

Below, you can find information on how to set Syslog as a target for vProtect Node.

1. to support Syslogs, make sure you have `rsyslog` package - if not you can install it like this:

   ```text
   sudo yum -y install rsyslog
   ```

2. in `/etc/rsyslog.conf` file:
   * uncomment these lines to enable UDP socket transport:

     ```text
     #module(load="imudp") # needs to be done just once
     #input(type="imudp" port="514")
     ```

   * add this line under `#### GLOBAL DIRECTIVES ####` section to support newline characters:

     ```text
     $EscapeControlCharactersOnReceive off
     ```
3. open 514 port for UDP connection

   ```text
   firewall-cmd --permanent --add-port=514/udp
   firewall-cmd --reload
   ```

4. enable rsyslog service

   ```text
   systemctl enable --now rsyslog
   ```

5. In `log4j2-node.xml` file add:
   * in `Appenders` section add Syslog appender:

     ```text
     <Socket name="Syslog" host="localhost" port="514" protocol="UDP">
     <PatternLayout 
     pattern="$${hostName} vprotect-node: %level [%t] %c{1}.%M:%L %n[$${ctx:task:-}] %msg%n%n"/>
     </Socket>
     ```

   * in `Loggers` add reference to Syslog appender in `Root` section:

     ```text
     <AppenderRef ref="Syslog"/>
     ```
6. restart vProtect Node service:

   ```text
   systemctl restart vprotect-node
   ```

Example of log4j2-node.xml after modifications:

```text
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
    <Properties>
        <Property name="logLevel">DEBUG</Property>
        <Property name="baseDir">/opt/vprotect/logs/$${sys:node:-null}</Property>
        <Property name="vmwareBaseDir">/opt/vprotect/logs/vmware</Property>
        <Property name="daemonLogFileName">vprotect_daemon.log</Property>
        <Property name="clientLogFileName">vprotect_client.log</Property>
        <Property name="vmwareLogFileName">vprotect_vmware.log</Property>
    </Properties>
    <Appenders>
        <Routing name="Routing">
            <Routes pattern="$${sys:port}">
                <Route key="$${sys:port}">
                    <RollingFile name="CLI" filename="${baseDir}/${clientLogFileName}"
                                 filepattern="${baseDir}/${clientLogFileName}.%i">
                        <PatternLayout pattern="[%d{yyyy-MM-dd HH:mm:ss}] %level [%t] %c{1}.%M:%L %n%msg%n%n"/>
                        <Policies>
                            <SizeBasedTriggeringPolicy size="8 MB"/>
                        </Policies>
                        <DefaultRolloverStrategy max="50">
                            <Delete basePath="${baseDir}" maxDepth="1">
                                <IfFileName glob="${clientLogFileName}.*"/>
                                <IfAccumulatedFileCount exceeds="50"/>
                            </Delete>
                        </DefaultRolloverStrategy>
                    </RollingFile>
                </Route>
                <Route>
                    <RollingFile name="Engine" filename="${baseDir}/${daemonLogFileName}"
                                 filepattern="${baseDir}/${daemonLogFileName}.%i">
                        <PatternLayout pattern="[%d{yyyy-MM-dd HH:mm:ss.SSS}] %level [%t] %c{1}.%M:%L %n[$${ctx:task:-}] %msg%n%n"/>
                        <Policies>
                            <SizeBasedTriggeringPolicy size="8 MB"/>
                        </Policies>
                        <DefaultRolloverStrategy max="50">
                            <Delete basePath="${baseDir}" maxDepth="1">
                                <IfFileName glob="${daemonLogFileName}.*"/>
                                <IfAccumulatedFileCount exceeds="50"/>
                            </Delete>
                        </DefaultRolloverStrategy>
                    </RollingFile>
                </Route>
            </Routes>
        </Routing>
​
        <Console name="StdOut" target="SYSTEM_OUT">
            <PatternLayout pattern="%msg%n"/>
        </Console>
​
        <RollingFile name="VMware" filename="${vmwareBaseDir}/${vmwareLogFileName}"
                     filepattern="${vmwareBaseDir}/${vmwareLogFileName}.%i" fileGroup="vprotect" fileOwner="vprotect"
                     filePermissions="rw-rw-rw-">
            <PatternLayout pattern="[%d{yyyy-MM-dd HH:mm:ss}] %level [%t] %c{1}.%M:%L %n%msg%n%n"/>
            <Policies>
                <SizeBasedTriggeringPolicy size="8 MB"/>
            </Policies>
            <DefaultRolloverStrategy max="50">
                <Delete basePath="${vmwareBaseDir}" maxDepth="1">
                    <IfFileName glob="${vmwareLogFileName}.*"/>
                    <IfAccumulatedFileCount exceeds="50"/>
                </Delete>
            </DefaultRolloverStrategy>
        </RollingFile>

    <Socket name="Syslog" host="localhost" port="514" protocol="UDP">
            <PatternLayout pattern="$${hostName} vprotect-node: %level [%t] %c{1}.%M:%L %n[$${ctx:task:-}] %msg%n%n"/>
    </Socket>   
    </Appenders>
    <Loggers>
        <Root level="${logLevel}">
            <AppenderRef ref="Routing"/>
            <AppenderRef ref="Syslog"/>
        </Root>
        <Logger name="StdOut" additivity="false">
            <AppenderRef ref="StdOut"/>
        </Logger>
        <Logger name="VMware" level="${logLevel}" additivity="false">
            <AppenderRef ref="VMware"/>
        </Logger>
    </Loggers>
```

