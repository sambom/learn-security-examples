# Tampering

This example demonstrates information disclosure by injecting malicious query objects to a NoSQL database.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Insert test data in the MongoDB database. Make sure the mongod is up and running by typing the `mongosh` command in the termainal. If mongod process is up then you will see that the connection was successful. Command to insert test data:

    `$ npx ts-node insert-test-users.ts`

This will create a database in MongoDB called __infodisclosure__. Verify its presence by connecting with mongosh and running the command `show dbs;`.

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, pretend to be a hacker and type a malicious request

    ```
        http://localhost:3000/userinfo?username[$ne]=
    ```

4. Do you see user information being displayed despite the malicious request not having a valid username in the request?

## For you to do

Answer the following:

1. **Briefly explain the potential vulnerabilities in `insecure.ts`.**  
   When retrieving user information, the `username` field in the query accepts any input without validation. Since users are stored in a database and a query is made based on this input, it opens the door to injection attacks—specifically NoSQL injection—where malicious input could manipulate the database query.

2. **Briefly explain how a malicious attacker can exploit them.**  
   An attacker can exploit this by injecting a special query operator like `$ne` (not equal) as the input. For example, if the attacker inputs `{ "$ne": "" }`, MongoDB will interpret this as a request to find all users whose usernames are not equal to an empty string. This results in unauthorized access to user data, effectively leaking information that should be restricted.

3. **Briefly explain the defensive techniques used in `secure.ts` to prevent the information disclosure vulnerability.**  
   `secure.ts` addresses this vulnerability by first verifying that the provided `username` is a string. It then sanitizes the input by stripping out all non-alphanumeric characters. This ensures that only clean, valid usernames are used in the query, preventing injection of special MongoDB operators or other potentially dangerous input.
