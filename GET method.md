As mentioned in the IDOR Exploitation Demo file, we can now see how the GET method simply displays the user-entered information on the URL query string or even in the server logs as stated below:


GET method:

```
from flask import request
@app.route(‘/order_details’, methods=[‘GET’])
def order_details():
 order_id = request.args.get(‘order_id’)
 # IDOR vulnerability in here due to lack of ownership control
 # of taken order_id object !
 order = Orders.query.filter_by(id=order_id).first_or_404()
 return render_template(‘order_details.html’, order=order)
 ```

BURPSUITE capture of the GET method:

```
GET /chat HTTP/1.1
Host: aca11f311f58aa94cbd9ae77004a0042.web-security-academy.net
Cache-Control: max-age=0
Sec-Ch-Ua: " Not;A Brand";v="99", "Google Chrome";v="97", "Chromium";v="97"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "macOS"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/97.0.4692.99 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: none
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Accept-Encoding: gzip, deflate
Accept-Language: en-GB,en-US;q=0.9,en;q=0.8
Connection: close
```

Since the data sent is part of the URL, it is also saved in the browser history and server logs in plaintext. 

To overcome this vulnerability, one of the proposed patchworks which have proven to be effective to a certain extent is the application of Session Storage. More details about this patch is listed in the "Patchwork" file. 
