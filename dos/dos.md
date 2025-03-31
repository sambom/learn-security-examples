# Denial-of-Service (DoS)

This example demonstrates DoS vulnerabilities and how they can be exploited.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Ignore if you have already done this once. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?id[$ne]=
    ```

4. Do you see the server crashing?

## For you to do

Answer the following:

1. **Briefly explain the potential vulnerabilities in `insecure.ts` that can lead to a DoS attack.**  
   In `insecure.ts`, there is no exception handling when attempting to retrieve a user from the database. If the query fails, the error is unhandled, which can cause the server to crash. This lack of error handling makes the application vulnerable to Denial-of-Service (DoS) attacks.

2. **Briefly explain how a malicious attacker can exploit them.**  
   Without input validation, an attacker can send requests that deliberately cause the database query to fail. For example, supplying an invalid ID (e.g., an empty string) will result in an error like: _"Argument passed in must be a 24 character hex string, 12 byte Uint8Array, or an integer."_ Since this error isn't caught, it can crash the server. Repeating such requests rapidly can overload and bring down the application.

3. **Briefly explain the defensive techniques used in `secure.ts` to prevent the DoS vulnerability.**  
   `secure.ts` includes a `try-catch` block around the database query to handle any errors and prevent the server from crashing. If an error occurs, a user-friendly message is returned instead. Additionally, a rate limiter is applied to the `/userinfo` route, allowing only one request per IP every 5 seconds. This rate limiting reduces the risk of the server being overwhelmed by repeated malicious requests.
