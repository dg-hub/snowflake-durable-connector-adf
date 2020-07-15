# Snowflake Durable Connector for Azure Data Factory (ADF)
This connector is an fork of work authored by Jeremiah Hansen: [snowflake-connector-adf](https://github.com/jeremiahhansen/snowflake-connector-adf)


This purpose of this fork is to address the maximum response time limitation in HTTP triggered function Documeneted [Here](https://docs.microsoft.com/en-us/azure/azure-functions/functions-scale)

> Regardless of the function app timeout setting, **230 seconds** is the maximum amount of time that an HTTP triggered function can take to respond to a request. This is because of the default idle timeout of Azure Load Balancer. For longer processing times, consider using the Durable Functions async pattern or defer the actual work and return an immediate response.

The solution takes the existing code and implements it using HTTP API pattern
[Durable Functions extension of Azure Functions](https://docs.microsoft.com/en-us/azure/azure-functions/durable/durable-functions-overview?tabs=csharp)

The code is an Azure Function which allows Azure Data Factory (ADF) to connect to Snowflake in a flexible way. It provides SQL-based stored procedure functionality with dyamic parameters and return values. Used with ADF you can build complete end-to-end data warehouse solutions for Snowflake while following Microsoft and Azure best practices around portability and security.

## HTTP Responses

The function returns from the HTTP request immediately and returns a json reponse with the status and control uris
```
{
    "id": "9d6cad94093948441ea39a417464b38f",
    "statusQueryGetUri": "http://...",
    "sendEventPostUri": "http://...",
    "terminatePostUri": "http://...,
    "purgeHistoryDeleteUri": "http://..."
}
```

Use the `statusQueryGetUri` to get the `runtimeStatus` and `output`
```
{
    "name": "SnowflakeDurableConnectorAdf_",
    "instanceId": "9d6cad94093948441ea39a417464b38f",
    "runtimeStatus": "Completed",
    "input": "......",
    "customStatus": null,
    "output": [
        "column1....",
        "column2...."
    ],
    "createdTime": "2020-07-10T03:38:58Z",
    "lastUpdatedTime": "2020-07-10T03:39:00Z"
}
```

## Data Factory 

This can be implemented in Data Factory with an Azure Function activity to call the function and a loop to pediodically poll `statusQueryGetUri` until `runtimeStatus` != `Running`

See the pipeline file: [pl_SnowflakeDurableConnectorAdf.json](AzureDataFactory/pipeline/pl_SnowflakeDurableConnectorAdf.json)

![Data Factory Pipeline](Docs/adf1.png?raw=true "Data Factory Pipeline")
![Data Factory Loop](Docs/adf2.png?raw=true "Data Factory Loop")

## Sequence Diagram
![Data Factory Loop](Docs/SnowflakeSequence.png?raw=true "Data Factory Loop")