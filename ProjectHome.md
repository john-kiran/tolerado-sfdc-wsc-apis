# What is **Tolerado** ? #
Project "tolerado" is a Java based WS client framework for better and fault tolerant use of Web Service APIs given by Salesforce. Tolerado works on top of highly performance **[SFDC Web Service Connector API](http://code.google.com/p/sfdc-wsc/)**. In case your project is depending on Apache axis 1.4, you can use [Tolerado for Axis](http://code.google.com/p/tolerado-salesforce-web-services-client-wrappers/)

On top of benefits exposed by WSC API, Tolerado gives

  * A RECOVERABLE framework for each web service method call. This recoverable framework will transparently RETRY all the recoverable web service/remote errors/exceptions rather failing fast.
  * A transparent caching mechanism for salesforce session id/urls, so that we save a login web service call before other web service operations.
  * Giving utility and wrapper API's for easing and making development effort less error prone with existing WS client stubs. For ex.
    * Easing the CRUD of new Sobjects via wrappers, if you are using Partner WSDL's Sobject.
    * Giving easy stubs, so that developers are not into hassle of setting right session headers, server urls etc for using either Metadata, Apex or Partner WSDL APIs.
  * Keeping the migration effort to minimal. So classes are designed to fit well with existing Axis generated classes for ex. QueryResult, RunTestsResult etc.


# Why use/migrate to Tolerado ? #
  * Your Apache Axis web service client code will be too simplified, more fault tolerant/recoverable and readable too.
    * This [post](http://www.tgerm.com/2010/07/salesforce-java-code-fixes.html) gives **detailed code samples and shows how code will be better and stable at production** ?
    * This [post](http://www.tgerm.com/2010/06/retrying-web-service-exceptions.html) explains why recovering from web service exceptions is important.

  * Apex/Metadata WSDL users will get rid of issues and complexities involved in creating correct MetadataBindingStub/ApexBindingStub.
    * If it tooks 15-20 lines of code previously to get correct Metadata binding, now it will take just 1 line.
    * What are the other issues, explained in this [post](http://www.tgerm.com/2010/05/apex-no-operation-available-request.html)

  * Simplified CRUD operations on Sobjects.
    * Easily set/get fields.
    * Direct methods to access lookup and child relationships.

  * queryMore() partner calls will be simplified too much via ToleradoQuery.
    * Now you can make queryMore() calls without query cursors handling and all the other house keeping in Soap Stubs.
    * ToleradoQuery offers you an easy Java style interface to do queryMore calls correctly with desired batch size.

  * Compatible with Google App Engine. [More details](http://code.google.com/p/tolerado-sfdc-wsc-apis/wiki/UsingWithGoogleAppEngine)

# References - Useful Pointers #
All of them are listed in the right side bar under labels, "Featured wiki pages" and "External Links". Still here is a useful list for important references

  * [Getting Started Guide](http://code.google.com/p/tolerado-sfdc-wsc-apis/wiki/GettingStartedGuide)
  * [Code Samples](http://code.google.com/p/tolerado-sfdc-wsc-apis/wiki/CodeSamples)
  * [Tolerado Dependencies](http://code.google.com/p/tolerado-sfdc-wsc-apis/wiki/ToleradoDependencies)
  * [How to add new web service method powered by Tolerado](http://code.google.com/p/tolerado-sfdc-wsc-apis/wiki/HowToAddNewToleradoWSMethod)
  * [WSC SObject handling Lookup, Child relationships and XPath queries !](http://www.tgerm.com/2010/08/wsc-sobject-lookup-childrelationships.html)
  * [ApacheAxis & WSC Partner WSDL Sobject compared !](http://www.tgerm.com/2010/08/salesforce-apacheaxis-wsc-sobject.html)
  * [Salesforce WSC ApacheAxis Session Sharing](http://www.tgerm.com/2010/08/salesforce-wsc-apacheaxis-session.html)
  * [WSC Apache Axis ClassCastException Issue Fixed !](http://www.tgerm.com/2010/08/wsc-apache-axis-classcastexception.html)



# Special Thanks #
  * [Jeff douglas](http://blog.jeffdouglas.com/) : for helping me with all WSC related docs and artifacts. This really helped me speeding up on WSC.
  * My family, for sparing me to work on this project.
