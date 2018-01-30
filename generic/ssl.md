### ssl sertificates form tree like structure

```
verisign <- let's encrypt <- your certificate
```

This is also called SSL certificate chain (looking from leafs to root) 

Sometimes JVM's keystore gets out of date, and cannot trace a given certificate to a trusted authority.

To fix this we need to tell JVM that all certificates in that chain are trusted, by importing all certificates.

1) Using curl or a browser, export all certificates in the chain to your local computer as `.crt` (X.509 PEM) files

2) In your `$JAVA_HOME` (run `/usr/libexec/java_home` to find it), navigate to `/jre/lib/security` 

3) `sudo keytool -importcert -file /path/to/cert/DSTRootCAX3.crt -keystore cacerts`
