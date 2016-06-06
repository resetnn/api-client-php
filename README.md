# retailCRM API PHP client

PHP-client for [retailCRM API](http://www.retailcrm.pro/docs/Developers/ApiVersion3).

Use [API documentation](http://retailcrm.github.io/api-client-php)

## Requirements

* PHP 5.3 and above
* PHP's cURL support

## Install

1) Get [composer](https://getcomposer.org/download/)

2) Run into your project directory:
```bash
composer require resetnn/api-client-php ~3.0.0 --no-dev
```

If you have not used `composer` before, include autoloader into your project.
```php
require 'path/to/vendor/autoload.php';
```

## Usage

### Get order
```php
$client = new \RetailCrm\ApiClient(
    'https://demo.retailcrm.pro',
    'T9DMPvuNt7FQJMszHUdG8Fkt6xHsqngH'
);


try {
    $response = $client->ordersGet('M-2342');
} catch (\RetailCrm\Exception\CurlException $e) {
    echo "Connection error: " . $e->getMessage();
}

if ($response->isSuccessful()) {
    echo $response->order['totalSumm'];
    // or $response['order']['totalSumm'];
    // or
    //    $order = $response->getOrder();
    //    $order['totalSumm'];
} else {
    echo sprintf(
        "Error: [HTTP-code %s] %s",
        $response->getStatusCode(),
        $response->getErrorMsg()
    );

    // error details
    //if (isset($response['errors'])) {
    //    print_r($response['errors']);
    //}
}
```

### Create order
```php

$client = new \RetailCrm\ApiClient(
    'https://demo.retailcrm.pro',
    'T9DMPvuNt7FQJMszHUdG8Fkt6xHsqngH'
);

try {
    $response = $client->ordersCreate(array(
        'externalId' => 'some-shop-order-id',
        'firstName' => 'Vasily',
        'lastName' => 'Pupkin',
        'items' => array(
            //...
        ),
        'delivery' => array(
            'code' => 'russian-post',
        )
    ));
} catch (\RetailCrm\Exception\CurlException $e) {
    echo "Connection error: " . $e->getMessage();
}

if ($response->isSuccessful() && 201 === $response->getStatusCode()) {
    echo 'Order successfully created. Order ID into retailCRM = ' . $response->id;
        // or $response['id'];
        // or $response->getId();
} else {
    echo sprintf(
        "Error: [HTTP-code %s] %s",
        $response->getStatusCode(),
        $response->getErrorMsg()
    );

    // error details
    //if (isset($response['errors'])) {
    //    print_r($response['errors']);
    //}
}
```
