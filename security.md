# HTTP Security

The term HTTP Security Header summarizes a set of HTTP response headers that allow the webserver to communicate with the browser using security features. These include:

`Content Security Policy (CSP):` With this header, the server specifies which resources are allowed to be loaded onto the page. It can also be used to prevent cross-site scripting attacks and other malicious code from being executed. We'll explain which parameters to set here in a moment.

`X-Frame-Options:` With this header, the server determines whether the page can be displayed in a frame or iframe on another website. This can prevent so-called clickjacking attacks.
```
Example:
X-Frame-Options: DENY
```

`X-XSS protection:` This header is used by the server to determine whether cross-site scripting (XSS) protection is enabled for the page. After activation, foreign scripts are prevented from being executed on the own website.
```
Example:
X-XSS-Protection: 1; mode=block
```

`X-Content-Type-Options:` In order for a browser to display a web page correctly, it must know which file types it is dealing with. If this information is not supplied, it tries to "guess" the correct file type - and attackers can exploit this vulnerability. This command in the HTTP security header prohibits "sniffing".
```
Example:
X-Content-Type-Options: nosniff
```

`Referrer-Policy:` This header is relevant for data protection. The server uses the referrer policy to control whether the link source should be transmitted when linking to other websites.
```
Example:
Referrer-Policy: no-referrer
```

`Permissions-Policy:` With this policy - formerly called feature policy - websites prohibit access to sensitive user data such as webcam, microphone, location or payment interface, which makes potential attacks more difficult.
```
Example: 
Permissions-Policy: <directive>=(<allowlist>), <directive>=(<allowlist>)
```

`HTTP Strict Transport Security (HSTS):` With this header, the server can force the browser to transfer all pages over HTTPS. This ensures the encryption of all data during transmission. Attention: This is not a substitute for setting up properly established server-side HTTPS encryption, but only an additional safeguard.
```
Example:
Strict-Transport-Security: max-age=<expire-time>; includeSubdomains
```