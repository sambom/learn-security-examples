# Tampering

This example demonstrates tampering through script injection.

## Steps to reproduce

1. Install all dependencies

    `npm install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. In the browser, type a potentially malicious script in the name field of the form

    ```
        <script> document.body.innerHTML = "<a href='https://google.com'> Gotcha </a>"</script>
    ```

4. Do you see the potentially malicious hyperlink being injected into the form?

## For you to do

Answer the following:

1. **Briefly explain the potential vulnerabilities in `insecure.ts`.**  
   When registering a name in `insecure.ts`, the input is saved directly after applying only the `trim()` function, which removes whitespace. This allows users to store any type of text in the `session.user` field, including potentially harmful scripts.

2. **Briefly explain how a malicious attacker can exploit them.**  
   An attacker can perform a Cross-Site Scripting (XSS) attack by injecting JavaScript code into the name field. Since the input is stored without sanitization, the script becomes part of `session.user`. Whenever the user's name is rendered on the site, the malicious script is executed in the browser.

3. **Briefly explain why `secure.ts` does not have the same vulnerabilities.**  
   `secure.ts` mitigates this vulnerability by using the `escapeHTML` function, which sanitizes input by converting special HTML characters into their safe equivalents. As a result, any input is stored and displayed as plain text rather than executable code, effectively preventing XSS attacks.
