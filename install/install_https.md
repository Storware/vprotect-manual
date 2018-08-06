# Enabling HTTPS connectivity for remote nodes

Default certificate presented by application server uses `localhost`. This works only for local node installations \(server and node on a single host\).

**Notice**:

* you can use default certificate - remember that you may need to use `./node_add_ssl_cert.sh` script after future updates to refresh certificate on the node
* for default certificate - jump to the Node configuration and use `localhost` instead of `vprotectserver.local` example

This section presents steps necessary to generate SSL certificate, setup vProtect to use it and how to regiester remote node.

**Notice**: When asked for certificate/keystore password use: `changeit`.

## vProtect Server \(new certificate\)

1. SSH to **vProtect Server** host
2. Generate key and certificate \(remember to provide **valid** vProtect Server DNS hostname - in our example it was `vprotectserver.local`\):

   ```text
   [root@localhost ~]# openssl req -x509 -newkey rsa:4096 -keyout vprotect.key -out vprotect.crt -days 365
   Generating a 4096 bit RSA private key
   ...............................................................................++
   .............................................................................................................................................................................................................................................................................................................................................++
   writing new private key to 'vprotect.key'
   Enter PEM pass phrase:
   Verifying - Enter PEM pass phrase:
   -----
   You are about to be asked to enter information that will be incorporated
   into your certificate request.
   What you are about to enter is what is called a Distinguished Name or a DN.
   There are quite a few fields but you can leave some blank
   For some fields there will be a default value,
   If you enter '.', the field will be left blank.
   -----
   Country Name (2 letter code) [XX]:PL
   State or Province Name (full name) []:
   Locality Name (eg, city) [Default City]:Warsaw
   Organization Name (eg, company) [Default Company Ltd]:Storware
   Organizational Unit Name (eg, section) []:
   Common Name (eg, your name or your server's hostname) []:vprotectserver.local
   Email Address []:
   ```

3. Create PKCS12 bundle from certificate and the key

   ```text
   [root@localhost ~]# openssl pkcs12 -export -in vprotect.crt -inkey vprotect.key -out vprotect.p12 -name vprotect
   Enter pass phrase for vprotect.key:
   Enter Export Password:
   Verifying - Enter Export Password:
   ```

4. Create a keystore for vProtect Server with PKCS12 bundle:

   ```text
   [root@localhost ~]# keytool -importkeystore -destkeystore /opt/vprotect/keystore.jks -srckeystore vprotect.p12 -srcstoretype PKCS12 -alias vprotect
   Enter destination keystore password:  
   Re-enter new password: 
   Enter source keystore password:
   ```

5. Change ownership on the keystore to the `vprotect` user:

   ```text
   chown vprotect:vprotect /opt/vprotect/keystore.jks
   ```

6. Edit `/opt/vprotect/payara.properties` and uncomment the following lines so they look like this:

   ```text
   #eu.storware.vprotect.http.port=8080
   #eu.storware.vprotect.https.port=8181
   #eu.storware.vprotect.db.host=localhost
   #eu.storware.vprotect.db.port=3306
   eu.storware.vprotect.ssl.certname=vprotect
   #payaramicro.userLogFile=/opt/vprotect/logs/appserver/server.log
   #payaramicro.logToFile=true
   javax.net.ssl.keyStore=/opt/vprotect/keystore.jks
   javax.net.ssl.keyStorePassword=changeit
   eu.storware.vprotect.db.user=vprotect
   ```

   ```text
   **Note: there are 3 lines uncommented above**:
   ```

   ```text
   eu.storware.vprotect.ssl.certname=vprotect
   javax.net.ssl.keyStore=/opt/vprotect/keystore.jks
   javax.net.ssl.keyStorePassword=changeit
   ```

7. Restart vProtect Server:

   ```text
   systemctl stop vprotect-server
   systemctl start vprotect-server
   ```

8. Make sure that your nodes resolve host name of the vProtect Server. You also can add entry in the `/etc/hosts` like this \(example IP: `1.2.3.4`\):

   ```text
   1.2.3.4    vprotectserver.local
   ```

9. Check with your browser that `https://VPROTECT_HOST:8181` presents certificate that you have just generated.
   * you also can execute `openssl` client from the node to print it \(check host name that you have provided in the certificate\):

     ```text
     openssl s_client -connect vprotectserver.local:8181 < /dev/null
     ```

## vProtect Node

1. SSH to **vProtect Node** host
2. Import server certificate using script under `/opt/vprotect/scripts` folder:

   ```text
   cd /opt/vprotect/scripts
   ./install_server_cert.sh [SERVER_HOST] [PORT] [KEYSTORE_PASS]
   ```

   where defaults \(if arguments have not been provided\) are `SERVER_HOST` = `localhost`, `PORT` = `8181`, `KEYSTORE_PASS` = `changeit` \(default java keystore password\), examples:

   * Default local installation \(Server and Node on the same host\):

     ```text
     ./install_server_cert.sh
     ```

   * Remote Server on the custom port:

     ```text
     ./install_server_cert.sh vprotectserver.local 8181
     ```

3. Register node with `NODE_NAME` of your choice `ADMIN_USER` user name which you would like to use and URL to vProtect API and provide password when prompted:
   * Syntax:

     ```text
     vprotect node -r NODE_NAME ADMIN_USER http(s)://VPROTECT_SERVER:PORT/api
     ```

   * Remote server with generated certificate:

     ```text
     vprotect node -r node1 admin https://vprotectserver.local:8181/api
     ```

   * Local installation with default certificate:

     ```text
     vprotect node -r node1 admin https://localhost:8181/api
     ```

