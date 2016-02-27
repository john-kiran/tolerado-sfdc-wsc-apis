Explains, what contributes to a Tolerado releases

# 1.1 (14 Nov 2010) #
  * Added support to use all Tolerado stubs with only sessionId and serverUrl in hand. Previously username/password was a must. ([Issue 1](http://code.google.com/p/tolerado-sfdc-wsc-apis/issues/detail?id=1))
  * Fixed cache sharing issue between enterprise and partner sessions ([Issue 2](http://code.google.com/p/tolerado-sfdc-wsc-apis/issues/detail?id=2))
  * Added support to use either Product, Developer, Sandbox, Prerelease or Custom Org Type(Enviornment) with Tolerado. Now one can feed in either the Enviornment or the hostname to use for authorizing with Salesforce.
  * Added support to specify Salesforce API version to Tolerado stubs. That means more control if you still want to stuck with previous API version.
  * Tolerado Sobject, internal changes for more flexiblity in extending and changing behavior like caching.
    * Added new method XmlObject[.md](.md) getChildrens(String relationshipName)
    * Added final getType(), setType() methods, to get/set SObjectType.
    * Deprecated setAttribute(String attribName) to match with getters like getValue(String) getTextValue(String) etc, we are promoting setValue(String, Object), This method(setAttribute) might be removed in future releases.
  * Simplified design of Tolerado stubs, to facilitate easy extensions.
  * Removed getLoginDriver() from ToleradoSession API contract.
  * ToleradoMetaStub for metadata wsdl, has new recoverable calls like
    * AsyncResult[.md](.md) checkStatus(final String[.md](.md) asyncProcessId)
    * RetrieveResult checkRetrieveStatus(final String asyncProcessId)
    * AsyncResult deploy(final byte[.md](.md) zipFile, final DeployOptions deployOptions)
    * DeployResult checkDeployStatus(final String asyncProcessId)
  * Added logout() call to both Tolerado Partner & Enterprise stubs.


# 1.0.1 (27 Oct 2010) #

  * Upgraded to latest WSC-API (wsc-20.jar)
  * Upgraded to winter'11 v20 of SFDC Web Service APIs
  * Added AllOrNone Soap Header support to Tolerado Partner & Enterprise stubs.
  * Updated Test cases to reflect the same.