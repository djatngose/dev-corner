# CSRF
Cross-Site Request Forgery (CSRF)
Cross-site request forgery attacks attempt to perform requests against sites where the user is logged in by tricking the user’s browser into sending a request from a different site. To accomplish this, a specially crafted site (or item) must contain the URL to the target. A common example is an <img> tag embedded in a malicious page with the src pointing to the attack’s target. For instance:
<!-- This is embedded in another domain's site -->
<img src="http://target.site.com/add-user?user=name&grant=admin">
The above <img> tag will send a request to target.site.com every time the page that contains it is loaded. If the user had previously logged in to target.site.com and the site used a cookie to keep the session active, this cookie will be sent as well. If the target site does not implement any CSRF mitigation techniques, the request will be handled as a valid request on behalf of the user. JWTs, like any other client-side data, can be stored as cookies.

Short-lived JWTs can help in this case. Common CSRF mitigation techniques include special headers that are added to requests only when they are performed from the right origin, per session cookies, and per request tokens. If JWTs (and session data) are not stored as cookies, CSRF attacks are not possible. Cross-site scripting attacks are still possible, though.

# How it works?
CSRF (Cross-Site Request Forgery) attack, the attacker cannot directly obtain the victim's cookies. CSRF attacks exploit the trust that a website has in the authenticated user's browser by tricking the browser into making unintended and unauthorized requests on behalf of the user.

Here's how a typical CSRF attack works:

The victim, let's say a logged-in user, visits a malicious website controlled by the attacker.

The malicious website contains code that automatically sends requests (e.g., form submissions, AJAX requests) to a target website where the victim is authenticated.

These requests exploit the victim's existing authentication and may perform actions on the target website without the victim's knowledge or consent.

Now, the crucial point is that the attacker cannot directly access the victim's cookies or any sensitive information stored in the cookies. The Same Origin Policy enforced by web browsers prevents the attacker's malicious website from reading or manipulating cookies from another domain.

However, in a CSRF attack, the attacker's goal is not to steal the victim's cookies but to perform actions on the target website using the victim's authenticated session. The browser automatically includes the cookies associated with the target website in the CSRF request, as it considers the request to be legitimate.

# To protect against CSRF attacks, web applications commonly employ countermeasures such as:

`CSRF tokens`: Web applications generate and include unique tokens in forms or requests. The tokens are validated on the server-side to ensure that the request originated from a legitimate source.

SameSite cookies: Cookies can be set with the SameSite attribute to restrict their use to the same origin, preventing them from being sent in cross-origin requests.

Referer header checking: The server can verify that requests originate from the same domain by checking the Referer header.

Double-submit cookie: A separate cookie is generated for each user session and its value is included in both a cookie and a request parameter. The server compares the two values to ensure they match.

Implementing these countermeasures can help mitigate CSRF attacks and protect the integrity of user actions on a website.






