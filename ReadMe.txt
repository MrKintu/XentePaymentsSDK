This is a guide on how to use the XentePayments Java SDK.

- Before getting started, add these dependencies to your pom.xml file:
i) Add this to access JSON Files
    <dependency>
        <groupId>org.json</groupId>
        <artifactId>json</artifactId>
        <version>20190722</version>
    </dependency>

ii) Add this to access the Xente Payments SDK
    <dependency>
        <groupId>org.bitbucket.dkintu</groupId>
        <artifactId>XentePaymentsSDK</artifactId>
        <version>1.0.1-SNAPSHOTSDK</version>
    </dependency>

- The SDK is divided into two main parts: the Components and the Services.
- The SDK user shall mainly interact with the Components package and the Services package shall run in the background.
- The XentePayments package takes in one JSON files as a parameter for the SDK to work: Credentials JSON File.
- The Credentials JSON has information about the integrator's XentePayments log in credentials.

- The Credentials JSON looks as such:
{
    "apiKey": "6A19EA2A706041A599375CC95FF08809",
    "password": "Demo123456",
    "mode": "sandbox" or "production"
}
Please note that the "mode" can ONLY be EITHER "sandbox" OR "production" but never both.

- The Services package contains the logic to generate bearer tokens (that grant authority to the XentePayments API) in the
    TokenHandler class.
- The Services package also contains logic to manipulate the data in the JSON files in the ObjectHandler class.
    It it highly important that the integrator passes the JSON files correctly to the XentePayments instance.
- The Services package also contains logic to handle the assignment of the various URLs to be called within the
    URLConstants class.
- The Services package has the logic to perform all GET requests from the XentePayments API within the GETRequestClient class.
- The Services package has the logic to perform all POST requests to the XentePayments API within the POSTRequestClient class.

> Invoke an instance of the XentePayments package as follows:
    XentePayments xente = new XentePayments(credentials);

- From this the integrator can now use the object to perform various methods within the Components package through
    accessing the various component objects already created in the class constructor. Below is how it can be done:

> The integrator can retrieve their account information by using their account ID as shown below:
    String accountID = "256784378515";
    xente.accountsHandler.getAccountByID(accountID);

> The integrator can also check to see which payment providers are available to use on the XentePayments platform. Call
    this method:
    xente.paymentProviders.getPaymentProviders();

> The integrator can also create a new transaction. To do this the integrator needs to pass a JSON File which contains
    information about the transaction that the integrator would like the Xente Payments API to process.
- The Transaction JSON looks as such:
{
    "paymentProvider": "MTNMOBILEMONEYUG",
    "amount": "800",
    "message": "Demo Request",
    "customerId": "256778418592",
    "customerPhone": "256778418592",
    "customerEmail": "d-kintu@outlook.com",
    "customerReference": "256778418592",
    "batchId": "Batch001",
    "requestId": "DemoRequest337",
    "metadata": "More information about transaction here"
}
- This is the method called to create a new transaction:
    xente.transactionsHandler.createTransaction(transaction);

> The integrator can also search for a specific transaction using the transaction ID:
    String transactionID = "631034D3F96C441085FA7D010ACB7194-256784378515";
    xente.transactionsHandler.getTransactionByID(transactionID);

> Lastly the integrator can search for a transaction using the Request ID:
    String requestID = "0.9351612896255068";
    xente.transactionsHandler.getRequestByID(requestID);