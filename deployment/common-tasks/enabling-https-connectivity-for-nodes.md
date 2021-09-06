# Enabling HTTPS connectivity for nodes

The default certificate presented by the application server uses `localhost.localdomain`. This works only for local node installations \(server and node on a single host\).

**Note**:

* When asked for the certificate/keystore password use: `changeit`.
* You can use the default certificate - remember that you may need to use the `./node_add_ssl_cert.sh` script after future updates to refresh the certificate on the node
* For the default certificate - jump to the Node configuration and use the `localhost.localdomain` instead of the `vprotectserver.local` example
* When registering the node locally over HTTPS please note that the URL you should use is`localhost.localdomain` - **NOT** `localhost`

This section presents the steps necessary for generating an SSL certificate, for setup vProtect to use it and how to register a remote node.

## vProtect Server \(when using own certificate\)

This section describes certificate generation and import on the vProtect Server side. It uses a self-signed certificate. If you would like to use CSR and your own CA instead - please check for additional steps described in the next section.

1. SSH to **vProtect Server** host
2. Generate the key and certificate \(remember to provide a **valid** vProtect Server DNS hostname - in our example it was `vprotectserver.local`\):

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
   Organization Name (eg, company) [Default Company Ltd]: your Company
   Organizational Unit Name (eg, section) []:
   Common Name (eg, your name or your server's hostname) []:vprotectserver.local
   Email Address []:
   ```

3. Create the PKCS12 bundle from the certificate and the key:

   ```text
   [root@localhost ~]# openssl pkcs12 -export -in vprotect.crt -inkey vprotect.key -out vprotect.p12 -name vprotect
   Enter pass phrase for vprotect.key:
   Enter Export Password:
   Verifying - Enter Export Password:
   ```

4. Create a keystore for the vProtect Server with the PKCS12 bundle:

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

6. Edit `/opt/vprotect/payara.properties` and change the path to the keystore: `javax.net.ssl.keyStore=/opt/vprotect/keystore.jks`
7. Restart the vProtect Server:

   ```text
   systemctl stop vprotect-server
   systemctl start vprotect-server
   ```

8. Make sure that your nodes resolve the host name of the vProtect Server. You also can add an entry in the `/etc/hosts` like this \(example IP: `1.2.3.4`\):

   ```text
   1.2.3.4    vprotectserver.local
   ```

9. Check with your browser that `https://VPROTECT_HOST:8181` presents the certificate that you have just generated.
   * you also can execute the `openssl` client from the node to print it \(check the host name that you have provided in the certificate\):

     ```text
     openssl s_client -connect vprotectserver.local:8181 < /dev/null
     ```

## Notes on using your own certificate with CSR and your own CA

When using CSR to get a trusted certificate, you need to replace step 2 \(self-signed certificate generation\) with several steps including CSR generation, and download the CRT signed by your CA. The steps are as follows

1. Generate the CSR - answer the same set of questions as above:`openssl req -new -newkey rsa:2048 -nodes -keyout vprotect.key -out vprotect.csr`
2. Send your CSR and have it signed by your CA
3. Download your CRT file and save it as `vprotect.crt` \(note that you should have your working directory set to `/opt/vprotect`\)
4. Download your CA certificate chain \(example for a single `ca.crt`\) and import it with the `CA_ALIAS` of your choice as follows: `keytool -import -trustcacerts -keystore /usr/lib/jvm/jre/lib/security/cacerts -storepass changeit -noprompt -alias CA_ALIAS -file ca.crt`
5. Now continue from PKCS12 bundle generation \(step 3 in the section above\).

## vProtect Node \(any SSL certificate\)

1. SSH to **vProtect Node** host
2. Import the server certificate using the script under the `/opt/vprotect/scripts` folder:

   ```text
   cd /opt/vprotect/scripts
   ./node_add_ssl_cert.sh [SERVER_HOST] [PORT] [KEYSTORE_PASS]
   ```

   where the defaults \(if arguments have not been provided\) are `SERVER_HOST` = `127.0.0.1`, `PORT` = `8181`, `KEYSTORE_PASS` = `changeit` \(default java keystore password\), examples:

   * Default local installation \(Server and Node on the same host\):

     ```text
     ./node_add_ssl_cert.sh
     ```

   * Remote Server on the custom port:

     ```text
     ./node_add_ssl_cert.sh vprotectserver.local 8181
     ```

3. Register the node with the `NODE_NAME` of your choice, the `ADMIN_USER` user name which you would like to use and the URL to vProtect API, and provide the password when prompted:
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
     vprotect node -r node1 admin https://localhost.localdomain:8181/api
     ```

