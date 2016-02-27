This post explains how to add new Web Service methods from partner, apex and metadata WSDLs that use the Tolerado fixture for error recovery and all other benefits.

# Introduction #

There are many web service methods in each partner, metadata and apex wsdls. So Tolerado haven’t created a method wrapper for each in ToleradoPartnerStub, ToleradoMetaStub and ToleradoApexStub. This is intentionally left because inheriting the power of fixture is too simple.

At very basic to implement a new web service method using Tolerado you have to extend [WSRecoverableMethod<R, S extends ToleradoStub>](http://tgerm-docs.appspot.com/wsdocs/com/tgerm/tolerado/axis14/core/method/WSRecoverableMethod.html), its an abstract class and expects you to provide implementation of following method. This is explained with examples below.

# New WS Method and WSRecoverableMethod<R, S extends ToleradoStub> #

WSRecoverableMethod is an Abstract web service method template, extending this class and implementing **protected abstract R invokeActual(S stub) throws Exception;** makes your implementation recoverable and stub caching.

## Template of how to extend WSRecoverableMethod ##

```
// Create a stub from Cached registry for the desired username/pass
ToleradoPartnerStub stub = new ToleradoPartnerStub(new Credential("userName", "password"));

// Create an Java Anonymous class, implementing the WSRecoverableMethod 
// First Generics Type Argument : "R" here is the result of actual web service call from salesforce apache axis stubs.
// Second Generics Type Argument : "ToleradoStub" here is the Tolerado stub, that can be either of ToleradoPartnerStub, ToleradoApexStub or ToleradoMetaStub.
WSRecoverableMethod<R, ToleradoPartnerStub> wsMethod = new WSRecoverableMethod<R, ToleradoPartnerStub>("methodName") {
    // This method is the one where the actual Web Service call should be done via
    // Apache axis stubs. 
    @Override
    protected R invokeActual(ToleradoPartnerStub stub) throws Exception {
        // use stub to do the actual call here
        // one shouldn't try recovering or do any special exception handling
        // here, as Tolerado fixture will do that transparently
        // Return the web service call result "R"
        return R;
    }
};
// This is must to actually make the web service call. Calling "invoke" method below will trigger the Tolerado recovery and will do the "invokeActual" call transparently.
// Note again "R" should be changed to the actual web service result.
R r = wsMethod.invoke(stub);

```


## Apex - RunTests Call -- Sample WSRecoverableMethod Impl ##
Code snippet below shows how to implement Query WS method. Don't think its big, 90% of the code is comments :)

```
static RunTestsResult runSelectiveTests(final String[] classes,
        final String namespace, final String[] packages) {
    // Stub for Apex WSDL, fetched from cached Registry
    ToleradoApexStub stub = new ToleradoApexStub(new Credential("userName", "password"));
    WSRecoverableMethod<RunTestsResult, ToleradoApexStub> wsRecoverableMethod = null;
    // An Java Anonymous class, implementing the WSRecoverableMethod created
    // First Generics Type Argument is the result of actual web service call
    // i.e. RunTestsResult
    // Second Generics Type Argument : "ToleradoApexStub" here is the
    // Tolerado stub for Apex WSDL
    // Note method name provided as "runTests" to the constructor
    wsRecoverableMethod = new WSRecoverableMethod<RunTestsResult, ToleradoApexStub>(
            "runTests") {
        // Implementation done for the invokeActual, this is only required
        // stuff. Note the return type of method should match the declared
        // Generics type argument 1 above and the Stub argument should match
        // similarly to the second argument
        @Override
        protected RunTestsResult invokeActual(ToleradoApexStub stub)
                throws Exception {
            boolean allTests = false;
            RunTestsRequest runTestsRequest = new RunTestsRequest(allTests,
                    classes, namespace, packages);
            // Actual web service call done using this actual Apache Axis
            // stubs
            ApexBindingStub apexBinding = stub.getApexBinding();
            // The actual web service method call
            return apexBinding.runTests(runTestsRequest);
            // Note no exception handling done here, as Tolerado will
            // transparently take care of that
        }
    };
    return wsRecoverableMethod.invoke(stub);
}

```

## Partner - Query Call -- Sample WSRecoverableMethod Impl ##
This code snippet shows how to make a Partner query call. Have added no comments to show the simplicity of code now :)

```

public QueryResult query(final String soql) {
    ToleradoPartnerStub stub = new ToleradoPartnerStub(new Credential("userName", "password"));   
    WSRecoverableMethod<QueryResult, ToleradoPartnerStub> wsMethod = new WSRecoverableMethod<QueryResult, ToleradoPartnerStub>(
            "Query") {
        @Override
        protected QueryResult invokeActual(ToleradoPartnerStub stub)
                throws Exception {
            QueryResult query = stub.getPartnerBinding().query(soql);
            return query;
        }
    };
    return wsMethod.invoke(stub);
}
```

## Metadata - Retrieve Call -- Sample WSRecoverableMethod Impl ##
This code snippet shows how to make a Retrieve call from metadata wsdl. Again Have added no comments for simplicity  :)

```
public AsyncResult retrieve(final RetrieveRequest retrieveRequest)
        throws Throwable {
    ToleradoMetaStub stub = new ToleradoMetaStub(new Credential("userName", "password"));   
    WSRecoverableMethod<AsyncResult, ToleradoMetaStub> wsMethod = new WSRecoverableMethod<AsyncResult, ToleradoMetaStub>(
            "retrieve") {
        @Override
        protected AsyncResult invokeActual(ToleradoMetaStub stub)
                throws Exception {
            return stub.getMetaBinding().retrieve(retrieveRequest);
        }
    };
    AsyncResult results = wsMethod.invoke(stub);
    return results;
}
```