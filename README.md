Yii2 Bitcko PayPal PayPal Api Extension
=========================================
Yii2 Bitcko PayPal PayPal Api extension  use to integrate simple PayPal payment in your website.

Installation
------------

The preferred way to install this extension is through [composer](http://getcomposer.org/download/).

Either run

```
php composer.phar require bitcko/yii2-bitcko-paypal-api:dev-master

```

or add

```
"bitcko/bitcko/yii2-bitcko-paypal-api": "dev-master"
```

to the require section of your `composer.json` file.


Usage
-----

Once the extension is installed, simply use it in your code by  :

1. Create developer account in PayPal and then create an app.  [PayPal Developer Dashboard](https://developer.paypal.com/).
2. Copy and paste the client Id and client secret in params.php file that exists in config directory:
```php
<?php

return [
    'adminEmail' => 'admin@example.com',
    'payPalClientId'=>'app client id here',
    'payPalClientSecret'=>'app client secret here'
];


```
3. Setup the extension configuration in config/web.php file for yii2 basic temp or config/main.php file for Yii2 advanced temp in components section: 

```php
<?php
'components'=> [
    ...
 'PayPalRestApi'=>[
            'class'=>'bitcko\paypalrestapi\PayPalRestApi',
            'redirectUrl'=>'/site/make-payment', // Redirect Url after payment
            ]
            ...
        ]

```
4. Controller example:

```php
<?php

namespace app\controllers;

use Yii;

use yii\web\Controller;

class PayPalApiController extends Controller
{
   
    public function actionCheckout(){
        // Setup order information array with all items
        $params = [
            'method'=>'paypal',
            'intent'=>'sale',
            'order'=>[
                'description'=>'Payment description',
                'subtotal'=>44,
                'shippingCost'=>0,
                'total'=>44,
                'currency'=>'USD',
                'items'=>[
                    [
                        'name'=>'Item one',
                        'price'=>10,
                        'quantity'=>1,
                        'currency'=>'USD'
                    ],
                    [
                        'name'=>'Item two',
                        'price'=>12,
                        'quantity'=>2,
                        'currency'=>'USD'
                    ],
                    [
                        'name'=>'Item three',
                        'price'=>1,
                        'quantity'=>10,
                        'currency'=>'USD'
                    ],

                ]

            ]
        ];

        Yii::$app->PayPalRestApi->checkOut($params);
    }

    public function actionMakePayment(){
         // Setup order information array 
        $params = [
            'order'=>[
                'description'=>'Payment description',
                'subtotal'=>44,
                'shippingCost'=>0,
                'total'=>44,
                'currency'=>'USD',
            ]
        ];
      // In case of payment success this will return the payment object that contains all information about the payment
      // In case of failure it will return Null
      return  Yii::$app->PayPalRestApi->processPayment($params);

    }
}


```



