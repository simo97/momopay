

# MOMOPAY



## Configuration

Add config to `config/services.php`

```text


    'mtn' => [
        'go_live' => env('MTN_MOMO_GO_LIVE',false),
        'api_key' => env('MTN_MOMO_USER_API_KEY'),
        'reference_id' => env('MTN_MOMO_ID'),
        'subscription_key' => env('MTN_MOMO_KEY'),
        'target_environment' => env('MTN_MOMO_ENV',env('MTN_MOMO_GO_LIVE') === true ? 'mtncameroon' : 'sandbox'), //
        'payment_callback_route' => env('MTN_MOMO_CALLBACK_URL','payment.momo.callback'),
        'payment_callback_host' => env('MTN_MOMO_CALLBACK_HOST'),
        'notification_email' => env('MTN_MOMO_NOTIFICATION_EMAIL'),
    ],


```

configuration description

- `reference_id` : is the user id 
- `subscription_key` : is the Ocp-Apim-Subscription-Key
- `target_environment` : can be sanbox or mtncameroon (when you go live)



## Add routes


```text

Route::any('/payment/momo/callback', 'PaymentMomoController@callback')->name('payment.momo.callback');
Route::get('/payment/momo/transaction/{id}', 'PaymentMomoController@getPayment')->name('payment.momo.gettransaction');

```

## Use in controller

```php

    $momoRequestToPayDto = new \Nkaurelien\Momopay\Fluent\MomoRequestToPayDto;
    $momoRequestToPayDto->amount = 3000;
    $momoRequestToPayDto->currency = 'XAF'; // Use EUR when you are in sandbox mode
    $momoRequestToPayDto->payeeNote = '';
    $momoRequestToPayDto->payerMessage = '';
    $momoRequestToPayDto->externalId = 'my_product_id';
    $momoRequestToPayDto->payer->telephone = '2376XXXXXXXX';

    $refId = \Ramsey\Uuid\Uuid::uuid4()->toString();

    $requestToPayResult = $this->paymentMomoRepository->requestToPay($momoRequestToPayDto, $refId);
```
