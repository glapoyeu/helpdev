# Accept server's self-signed ssl certificate in Java client

## Problem
It looks like a standard question, but I couldn't find clear directions anywhere.

I have java code trying to connect to a server with probably self-signed (or expired) certificate. The code reports the following error :

```
[HttpMethodDirector] I/O exception (javax.net.ssl.SSLHandshakeException) caught 
when processing request: sun.security.validator.ValidatorException: PKIX path 
building failed: sun.security.provider.certpath.SunCertPathBuilderException: 
unable to find valid certification path to requested target
```

As I understand it, I have to use keytool and tell java that it's OK to allow this connection.

All instructions to fix this problem assume I'm fully proficient with keytool, such as

    generate private key for server and import it into keystore

Is there anybody who could post detailed instructions?

I'm running unix, so bash script would be best.

Not sure if it's important, but code executed in jboss.

## Solution
You have basically two options here: add the self-signed certificate to your JVM truststore or configure your client to 

### Option 1

Export the certificate from your browser and import it in your JVM truststore (to establish a chain of trust):

```
<JAVA_HOME>\bin\keytool -import -v -trustcacerts
-alias server-alias -file server.cer
-keystore cacerts.jks -keypass changeit
-storepass changeit 
```

### Option 2
Disable Certificate Validation:
```
/ Create a trust manager that does not validate certificate chains
TrustManager[] trustAllCerts = new TrustManager[] { 
    new X509TrustManager() {     
        public java.security.cert.X509Certificate[] getAcceptedIssuers() { 
            return new X509Certificate[0];
        } 
        public void checkClientTrusted( 
            java.security.cert.X509Certificate[] certs, String authType) {
            } 
        public void checkServerTrusted( 
            java.security.cert.X509Certificate[] certs, String authType) {
        }
    } 
}; 

// Install the all-trusting trust manager
try {
    SSLContext sc = SSLContext.getInstance("SSL"); 
    sc.init(null, trustAllCerts, new java.security.SecureRandom()); 
    HttpsURLConnection.setDefaultSSLSocketFactory(sc.getSocketFactory());
} catch (GeneralSecurityException e) {
} 
// Now you can access an https URL without having the certificate in the truststore
try { 
    URL url = new URL("https://hostname/index.html"); 
} catch (MalformedURLException e) {
} 
```

Note that **I do not recommend the Option #2 at all**. Disabling the trust manager defeats some parts of SSL and makes you vulnerable to man in the middle attacks. Prefer Option #1 or, even better, have the server use a "real" certificate signed by a well known CA.

# Create link symbolic
sudo ln -s /opt/jdk1.8/bin/java /usr/bin/java
java --version
java -version
eclipse
sudo ln -s /usr/lib/jvm/default-runtime/bin/java /usr/bin/java22 