# Repudiation

The example demonstrates a vulnerability that can lead to repudiation by malicious users attempting to access the services provided by a server.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Run the server __insecure.ts__.

3. Pretend to be a malicous user and interact with the services by sending requests from the browser.

4. Do you think your actions can be repudiated?

## For you to do

1. **Briefly explain the vulnerability.**  
   When requests are made to our server, no identifying information is stored. As a result, any changes to data or access attempts cannot be traced back to their source. This lack of tracking means there is no way to verify who performed which actions, leading to a lack of non-repudiation on the server.

2. **Briefly explain why the vulnerability is addressed in `secure.ts`.**  
   The vulnerability is addressed in `secure.ts` to enable tracking of user actions throughout the website. For each request, the server logs the HTTP method and the source IP address. Additionally, whenever a user sends or receives a message successfully, the action is logged. This creates an audit trail, helping to ensure non-repudiation on the server.

3. **Which design pattern is used in the secure version to address the vulnerability? Briefly explain how it works.**  
   The **Chain of Responsibility** design pattern is used in this implementation. An Express middleware function logs each incoming request before it is processed. It captures the HTTP method and IP address, then passes the request to the next function in the chain. This ensures consistent logging without interfering with the main application logic.
