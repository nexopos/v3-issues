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
The part to edit on that file, is quite similar to the previous. We'll then target the line 370. But it might be different as modification might be added on top.

Let's search for "Requests" and this point us straight to the location where we need to edit as we did before.

![adding a modification](https://user-images.githubusercontent.com/5265663/61131195-72e01c00-a4b0-11e9-945b-305fb8f9207e.png)

The modification will be the same. We'll add an array with the key pair : verify => false

```php
$request    =   Requests::post( site_url( array( 'rest', 'nexo', 'order_payment', $current_order[0][ 'ID' ], store_get_param( '?' ) ) ), [
     $this->config->item('rest_key_name')    =>  $_SERVER[ 'HTTP_' . $this->config->item('rest_header_key') ]
 ], array(
          'author'		=>	$author_id,
          'date'			=>	date_now(),
          'payment_type'	=>	$payment[ 'namespace' ],
          'amount'		=>	$payment[ 'amount' ],
     'order_code'	=>	$current_order[0][ 'CODE' ],
     'ref_id'        =>  intval( @$payment[ 'meta' ][ 'coupon_id' ]),
     'payment_id'    =>  @$payment[ 'meta' ][ 'payment_id' ] ?? null
 )[
     'verify'  =>   false // <= here
 ]);

```

It should be similar to this.

![adding a verification false](https://user-images.githubusercontent.com/5265663/61131738-e6365d80-a4b1-11e9-98fa-d642bfad8aeb.png)

### Warning
If you proceed to an update these modification will be overwritten, so you'll need to restore these change after each update.
