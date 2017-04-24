# Add SSL

It is very easy to add SSL to a spring boot project.
First, we need to **obtain a SSL certificate**. We can either buy one (or use letsencrypt), or generate one for development.
All JDK installations come with a little tool, called keytool. We can use it to generate self-signed certificate that is good for development only.
To do that, we need to add keytool to the path variable and then run the following command:

    keytool -genkey -alias boot_dev_ssl -storetype PKCS12 -keyalg RSA -keysize 2048 -keystore keystore.p12 -validity 3650

replacing the `boot_dev_ssl` with your preferred alias.

You will then be asked a series of questions, please remember the password you use.


**Configure spring boot to use new certificate**.
Move the certificate to the project folder, and add the following lines to spring boot's application.properties file.


    server.port: 8443
    server.ssl.key-store: keystore.p12
    server.ssl.key-store-password: abc123
    server.ssl.keyStoreType: PKCS12
    server.ssl.keyAlias: boot_dev_ssl

Now restart the server and remember to use https and port 8443.

Optionally you can redirect http to https. To javaconfig class, add the following code. It very self-explanatory:

    @Bean
    public EmbeddedServletContainerFactory servletContainer() {
    TomcatEmbeddedServletContainerFactory tomcat = new        TomcatEmbeddedServletContainerFactory() {
        @Override
        protected void postProcessContext(Context context) {
         SecurityConstraint securityConstraint = new SecurityConstraint();
         securityConstraint.setUserConstraint("CONFIDENTIAL");
         SecurityCollection collection = new SecurityCollection();
         collection.addPattern("/*");
         securityConstraint.addCollection(collection);
         context.addConstraint(securityConstraint);
        }
      };

    tomcat.addAdditionalTomcatConnectors(initiateHttpConnector());
    return tomcat;
    }

    private Connector initiateHttpConnector() {
     Connector connector = new Connector("org.apache.coyote.http11.Http11NioProtocol");
     connector.setScheme("http");
     connector.setPort(8080);
     connector.setSecure(false);
     connector.setRedirectPort(8443);
     return connector;
    }
