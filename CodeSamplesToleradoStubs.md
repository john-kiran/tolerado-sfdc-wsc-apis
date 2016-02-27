# Introduction #
ToleradoStubs are meant to provide transparent fault tolerance, caching and house keeping of Saelsforce Apache Axis Stubs. This post shows some code samples about ToleradoStub for Salesforce partner, apex and metadata WSDLs

# Tolerado Stubs using Session Id and Server URL ONLY ! #
```
// Creating credentials using SessionId/ServerUrl
// New helper method Credential:createFromSessionToken 
Credential cred = Credential.createFromSessionToken("SessionId", "ServerUrl");
// Create stub using sessionid and serverurl only
// no credential given here
ToleradoPartnerStub stub = new ToleradoPartnerStub(cred);
QueryResult qr = stub.query("Select Name from Contact LIMIT 1");
SObject[] records = qr.getRecords();
Assert.assertEquals(1, records.length); 
```

# Partner WSDL – ToleradoStub Code sample #

## Querying Records - Sample ##
```
// All the hassle of doing login and setting headers encapsulated in this single call
// ToleradoPartnerStub is a ready to use stub, with no settings/configuration required
ToleradoPartnerStub pStub = new ToleradoPartnerStub(new Credential("userName", "password"));
// Binding created transparently from the given salesforce user name password.
// You transparently got the
// 1. Fault recovery mechanism
// 2. Cached stub (if its second call via the same login)
// 3. QueryResult is same class as before, so no change on your rest of
// the logic.
QueryResult qr = pStub.query("select FirstName, LastName from Contact");
// ...
// process the results within QueryResult
// ...
```


## Creating SObjects - Sample ##
```

ToleradoPartnerStub partnerStub = new ToleradoPartnerStub(new Credential("userName", "password"));

// ToleradoSobject used for avoiding hassles with Messageelements
ToleradoSobject sobj = new ToleradoSobject("Contact");
sobj.setAttribute("FirstName", "Abhinav");
sobj.setAttribute("LastName", "Gupta");

// Get the updated Sobject
SObject updatedSObject = sobj.getUpdatedSObject();
SObject[] sObjects = new SObject[] { updatedSObject };

// This call transparently gets fault recovery 
// SaveResult is same standard Apache Axis 1.4 class.
SaveResult[] saveResults = partnerStub.create(sObjects);

```

## Delete Sobject - Sample ##
```

static DeleteResult[] deleteSobjects(final String[] idsToDelete) {
    // Create a partner stub by giving your user name and password
    ToleradoPartnerStub partnerStub = new ToleradoPartnerStub(new Credential("userName", "password"));
    // Implemented the delete method on the fly for demo
    WSRecoverableMethod<DeleteResult[], ToleradoStub> deleteMethod = new WSRecoverableMethod<DeleteResult[], ToleradoStub>(
            "delete") {
        @Override
        protected DeleteResult[] invokeActual(ToleradoStub stub)
                throws Exception {
            return stub.getPartnerBinding().delete(idsToDelete);
        }
    };

    DeleteResult[] results = deleteMethod.invoke(partnerStub);
    return results;
}

```

# Apex WSDL – ToleradoApexStub Code sample #

## Run All Tests - Sample ##
```
// You saved all the hassle of creating Soap Stubs for partner login and then setting up
// headers for Apex Stub. Even Apex stub requires some url replacement to work also, that is done transparently
ToleradoApexStub stub = new ToleradoApexStub(new Credential("userName", "password"));
// Tolerado stub will prepare RunTestsRequest correctly
RunTestsResult runResult = aStub.runAllTests();
System.out.println("All Test Failures : " + runResult.getNumFailures());
```

# Metadata WSDL- ToleradoMetaStub Code Sample #

## Retrieve Request - Sample ##
```
// You saved all the hassle of creating Soap Stubs for partner login and then setting up
// headers for Metadata Stub. 
ToleradoMetaStub mStub = new ToleradoMetaStub(cred);
// Create the RetrieveRequest 
RetrieveRequest retrieveRequest = new RetrieveRequest();
retrieveRequest.setApiVersion(18.0);
// Put your package name here
retrieveRequest.setPackageNames(new String[] { "xyz" });
retrieveRequest.setSinglePackage(true);
// This call got the fault recovery and caching transparently.
// AsyncResult is the same Apache Axis class you all know
AsyncResult results = mStub.retrieve(retrieveRequest);

```