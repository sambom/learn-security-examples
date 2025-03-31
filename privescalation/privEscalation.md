# Privilege Escalation

The example demonstrates a privilege escalation vulnerability and how to exploit it.

## Steps to reproduce

1. Install all dependencies

    `$ npm install`

2. Start the **insecure.ts** server

    `$ npx ts-node insecure.ts`

3. In the browser, send a GET request

    ```
        http://localhost:3000/send-form
    ```

4. Try different UserIds and see which one gives you authorized access to change the role of that user.

## For you to do

Answer the following:

1. **Briefly explain the potential vulnerabilities in `insecure.ts`.**  
   `insecure.ts` does not properly validate or authenticate users and naively trusts user input. This allows an attacker to impersonate another user by simply providing a different user ID. If an attacker obtains the ID of a higher-privileged user (e.g., an admin), they can perform unauthorized actions as that user.

2. **Briefly explain how a malicious attacker can exploit them.**  
   An attacker can attempt to guess or iterate through user IDs to find an admin's ID. Once discovered, they can send a request using that ID to impersonate the admin. This could allow them to perform sensitive operations, such as altering the admin's privileges—e.g., downgrading their role to a regular user.

3. **Briefly explain the defensive techniques used in `secure.ts` to prevent the privilege escalation vulnerability.**  
   `secure.ts` mitigates this vulnerability by using session-based authentication and authorization. It validates that a valid session ID is included with each request and retrieves the user's role based on the session. Only users with an active session and proper admin privileges are allowed to perform sensitive actions. If the session is invalid or the user is not an admin, the request is denied.
