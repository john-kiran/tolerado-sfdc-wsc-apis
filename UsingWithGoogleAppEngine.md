How to use Tolerado with Google App Engine

# Setting up Classpath #

As suggested in [WSC Getting Started Guide](http://code.google.com/p/sfdc-wsc/wiki/WscFromGae). Download([from here](http://code.google.com/p/sfdc-wsc/downloads/list)) and copy these files to the /war/WEB-INF/lib/ directory of the GAE war file.
  * wsc-XX.jar
  * wsc-gae.jar

### Please NOTE ###
> You don't need to download partner-XX.jar as suggested in WSC guide. Rather, use the sfdc-wsc-xx-partner.jar available in Tolerado download are [here(sfdc-wsc-20-wsdl2java.zip)](http://code.google.com/p/tolerado-sfdc-wsc-apis/downloads/list). This is because
  * partner-xx.jar is not updated since April 2010
  * Tolerado's partner jar is complied for addressing ClassCast issues faced when used with Axis. See this issue on WSC forum : http://code.google.com/p/sfdc-wsc/issues/detail?id=15

After this just set your environment, as per tolerado extensions(metadata, apex, enterprise) required, this is explained in detail [here](http://code.google.com/p/tolerado-sfdc-wsc-apis/wiki/GettingStartedGuide).

# Tell Tolerado to use correct Http Transport #
I hate config files, so I just opened a simple static call to let Tolerado understand the transport to be used. Just make sure you execute this code at least once, from some startup class or before instantiating Tolerado stubs.

```
// I prefer static blocks to do such stuff, as they don't require to be called explicitly.
// Just add this static block to some startup servlet or context listener.
static {
  ToleradoStub.WSC_HTTP_TRANSPORT = GaeHttpTransport.class;
}
```

This code snippet shows, how to do this via Context Listener
Listener Java Class
```
public class AppContextListener implements ServletContextListener {
  static {
    ToleradoStub.WSC_HTTP_TRANSPORT = GaeHttpTransport.class;
  }
  
  @Override
  public void contextDestroyed(ServletContextEvent arg0) {

  }

  @Override
  public void contextInitialized(ServletContextEvent arg0) {
  }

}
```

Deployment descriptor (web.xml)

```
  <listener>
    <listener-class>com.tgerm.rest.AppContextListener</listener-class>
  </listener>

```