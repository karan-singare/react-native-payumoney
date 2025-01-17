[Note]: Archiving in favour of official sdk and lack of time to maintain, see [#63](https://github.com/karan-singare/react-native-payumoney/issues/63#issuecomment-1166210031)

---

# react-native-payumoney

<img src='https://img.shields.io/badge/license-MIT-blue.svg' />  <a href="https://www.npmjs.com/package/react-native-payumoney"><img alt="npm dowloads" src="https://img.shields.io/npm/dm/react-native-payumoney.svg"/></a> <a href="https://www.npmjs.com/package/react-native-payumoney"><img alt="npm version" src="https://badge.fury.io/js/react-native-payumoney.svg"/></a> [![Build Status](https://travis-ci.org/Suraj-Tiwari/react-native-payumoney.svg?branch=master)](https://travis-ci.org/Suraj-Tiwari/react-native-payumoney) [![Greenkeeper badge](https://badges.greenkeeper.io/Suraj-Tiwari/react-native-payumoney.svg)](https://greenkeeper.io/)
___
### Installation

#### If Upgrading from 0.0.3
> Make sure to remove existing package first. 

Install package:

```bash
$ npm i @mogi/react-native-payumoney --save
```

### Only For RN <= 0.59

```bash
$ react-native link @mogi/react-native-payumoney
```

Add following line in Podfile
```bash 
pod 'PayUmoney_PnP'
```
Then, run the following command:
```bash
$ pod install
```

### RN >= 0.60

> No need to do anything 

### Making Payment Request

1. Import PayuMoney module to your component:
    ```js
    import PayuMoney from 'react-native-payumoney';
    ```

2. Call `PayuMoney()` method with the payment `options`. Method
returns a **Promise** 
```js
const payData = {
    amount: '10.0',
    txnId: '1594976828726',
    productName: 'product_info',
    firstName: 'firstname',
    email: 'xyz@gmail.com',
    phone: '9639999999',
    merchantId: '5960507',
    key: 'QylhKRVd',
    successUrl: 'https://www.payumoney.com/mobileapp/payumoney/success.php',
    failedUrl: 'https://www.payumoney.com/mobileapp/payumoney/failure.php',
    isDebug: true,
    hash:
        '461d4002c1432b3393cf2bfaae7acc4c50601c66568fb49a4a125e060c3bfc0e489290e7c902750d5db3fc8be2f180daf4d534d7b9bef46fa0158a4c8a057b61',
}

Payumoney(payData).then((data) => {
    // Payment Success
    console.log(data)
}).catch((e) => {
    // Payment Failed
    console.log(e)
})
```

### Validating Hash
> Don't use in production just for testing purpose

```js
import {HashGenerator} from 'react-native-payumoney';

HashGenerator({
    key: "QylhKRVd",
    amount: "10.0",
    email: "xyz@gmail.com",
    txnId: "1594976828726",
    productName: "product_info",
    firstName: "firstname",
    salt: "seVTUgzrgE",
})

// output: 461d4002c1432b3393cf2bfaae7acc4c50601c66568fb49a4a125e060c3bfc0e489290e7c902750d5db3fc8be2f180daf4d534d7b9bef46fa0158a4c8a057b61
```

Server side function to get Hash Key

```php
function makeHash($key, $txnid, $amount, $productinfo, $firstname, $email){
    $salt = "XXXXXX"; //Please change the value with the live salt for production environment

    $payhash_str = $key . '|' . checkNull($txnid) . '|' . checkNull($amount) . '|' . checkNull($productinfo) . '|' . checkNull($firstname) . '|' . checkNull($email) . '|||||||||||' . $salt;

    $hash = strtolower(hash('sha512', $payhash_str));
    return $hash;
}

function checkNull($value)
{
    if ($value == null) {
        return '';
    } else {
        return $value;
    }
}

```

### Troubleshooting

    
## `{ success: 0 } or { success: false }`

This is very common error, when your server side hash is calculated in-correctly or
when trying to use **Web Merchant KEY + SALT** on sandbox in Android  
Please use Following KEY, SALT, MERCHANT ID generated by PayU for sandbox usage  
Or you can always ask them for new pair of KEY & SLAT for Android usage

```js
  MID : 5960507
  Key : QylhKRVd
  Salt : seVTUgzrgE
```

Below is the test card details for doing a test transaction in the testing mode.

```js
  Card No - 4242 4242 4242 4242
  Expiry - 22/2222 // any date in future
  CVV - 999 // any 3 digits
  Name - Test // anything
```

## Merchant Key missing in release mode

Edit `android/app/proguard-rules.pro` and add 
```
-keep class com.surajtiwari.reactnativepayumoney.** { *; }
```
see issue [#43](https://github.com/karan-singare/react-native-payumoney/issues/43)

## Could not find com.payumoney.sdkui:plug-n-play:1.6.1.

Add `jcenter()` in your android/build.gradle

see [example](https://github.com/karan-singare/react-native-payumoney/blob/c366d8ce6db21ddf9c0f62ff95082a2659126cd2/example/android/build.gradle#L35)

## Running example

### 1. Install dependencies

```bash
$ cd ./example && npm install
```

### 2. Run it on Android

```bash
$ cd ./example && npm run android
```
[version-badge]: https://img.shields.io/npm/v/react-native-payumoney.svg?style=flat-square
[package]: https://www.npmjs.com/package/react-native-payumoney

<!--<a href="https://circleci.com/gh/Suraj-Tiwari/react-native-payumoney"><img src="https://circleci.com/gh/Suraj-Tiwari/react-native-payumoney.svg?style=shield" alt="build"></a>-->
