# External log targets

vProtect uses log4j2 for logging both in the Node and Server. This module allows to write to external log targets. All supported appenders are described here: [https://logging.apache.org/log4j/2.x/manual/appenders.html](https://logging.apache.org/log4j/2.x/manual/appenders.html).

Below, you can find information on how to set Syslog as a target for vProtect Server and Node.

1. To support Syslogs, make sure you have `rsyslog` package - if not you can install it like this:

   ```text
   sudo yum -y install rsyslog
   ```

2. In `/etc/rsyslog.conf` file:
   * uncomment these lines to enable UDP socket transport:

     ```text
     #module(load="imudp") # needs to be done just once
     #input(type="imudp" port="514")
     ```

   * add this line under `#### GLOBAL DIRECTIVES ####` section to support newline characters:

     ```text
     $EscapeControlCharactersOnReceive off
     ```
3. Open 514 port for UDP connection

   ```text
   firewall-cmd --permanent --add-port=514/udp
   firewall-cmd --reload
   ```

4. Enable rsyslog service

   ```text
   systemctl enable --now rsyslog
   ```

5. To use Syslog's with vProtect Server add in `log4j2-server.xml` file:
   * in `Appenders` section add Syslog appender:

     ```text
     <Socket name="Syslog" host="localhost" port="514" protocol="UDP">
     <PatternLayout 
     pattern="$${hostName} vprotect-server: %level [%t] %c{1}.%M:%L %n[$${ctx:task:-}] %msg%n%n"/>
     </Socket>
     ```

   * in `Loggers` add reference to Syslog appender in `Root` section:

     ```text
     <AppenderRef ref="Syslog"/>
     ```
6. Restart vProtect Server service:

   ```text
   systemctl restart vprotect-server
   ```

Example of log4j2-server.xml after modifications:

```text
<?xml version="1.0" encoding="UTF-8"?>
<Configuration status="WARN">
    <Properties>
        <Property name="baseDir">/opt/vprotect/logs/api</Property>
    </Properties>
    <Appenders>
        <Console name="Console" target="SYSTEM_OUT">
            <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss} %-5p [%t] %c{1}.%M:%L - %msg%n"/>
        </Console>

        <RollingFile name="RollingFile" filename="${baseDir}/api.log"
                     filepattern="${baseDir}/api.log_%d{yyyy-MM-dd--HH.mm.ss}.log.zip" fileOwner="vprotect"
                     fileGroup="vprotect" filePermissions="rw-rw----">
            <PatternLayout pattern="[%d{yyyy-MM-dd HH:mm:ss}] %-5p [%t] %X %c{1}.%M:%L%n%msg%n%n"/>
            <Policies>
                <SizeBasedTriggeringPolicy size="2 MB" />
            </Policies>
            <DefaultRolloverStrategy max="20">
                <Delete basePath="${baseDir}" maxDepth="1">
                    <IfFileName glob="api.log_*.log.zip"/>
                    <IfAccumulatedFileCount exceeds="20"/>
                </Delete>
            </DefaultRolloverStrategy>
        </RollingFile>

        <Socket name="Syslog" host="localhost" port="514" protocol="UDP">
            <PatternLayout pattern="$${hostName} vprotect-server: %level [%t] %c{1}.%M:%L %n[$${ctx:task:-}] %msg%n%n"/>
        </Socket>
    </Appenders>
    <Loggers>
	<Root level="DEBUG">
            <AppenderRef ref="RollingFile" />
            <AppenderRef ref="Syslog" />
        </Root>
    </Loggers>
</Configuration>
```

7. To use Syslog's with vProtect Node add in `log4j2-node.xml` file:
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
8. Restart vProtect Node service:

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
</Configuration>
```

**Notice**: If you want to use Syslogs in both vProtect Server and Node you need to use two different udp ports and specify them in `/etc/rsyslog.conf`
