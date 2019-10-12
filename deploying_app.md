---

copyright:
  years: 2014, 2019
lastupdated: "2019-4-4"

keywords:

subcollection: InformixOnCloud

---

<!-- Attribute definitions -->
{:javascript: #javascript .ph data-hd-programlang='javascript'}
{:java: #java .ph data-hd-programlang='java'}
{:ruby: #ruby .ph data-hd-programlang='ruby'}
{:php: #php .ph data-hd-programlang='php'}
{:python: #python .ph data-hd-programlang='python'}
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:tip: .tip}
{:important: .important}
{:note: .note}
{:deprecated: .deprecated}
{:pre: .pre}

# Managing your Informix on Cloud server
## Logging in to your Informix on Cloud server

Log in to your Informix on Cloud machine using an SSH client. Details are provided in a
welcome email after your system is ready. Upon logging in the first time, you are immediately
required to change your password.
{: shortdesc}

## Administering your server
IBM® Informix® on Cloud for IBM Cloud is based on IBM Informix Advanced Enterprise Server Edition.
{: shortdesc}

You administer your Informix on Cloud Informix Server with the same utilities and tools that you use for your on-premises Informix Servers. See [Informix documentation](http://www.ibm.com/support/knowledgecenter/SSGU8G_14.1.0/com.ibm.welcome.doc/welcome.htm) for the latest information about the Informix Server.

For instructions on how to successfully back up your Informix Server, please refer to the IBM Informix [Backup and Restore Guide](https://www.ibm.com/support/knowledgecenter/en/SSGU8G_14.1.0/com.ibm.bar.doc/bar.htm)

## Securing your Informix on Cloud server
The IBM Informix on Cloud for IBM Cloud server is configured by default to allow SSH and SSL-based communication to occur only. Network access to the
server is secured using the iptables firewall.
{: shortdesc}
Run this command to display the rules and status of the iptable firewall:

```sudo iptables -L -n```

If different settings are desired than the defaults, you have complete control to make these changes. For example, you can:

<ul>
<li>Allow access to the server from a group of IP addresses only, also called a subnet. A subnet restricts access from users who are not part of the subnet. For example, the following rule allows incoming ssh connections from the `192.168.100.X` network
only:
<pre class="codeblock"><code>iptables -A INPUT -i eth0 -p tcp -s 192.168.100.0/24 --dport 22 -m state --state NEW,ESTABLISHED -j ACCEPT</code></pre>
</li>
<li>Allow access through a single, approved IP address only. For
example:
<pre class="codeblock"><code>iptables -A INPUT -i eth0 -p tcp -s 192.168.100.11 --dport 22 -m state --state NEW, ESTABLISHED -j ACCEPT</code></pre>
</li>
</ul>

## Server instance information
IBM provides you with an initially configured IBM Informix on Cloud for IBM Cloud server ready to interact with clients that use SQLI, DRDA, Mongo, and REST protocols. The initial configuration represents a starting point, and can be further customized by you to suit your business needs. You have complete flexibility and control to customize Informix, including the operating system, and more.
{: shortdesc}
The Informix on Cloud server is configured to
interact with clients using the following protocols on the following ports. The firewall is configured to allow traffic through to the SSL ports only.

<table><caption>Table 1. Server Instance Ports</caption><thead><tr><th style="width: 33.33333333333333%" id="d286e233" class="thleft">Protocol</th>
<th style="width: 33.33333333333333%" id="d286e235" class="thleft">Without SSL</th>
<th style="width: 33.33333333333333%" id="d286e237" class="thleft">With SSL</th>
</tr>
</thead>
<tbody><tr><td style="width: 33.33333333333333%" headers="d286e233 ">SQLI</td>
<td style="width: 33.33333333333333%" headers="d286e235 ">9088</td>
<td style="width: 33.33333333333333%" headers="d286e237 ">9089</td>
</tr>
<tr><td style="width: 33.33333333333333%" headers="d286e233 ">DRDA</td>
<td style="width: 33.33333333333333%" headers="d286e235 ">9090</td>
<td style="width: 33.33333333333333%" headers="d286e237 ">9091</td>
</tr>
<tr><td style="width: 33.33333333333333%" headers="d286e233 ">Mongo</td>
<td style="width: 33.33333333333333%" headers="d286e235 ">27017</td>
<td style="width: 33.33333333333333%" headers="d286e237 ">27018</td>
</tr>
<tr><td style="width: 33.33333333333333%" headers="d286e233 ">REST</td>
<td style="width: 33.33333333333333%" headers="d286e235 ">80</td>
<td style="width: 33.33333333333333%" headers="d286e237 ">443</td>
</tr>
<tr><td style="width: 33.33333333333333%" headers="d286e233 ">InformixHQ Server HTTP</td>
<td style="width: 33.33333333333333%" headers="d286e235 ">8080</td>
<td style="width: 33.33333333333333%" headers="d286e237 ">8443</td>
</tr>
<tr></tr>
</tbody>
</table>

As part of the initial configuration, an `Informix` operating system user is created without a password. To connect to the database, a number of options are available to you as a client to choose from:
<ul><li>Set the password for the `Informix` user.</li>
<li>Create a new operating system user.</li>
<li>Configure Informix to authenticate through a PAM service.</li>
<li>[Create a mapped user](http://www.ibm.com/support/knowledgecenter/SSGU8G_14.1.0/com.ibm.sec.doc/ids_am_037.htm).</li>
</ul>

## Connecting to your Informix on Cloud server
You can use any application that can access the internet to connect to your IBM Informix on Cloud for IBM Cloud server. The application can be in your data center or be hosted in IBM Cloud.
{: shortdesc}
The Informix on Cloud server is configured with a self-signed SSL certificate, located on the machine at `/home/informix/client_ssl/selfsigned_ssl.cert`. By default the Informix on Cloud server accepts only SSL-enabled connections, so you must first configure a client for SSL.

The following steps explain how to configure a SQLI-based IBM Informix client driver for establishing an SSL connection with the server. All these steps are performed on the machine where the
client runs.

<ol><li>Ensure that the IBM Informix Client Software Development Kit (CSDK) and GSKit are installed.</li>
<li>Identify a directory to create the client's keystore> For example: the `/opt/ifx/mykeystores` directory. Change to that directory.</li>
<li>Copy the SSL certificate from the Informix on Cloud server in the
`/home/informix/client_ssl/selfsigned_ssl.cert` path to the local directory. You can do this by using the `scp` command.</li>
<li>Create a new keystore:

<pre class="codeblock"><code>gsk8capicmd_64 -keydb -create -db mydbclient.kdb -pw MyPassword -type cms -stash</code></pre>

<p>The above command creates a key database called `mydbclient.kdb` and a stash file called `mydbclient.sth`.

The `-stash` option creates a stash file
at the same path as the key database, with a file extension of `.sth`. At connect time, GSKit uses the stash file to obtain the password to the key database.</p>
</li>
<li>Add the certificate to keystore:  
<pre class="codeblock"><code>gsk8capicmd_64 -cert -add -db mydbclient.kdb -pw MyPassword -label selfsigned_ssl -file selfsigned_ssl.cert -format ascii</code></pre>
The above command imports the certificate from the file `selfsigned_ssl.cert` into the key database called `mydbclient.kdb`. <br>You may also consider applying appropriate OS level access restrictions to avoid unauthorized access.</li>
<li>Edit a file `$INFORMIXDIR/etc/conssl.cfg` to contain the following
lines:
<pre class="codeblock"><code>SSL_KEYSTORE_FILE /opt/ifx/keystores/mydbclient.kdb
SSL_KEYSTORE_STH /opt/ifx/keystores/mydbclient.sth</code></pre>

If `conssl.cfg` does not exist or if any of the above parameters are not configured, the client keystore and stash file will default to
`$INFORMIXDIR/etc/client.kdb` and `$INFORMIXDIR/etc/client.sth`.


The IBM Informix client drivers are now ready to make SQLI connections using SSL.
</li>
</ol>

<ul>
<li>[Client side SSL setup needed for IBM Informix JDBC application](https://www.ibm.com/support/knowledgecenter/SSGU8G_14.1.0/com.ibm.jdbc.doc/ids_jdbc_490.htm)</li>
<li>[Client side SSL setup needed for DRDA based non-Java client](https://www.ibm.com/support/knowledgecenter/SSEPGG_11.1.0/com.ibm.db2.luw.admin.sec.doc/doc/t0053518.html?view=embed)</li>
<li>[Client side SSL setup needed for DRDA based Java client (IBM JCC)](http://www.ibm.com/support/knowledgecenter/SSSNY3_10.1.0/com.ibm.db2.luw.apdv.java.doc/src/tpc/imjcc_c0024688.html)</li>
<li>[Setting up MongoDB remote clients for SSL](http://www.ibm.com/support/knowledgecenter/SSB23S_1.1.0.13/gtpd5/tmdbssl.html)</li>
</ul>

For more information about configuring Informix client driver for SSL connection, refer to the Informix Security Guide's [Secure sockets layer protocol](http://www.ibm.com/support/knowledgecenter/SSGU8G_14.1.0/com.ibm.sec.doc/ids_ssl_001.htm) topic in IBM Knowledge Center.


## InformixHQ
The web-based monitoring and administration tool, InformixHQ, is also configured and running.  The HQ Server and Agent processes run as the OS user "informixhqu".  Using a browser connect to InformixHQ at https://127.0.0.1:8443/.   Details about the initial credentials can be found in the /opt/informix/hq/config/informixhq-server.properties file. 
