# Snowflake Durable Connector for Azure Data Factory (ADF)
This connector is an fork of work authored by Jeremiah Hansen: [snowflake-connector-adf](https://github.com/jeremiahhansen/snowflake-connector-adf)


This purpose of this fork is to address the maximum response time limitation in HTTP triggered function Documeneted [Here](https://docs.microsoft.com/en-us/azure/azure-functions/functions-scale)

> Regardless of the function app timeout setting, **230 seconds** is the maximum amount of time that an HTTP triggered function can take to respond to a request. This is because of the default idle timeout of Azure Load Balancer. For longer processing times, consider using the Durable Functions async pattern or defer the actual work and return an immediate response.

The code is an Azure Function which allows Azure Data Factory (ADF) to connect to Snowflake in a flexible way. It provides SQL-based stored procedure functionality with dyamic parameters and return values. Used with ADF you can build complete end-to-end data warehouse solutions for Snowflake while following Microsoft and Azure best practices around portability and security.

![Connector Sequence Diagram](Docs/SnowflakeSequence.png?raw=true "Connector Sequence Diagram")