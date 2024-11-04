# Unrestricted Resource Consumption  

>[API4:2023 Unrestricted Resource Consumption](https://university.apisec.ai/products/owasp-api-security-top-10-and-beyond/categories/2152492155/posts/2166898594)  

>Prevent excessive use of their API  

## OWASP Impacts Description  

* Execution timeouts
* Maximum allocable memory
* Maximum number of file descriptors
* Max number of processes
* Maximum upload file size
* Number of operations to perform in a single API client request (e.g. GraphQL batching)
* Number of records per page to return in a single request-response
* Third-party service providers' spending limit  

>Additional Resources:  

* CWE-770: Allocation of Resources Without Limits or Throttling
* CWE-400: Uncontrolled Resource Consumption
* CWE-799: Improper Control of Interaction Frequency  

----  

>Which of the following is NOT a recommended preventative measure for limiting resource consumption in APIs?  

* Allow an unlimited number of elements in request arrays.  

>Which of the following best describes the threat associated with the lack of rate limiting?

* The API allows uncontrolled interactions or resource consumption which can lead to Denial of Service (DoS) or increased financial costs.  

>Why is rate limiting important for the monetization and availability of APIs?  

* Rate limiting allows API providers to control the flow of their data.  

>In regards to API rate limiting, what is indicated by an HTTP 429 response status code.?  

* The consumer has made too many requests.  

>According to OWASP, what is the primary consequence of not implementing API rate limiting?  

* DoS due to resource starvation or a negative impact to the service provider's billing.

----  
