# Spoofing

This example demonstrates spoofind through two ways -- Stealing cookies programmatically and cross site request forgery (CSRF).

## Steps to reproduce the vulnerability

1. Install dependencies

    `$ npx install`

2. Start the **insecure.ts** server

    `npx ts-node insecure.ts`

3. Start the malicious server **mal.ts**

    `npx ts-node mal.ts`

4. Open __http://localhost:8000__ in a browser, type a name and Submit.

5. Open the __Application__ tab in the Browser's inspect pane. Find the __Cookies__ under __Storage__. You should see a __connect.sid__ cookie being set.

6. Open the HTML file __mal-steal-cookie.html__ file in the same browser (different tab). Open inspect and view the console.

7. Click the link in the HTML file. Do you see the cookie being stolen in the console?

8. Open the HTML file __mal-csrf.html__ file in the same browser (different tab). What do you see if the user has not logged out of **insecure.ts**? What do you see if the user has logged out? 


## For you to answer

1. **Briefly explain the spoofing vulnerability in `insecure.ts`.**  
   The vulnerability stems from the session middleware configuration. When creating a session, the `httpOnly` flag for the session cookie is set to `false`, allowing client-side JavaScript to access the cookie. This opens the door for attackers to steal the session ID (`sid`) and impersonate a user. Additionally, the session secret is hardcoded, which further weakens the security.

2. **Briefly explain different ways in which the vulnerability can be exploited.**  
   One method of exploitation is **session hijacking**. Since `httpOnly` is set to `false`, malicious scripts can access the session cookie using JavaScript (e.g., `document.cookie`) and send it to a remote server controlled by the attacker. This can be done by tricking a victim into clicking a link or visiting a malicious site.  
   
   Another method is **Cross-Site Request Forgery (CSRF)**. An attacker can embed hidden forms or scripts on their own site that send requests to the legitimate server. If the victim is authenticated, their browser will automatically include the session cookie with the request, making it appear as if the action was performed by the victim.  
   
   Furthermore, since the session secret is hardcoded and potentially guessable or discoverable, an attacker could forge their own session tokens, bypassing authentication altogether.

3. **Briefly explain why `secure.ts` does not have the spoofing vulnerability in `insecure.ts`.**  
   `secure.ts` mitigates **session hijacking** by setting `httpOnly` to `true`, preventing access to cookies via client-side JavaScript. It also prevents **CSRF** attacks by enabling the `sameSite` attribute, which ensures that cookies are only sent with requests originating from the same site. This stops unauthorized cross-site requests from including the victim's cookies. Additionally, `secure.ts` avoids hardcoding the session secret, making it more difficult for attackers to forge valid session tokens.
