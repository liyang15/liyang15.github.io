---
layout: post
title:  "OpenSSL Certification Authority (CA) on Ubuntu Server"
categories: netdevops
tags: Network Engineer
date: 2024-1-9
---

# OpenSSL Certification Authority (CA) on Ubuntu Server

OpenSSL is a free, open-source library that you can use for digital certificates. One of the things you can do is build your own CA (Certificate Authority).

A CA is an entity that signs digital certificates. An example of a well-known CA is [Verisign](https://www.verisign.com/). Many websites on the Internet use certificates for their HTTPS connections that were signed by Verisign.

Besides websites and HTTPS, there are some other applications/services that can use digital certificates. For example:

- VPNs: instead of using a pre-shared key you can use digital certificates for authentication.
- Wireless: WPA 2 enterprise uses digital certificates for client authentication and/or server authentication using PEAP or EAP-TLS.

Instead of paying companies like Verisign for all your digital certificates. It can be useful to build your own CA for some of your applications. In this lesson, you will learn how to create your own CA.

## Configuration

In my examples, I will use a Ubuntu server, the configuration of openSSL will be similar though on other distributions like CentOS.









### Prerequisites

Before we configure OpenSSL, I like to configure the hostname/FQDN correctly and make sure that our time, date and timezone is correct.

Let’s take a look at the hostname:

```
vmware@ca:~$ hostname
ca
```

My hostname is “ca”. Let’s check the FQDN:

```
vmware@ca:~$ hostname -f
ca
```

It’s also “ca”. Let’s change the FQDN; you need to edit the following file for this:

```
$ sudo vim /etc/hosts 
```

Change the following line:

```
127.0.1.1       ca
```

To:

```
127.0.1.1       ca.networklessons.local ca
```

Let’s verify the hostname and FQDN again:

```
vmware@ca:~$ hostname
ca
vmware@ca:~$ hostname -f
ca.networklessons.local
```

Our hostname and FQDN is now looking good.

We could configure the time/date manually, but it might be a better idea to use NTP. You can synchronize the time/date with this command:

```
$ sudo ntpdate pool.ntp.org
29 Mar 19:46:44 ntpdate[16478]: adjust time server 149.210.205.44 offset 0.062135 sec
```

But it might be a better idea to synchronize periodically. Let’s install the NTP tools:

```
$ sudo apt-get install ntp
```

Your Ubuntu server will use the following NTP server pools by default:

```
$ cat /etc/ntp.conf | grep server
# Specify one or more NTP servers.
# Use servers from the NTP Pool Project. Approved by Ubuntu Technical Board
server 0.ubuntu.pool.ntp.org
server 1.ubuntu.pool.ntp.org
server 2.ubuntu.pool.ntp.org
server 3.ubuntu.pool.ntp.org
```

You can verify which servers it is currently using with the following command:

```
$ ntpq -p
     remote           refid      st t when poll reach   delay   offset  jitter
==============================================================================
 notax.pointpro. 193.79.237.14    2 u   14   64    3   13.607   16.002  31.631
 ntp.luna.nl     193.67.79.202    2 u   12   64    3   11.728   13.030  32.101
 ntp1.edutel.nl  80.94.65.10      2 u   11   64    3   16.193   12.460  31.346
 dsl-083-247-002 193.67.79.202    2 u    9   64    3   13.893   11.284  32.550
 juniperberry.ca 193.79.237.14    2 u    9   64    3   20.803   11.177  31.101
```

Our server is now configured correctly.

### OpenSSL Configuration

OpenSSL uses a configuration file that is easy to read. There are a couple of things that we will change in it:

```
# vim /usr/lib/ssl/openssl.cnf
```

Look for the following section:

```
[ CA_default ]

dir		= ./demoCA
```

And change it, so it looks like this:

```
[ CA_default ]                                                                                 
                                                                                               
dir             = /root/ca
```

The “/root/ca” folder is where we will store our private keys and certificates.

You might also want to take a look at the default policy:

```
[ policy_match ]
countryName		= match
stateOrProvinceName	= match
organizationName	= match
organizationalUnitName	= optional
commonName		= supplied
emailAddress		= optional
```

Some fields like country, state/province, and organization have to match. If you are building your CA for a lab environment like I am then you might want to change some of these values:

```
[ policy_match ]
countryName             = match
stateOrProvinceName     = optional
organizationName        = optional
organizationalUnitName  = optional
commonName              = supplied
emailAddress            = optional
```

I’ve changed it so that only the country name has to match.

### Root CA

The first thing we have to do is to create a root CA. This consists of a private key and root certificate. These two items are the “identity” of our CA.

Let’s switch to the root user:

```
$ sudo su
```

We will create a new folder which stores all keys and certificates:

```
# mkdir /root/ca
```

In this new folder we have to create some additional sub-folders:

```
# cd /root/ca
# mkdir newcerts certs crl private requests
```

We also require two files. The first one is called “index.txt”. This is where OpenSSL keeps track of all signed certificates:

```
# touch index.txt
```

The second file is called “serial”. Each signed certificate will have a serial number. I will start with number 1234:

```
# echo '1234' > serial
```

All folders and files are in place. Let’s generate the root private key:

```
# openssl genrsa -aes256 -out private/cakey.pem 4096
Generating RSA private key, 4096 bit long modulus
..++
..................++
e is 65537 (0x10001)
Enter pass phrase for private/cakey.pem:
Verifying - Enter pass phrase for private/cakey.pem:
```

The root private key that I generated is 4096 bit and uses AES 256 bit encryption. It is stored in the private folder using the “cakey.pem” filename.

Anyone that has the root private key will be able to create trusted certificates. Keep this file secure!

We can now use the root private key to create the root certificate:

```
# openssl req -new -x509 -key /root/ca/private/cakey.pem -out cacert.pem -days 3650 -set_serial 0
Enter pass phrase for /root/ca/private/cakey.pem:
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:NL
State or Province Name (full name) [Some-State]:North-Brabant
Locality Name (eg, city) []:Tilburg
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Networklessons
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:CA.networklessons.local
Email Address []:admin@networklessons.local
```

The root certificate will be saved as the “cacert.pem” filename and is valid for 10 years.

### Create a certificate

Our root CA is now up and running. Normally when you want to install a certificate on a device (a web server for example), then the device will generate a CSR (Certificate Signing Request). This CSR is created by using the private key of the device.

On our CA, we can then sign the CSR and create a digital certificate for the device.

Another option is that we can do everything on our CA. We can generate a private key, CSR and then sign the certificate…everything “on behalf” of the device.

That’s what I am going to do in this example; it’s a good way to test if your CA is working as expected.

I’ll generate a private key, CSR and certificate for an imaginary “web server”.

Let’s use the requests folder for this:

```
# cd /root/ca/requests/
```

First, we have to generate a private key:

```
# openssl genrsa -aes256 -out some_serverkey.pem 2048
Generating RSA private key, 2048 bit long modulus
..............................+++
....+++
e is 65537 (0x10001)
Enter pass phrase for some_server.pem:
Verifying - Enter pass phrase for some_server.pem:
```

The private key will be 2048 bit and uses AES 256 bit encryption. With the private key, we can create a CSR:

```
root@ca:~/ca/requests# openssl req -new -key some_serverkey.pem -out some_server.csr
Enter pass phrase for some_serverkey.pem:
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:NL
State or Province Name (full name) [Some-State]:North-Brabant
Locality Name (eg, city) []:Tilburg
Organization Name (eg, company) [Internet Widgits Pty Ltd]:Networklessons
Organizational Unit Name (eg, section) []:
Common Name (e.g. server FQDN or YOUR name) []:some_server.networklessons.local
Email Address []:admin@networklessons.local

Please enter the following 'extra' attributes
to be sent with your certificate request
A challenge password []:
An optional company name []:
```

Now we can sign the CSR that we just created:

```
# openssl ca -in some_server.csr -out some_server.pem 
Using configuration from /usr/lib/ssl/openssl.cnf
Enter pass phrase for /root/ca/private/cakey.pem:
Check that the request matches the signature
Signature ok
Certificate Details:
        Serial Number: 4660 (0x1234)
        Validity
            Not Before: Apr  1 09:08:59 2016 GMT
            Not After : Apr  1 09:08:59 2017 GMT
        Subject:
            countryName               = NL
            stateOrProvinceName       = North-Brabant
            organizationName          = Networklessons
            commonName                = some_server.networklessons.local
            emailAddress              = admin@networklessons.local
        X509v3 extensions:
            X509v3 Basic Constraints: 
                CA:FALSE
            Netscape Comment: 
                OpenSSL Generated Certificate
            X509v3 Subject Key Identifier: 
                57:A7:7A:41:3E:3F:B3:EE:0D:CF:46:D0:A7:A5:9B:46:92:D1:F0:AD
            X509v3 Authority Key Identifier: 
                keyid:1B:38:B6:9F:82:46:72:5A:04:07:76:C2:DA:A5:5D:EB:95:83:81:30

Certificate is to be certified until Apr  1 09:08:59 2017 GMT (365 days)
Sign the certificate? [y/n]:y


1 out of 1 certificate requests certified, commit? [y/n]y
Write out database with 1 new entries
Data Base Updated
```

That’s all there is to it. The “some_server.pem” file is the signed digital certificate for our web server. If you want you can delete the CSR, move the private key to the “private” folder, and move the new certificate to the “certs” folder:

```
# rm some_server.csr
# mv some_serverkey.pem /root/ca/private/
# mv some_server.pem /root/ca/certs/
```

The “some_server.pem” certificate can now be installed on your web server.

### Security

Protecting your CA is important. Anyone that has access to the private key of the CA will be able to create trusted certificates.

One of the things you should do is reducing the permissions on the entire /root/ca folder so that only our root user can access it:

```
# chmod -R 600 /root/ca
```

In this example, we used the root CA to sign the certificate of an imaginary web server directly. This is fine for a lab environment but for a production network, you should use an intermediate CA.

The intermediate CA is another server that signs certificates on behalf of the root CA.

The root CA signs the certificate of the intermediate CA. You can then take the root CA offline which reduces the chance of anyone getting their hands on your root private key.

## Verification

We created some private keys and generated some certificates. Let’s take a closer look at some of our work.

Here’s the index.txt file:

```
# cat /root/ca/index.txt
V       170401090859Z           1234    unknown /C=NL/ST=North-Brabant/O=Networklessons/CN=some_server.networklessons.local/emailAddress=admin@networklessons.local
```

Above you can see the certificate that we created for our web server. It also shows the serial number that I stored in the serial file. The next certificate that we sign will get another number:

```
# cat /root/ca/serial
1235
```

Let’s take a closer look at the certificates. We can verify them with OpenSSL, but it might be nice to see them on your computer. I’ll use a Windows computer for this.

Windows doesn’t recognize the .PEM file extension so you might want to rename your certificates to .CRT.

Here’s the root certificate:



![OpenSSL Root Certificate](https://cdn.networklessons.com/wp-content/uploads/2016/04/openssl-root-certificate.png)



Above you can see the name of our root CA and the validity (10 years). If we want to trust certificates that are signed by our root CA, then we’ll have to install this certificate. Here’s how:



![OpenSSL install root certificate](https://cdn.networklessons.com/wp-content/uploads/2016/04/openssl-install-root-certificate.png)



Hit the Install Certificate button and you will see this wizard:



![openssl user or machine](https://cdn.networklessons.com/wp-content/uploads/2016/04/openssl-user-or-machine.png)



It’s up to you if you want to install it for your current user or the entire computer. Click Next to continue:



![openssl trusted root certificate store](https://cdn.networklessons.com/wp-content/uploads/2016/04/openssl-trusted-root-certificate-store.png)



Make sure you select the **Trusted Root Certification Authorities** store and click Next and Finish:



![openssl finish install root certificate](https://cdn.networklessons.com/wp-content/uploads/2016/04/openssl-finish-install-root-certificate.png)



Windows will give you one more big security warning, click Yes to continue:



![openssl root certificate security warning](https://cdn.networklessons.com/wp-content/uploads/2016/04/openssl-root-certificate-security-warning.png)



The root certificate is now installed and trusted. Now open the certificate that we assigned to “some server”:



![openssl server certificate trusted](https://cdn.networklessons.com/wp-content/uploads/2016/04/openssl_server_certificate_trusted.png)



Above you can see that it was issued by our root CA, it’s valid for one year. When you look at the certification path then you can see that Windows trusts the certificate:



![openssl server certification path](https://cdn.networklessons.com/wp-content/uploads/2016/04/openssl-server-certification-path.png)



This is looking good. If a web server would present this certificate to your computer, then it will trust it from now on.

 

## Conclusion

You have now learned how to build your own CA using OpenSSL and are ready to sign certificates for your servers, routers, firewalls, clients or any other devices that you have.

I hope you enjoyed this lesson. If you have any questions feel free to ask in our forum.
