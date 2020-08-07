---
layout: post
title:  "Unintended control over the email body in the password reset notification - Mozilla"
date:   2020-08-05 21:44:46 -0800
categories: blog
---
While testing Mozilla, I found it was possible to include HTML code and plaintext content in the password reset email coming from Mozilla.
<br />


When resetting the password on Mozilla, I found Mozilla includes browser, OS, and IP in its email notification. They were trying to parse the user-agent string into a readable device description from the request, but when no valid user-agent is provided then the whole string is taken as device description and then the string gets inserted into the email without any further sanity checks. This allows us to include HTML content into the email. 


**POC**


```http
POST /v1/password/forgot/send_code HTTP/1.1
Host: api.accounts.firefox.com
User-Agent: <br><br>This is fake message with fake <a href="http://phishing-url.com">http://phishing-url.com/</a><br><br>
Accept: */*
Accept-Language: en-US,en;q=0.5
Accept-Encoding: gzip, deflate, br
Content-Type: application/json
Referer: https://accounts.firefox.com/
Content-Length: 597
Origin: https://accounts.firefox.com
Connection: close

data...
```

<br />
The email looks something like
![](/images/mozilla.png)


There wasn't any rate limit on password reset functionality. So, it was possible to send emails in bulk with relevant DKIM/SPF checks passing.

**Timeline**

May 24, 2017 - Report Sent\
Jun 13, 2017 - Bounty Awarded by Mozilla
