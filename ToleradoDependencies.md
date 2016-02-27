This page explains what are the other 3rd party lib dependencies for Tolerado

# Tolerado Dependencies #

Tolerado dependencies are broadly classified into to areas

  1. WSC + WSDL Comilation Dependencies
  1. Other Dependencies


# WSC + WSDL Comilation Dependencies #

These dependencies include classes generated via WSC compilation. We already did WSDL compilation on partner, apex and metadata WSDLs, so ready to use JARS for each partner, apex and metadata are available at the [downloads page](http://code.google.com/p/tolerado-sfdc-wsc-apis/downloads/list). Here is the list
  * sfdc-wsc-xx-apex.jar
  * sfdc-wsc-xx-metadata.jar
  * sfdc-wsc-xx-partner.jar
  * sfdc-wsc-xx-enterprise.jar

Here xx corresponds to SFDC API version from which the jar was compiled.

You will need to do WSC compilation yourself if you are planning to use enterprise WSDL APIs. This requires special care while compilation, how is explained [here ](http://code.google.com/p/tolerado-sfdc-wsc-apis/wiki/GettingStartedGuide#WSDL2Java_with_WSC)


# Other Dependencies #

Tolerado code depends directly indirectly on following libraries. I said indirectly because many of these are Apache Axis dependencies. Here is the list

| **Library Name** | **Jar File** | **Remarks** |
|:-----------------|:-------------|:------------|
| wsc-19.jar       | WSC Jar file |             |
| Apache commons lang | commons-lang-2.5.jar |             |
| Apache Common Logging | commons-logging-1.1.1.jar | Used by Tolerado code directly, we are leaving logging stuff open so that you can plugin you logger with Tolerado. |
| Log4j            | log4j-1.2.16.jar | **OPTIONAL** include, if you are using log4j with commons logging. Otherwise feel free to remove this. |

### NOTE ###
Listed are the minimal library versions, you guys can try with latest ones. Hope they should work well.