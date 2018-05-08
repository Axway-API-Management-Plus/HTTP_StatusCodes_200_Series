# HTTP_StatusCodes_200_Series
WIP Project to add HTTP Status Code policy samples and insight on their use.
Learn more at: http://danielwille.blogspot.com/2018/04/rest-api-best-practices-http-status.html

## Status

***8 May, 2018:***
- Current policy set is complete for HTTP 200s.
- Tester policy works for 200s.

HTTP 200 Best Practices: http://danielwille.blogspot.com/2018/04/rest-api-best-practices-http-status_9.html


## Descrpition
This project is an artefact supporting a blog series on REST API Best Practices around HTTP Status Codes. HTTP Status codes are a very powerful tool when building APIs that are often very under utilized. With this project and the associated blog series, we hope to give you insights on how to better utilize HTTP Status codes to enhance your APIs and provide samples to jump start your journey to implementation. General insights into the policies will be included here, but the associated blog entries will really delve into how to use the different codes and what value they bring to an API.

All the policies built on the API Gateway will follow the same general flow:
- Policy Specific Logic: This may vary depending on what the policy hopes to achieve.
- Set Message Filter: This will define the content type and set the value of ${content.body} with the HTTP Response Status message we return. 
- Reflect Message: This will set the HTTP Status Code and return ${content.body} as the HTTP Response.

Also included is a Status Code Test policy. This policy is exposed by default at '/api/status_code/v1/test' and operates based on two query parameters, code and format. Code refers to the HTTP status code you wish to test and format refers to the content type you wish to return. For example, a sample request sent to 'http://192.168.145.130:8080/api/status_code/v1/test?code=201&format=xml' will return a 201 status code in XML format.

## Status Code 200s

- ***200 - OK:*** Success, everything worked out. In the sample, we set the response content to 'OK' to demonstrate a simple response message, as you might provide for a health check resource. This could be updated to return a service response from a GET request for example.
- ***201 - Created:*** Success, everything worked out and we created new resource successfully. This status code will be used generally with a POST to show that your attempt to create a new object was successful. Additional logic is added to this policy to also return a new  header (in this example called Location) to show where the new object was created and relay its new resource ID. While a simple success is good, if the client wishes to use the new object or verify the values in its creation, it needs to know the new objects ID/location. The new header value gives a means to immediately ascertain this value (no tedious searching) for programatic use. Furthermore, in this example policy, the new location is also returned in the response message so a user could simply click the link to follow it to the new resource, embracing hyper media for ease of use. This policy provides a placeholder for logic to collect the new resource's ID, creates the value for its location, and sets it to the attribute ${location} for use as a new header and in the response message. The location attribute URL likely should be updated to point at the API you would use to view the new object, but in this example pulls attributes from the request to dynamically build a location as an example.
- ***202 - Accepted:*** Request has been accepted successfully, and queued for asynchronous operations. No payload returned.
- ***204 - No Content:*** This is used to express that your request was accepted and processed, and the server is intentionally not sending a response. For example, there would be nothing to return from processing a DELETE request on an element. It no longer has any content and the location has been removed. A 204 could be returned to acknowledge the deletion. The policy example only reflects the 204 status code to the client, as no payload is sent with a 204 by definition. You may wish to instead return a 200 with a link to where the resource previously resided or a payload with a deletion acknowledgment.
- ***206 - Partial Content:*** This would be used with pagination. It is not covered in this policy set at this time as pagination methods can vary greatly depending on your enterprise and your data.
