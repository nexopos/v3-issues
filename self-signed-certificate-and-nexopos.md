### How to use NexoPOS with self signed certificate

If you have a self signed certificate on your hosting. NexoPOS won't be able to process payment. In fact, during the payment, a CURL operation is made
which involve sending HTTP request to NexoPOS itself. During that process, if you have a SSL auto-signed the payment will fail, because self signed certificate aren't allowed.

This guide will let you know how to bypass this issue and make the payment works.
     
### Disabling the SSL certificate verification

In order to disable the SSL verification, you need to change the script manually at 2 specific locations.

#### Updating : application/models/Nexo_Checkout.php

We need to make sure that the request send to NexoPOS doesn't verify the certifacate. Here is the place we'll proceed the modification.

![screenshot-github com-2019 07 12-12-39-22](https://user-images.githubusercontent.com/5265663/61125870-dbc09780-a4a2-11e9-80cc-fb3eaeb9be3a.png)

At this location, we'll add an array with the key pair : verify => false.

![screenshot-github com-2019 07 12-12-39-22 (1)](https://user-images.githubusercontent.com/5265663/61126153-a0729880-a4a3-11e9-858c-ced1951e9cac.png)


#### Updating : application/modules/nexo/inc/traits/orders.php
