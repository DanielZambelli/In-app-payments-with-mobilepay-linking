# In-app payments with MobilePay linking
In mobile app development: When the MobilePay app is installed a payment can be initated from another app using linking. Using the linking method is convenient, especially if working with Expo projects as MobilePay-AppSwitch needs to be ported and maintained for ReactNative. 

The flow: Payment is initated from app by linking to the MobilePay app. The MobilePay app opens on the device and a request for payment is started. If there is a need to verify the payment MobilePay's API endpoint can be pinged, where a record apears after the user has completed the payment.  

[Questions and requests are welcomed and answered here.](https://danielzambelli.dk/contact)

## Link to initiate payment with MobilePay
Using the link below to initate the payment. The Mobile Pay app will open and the user will complete or abort the payment. The phone must be a **[shop account number](https://www.mobilepay.dk/erhverv/fysiske-butikker/mobilepay-myshop)** from MobilePay and can be created via the link. Amount currency is in DKK. Lock prevents the user from modifying the comment, important if the payment is to be verified in the next step.


*Link example using expos API:*
``` javascript
import { Linking } from 'expo'

Linking.openURL(`mobilepay://send?phone=${9999}&amount=${1.00}&comment=${'Order #999'}&lock=1`)
```

---

## Verify payment using record from API endpoint

To know if the payment has been completed with the expected amount the [MobilePay Transaction API endpoint](https://github.com/MobilePayDev/MobilePay-TransactionReporting-API#transactions-endpoint) can be called. Once the payment has been completed a record with the comment "Order #999" will appear in the API after which the amount can be verified. If the record does not appear, lets say within a few minuttes it can be asummed that the user aborted the payment by closing the app, leaving the payment flow or in another way.


```
{
  ...
   "transactions": [
       {
          "amount": 1.00,
          "senderComment" : "Order #999",
          ...
       },
   ],
}

```
