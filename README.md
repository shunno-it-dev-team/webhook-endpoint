# bKash Webhook Notifications API Documentation

## Overview

The **bKash Webhook Notifications API** allows merchants to seamlessly receive real-time updates regarding bKash transactions. With this API, merchants are notified of key events such as payment completions and subscription confirmations, ensuring that they stay informed of important actions on their accounts.

---

## Base URL

The base URL for the bKash Webhook Notifications API is:

```
https://shunnoit.top/shunno-payment/api/v1
```

This is the endpoint to which all webhook notifications are sent, and it should be configured in the merchant's system.

---

## Events

The bKash Webhook Notifications API provides notifications for two main events:

1. **Subscription Confirmation**
2. **Payment Notification**

Each event will trigger a corresponding HTTP POST request to the webhook URL, delivering important information about the event.

---

## 1. Subscription Confirmation

When the merchant subscribes to receive notifications, bKash will send a **Subscription Confirmation** payload. This payload contains a `SubscribeURL` that must be visited in order to confirm the subscription.

### Request URL

```
{{BASE_URL}}/webhooks/notifications?shunno_id=<shunno_id>&client_app=<client_app>
```

### HTTP Method

**POST**

### Headers

The following headers are required in the request:

- `x-app-secret`: Application-specific secret key.
- `x-client-secret`: Client-specific secret key.

### Request Body

```json
{
    "Type": "SubscriptionConfirmation",
    "MessageId": "165545c9-2a5c-472c-8df2-7ff2be2b3b1b",
    "Token": "2336412f37fb687f...",
    "TopicArn": "arn:aws:sns:us-west-2:123456789012:MyTopic",
    "Message": "You have chosen to subscribe to the topic arn:aws:sns:us-west-2:123456789012:MyTopic.\nTo confirm the subscription, visit the SubscribeURL included in this message.",
    "SubscribeURL": "https://sns.us-west-2.amazonaws.com/?Action=ConfirmSubscription&TopicArn=arn:aws:sns:us-west-2:123456789012:MyTopic&Token=2336412f37fb687f...",
    "Timestamp": "2012-04-26T20:45:04.751Z",
    "SignatureVersion": "1",
    "Signature": "EXAMPLEpH+DcEwjAPg8O9mY8dReBSwksfg2...",
    "SigningCertURL": "https://sns.us-west-2.amazonaws.com/SimpleNotificationService-f3ecfb7224c7233fe7bb5f59f96de52f.pem"
}
```

### Query Parameters

- `shunno_id`: The unique identifier of the client or merchant (e.g., `3860`).
- `client_app`: Specifies the application initiating the request. The possible values are:
  - **NETFEE**
  - **HISABNIKASH**
  - **BAYANNOPAY**
  - **SHUNNOIT**
  - **ROOT_BILLING**
  - **SILICON_ISP**

### Response

```json
{
    "status": 200,
    "message": "Subscription Confirmed"
}
```

This response indicates that the subscription was successfully confirmed.

---

## 2. Payment Notification

Once a payment transaction is successfully completed, bKash will send a **Payment Notification** to the merchant's configured webhook URL. This payload contains all relevant details of the payment.

### Request URL

```
{{BASE_URL}}/webhooks/notifications?shunno_id=<shunno_id>&client_app=<client_app>
```

### HTTP Method

**POST**

### Headers

The following headers are required in the request:

- `x-app-secret`: Application-specific secret key.
- `x-client-secret`: Client-specific secret key.

### Request Body

```json
{
    "Type": "Notification",
    "MessageId": "20d48143-6af4-571d-b7cb-d211e6a2ac69",
    "TopicArn": "arn:aws:sns:ap-southeast-1:354285753755:bpt_01823072645",
    "Message": {
        "dateTime": "20180419122246",
        "debitMSISDN": "8801700000001",
        "creditOrganizationName": "Org 01",
        "creditShortCode": "01929918***",
        "trxID": "4J420ANOXC",
        "transactionStatus": "Completed",
        "transactionType": "1003",
        "amount": "100",
        "currency": "BDT",
        "transactionReference": "Test_Payment",
        "merchantInvoiceNumber": "orderId1233"
    },
    "Timestamp": "2018-04-19T12:22:46.236Z",
    "SignatureVersion": "1",
    "Signature": "jZBoouSuStaUqYZY+mruc3r3ST58CPkzjOu8i65dDIReWDcNAvPG...",
    "SigningCertURL": "https://sns.ap-southeast-1.amazonaws.com/SimpleNotificationService-ac565b8b1a6c5d002d285f9598aa1d9b.pem",
    "UnsubscribeURL": "https://sns.ap-southeast-1.amazonaws.com/?Action=Unsubscribe&SubscriptionArn=arn:aws:sns:ap-southeast-1:354285753755:bpt_01823072645:ddc1093b-2885-4179-ae8f-961577b564bd"
}
```

### Query Parameters

- `shunno_id`: The unique identifier of the client or merchant (e.g., `3860`).
- `client_app`: Specifies the application initiating the request. The possible values are:
  - **NETFEE**
  - **HISABNIKASH**
  - **BAYANNOPAY**
  - **SHUNNOIT**
  - **ROOT\_BILLING**

### Response

```json
{
    "status": 200,
    "message": "Notification processed."
}
```

This response indicates that the payment notification has been successfully processed.

---

## Explanation of Query Parameters

1. **shunno\_id**

   - **Description**: The `shunno_id` is a unique identifier assigned to each merchant or client subscribing to the webhook. This ensures that each client’s webhook notifications are properly routed to them.
   - **Example**: `shunno_id=3860`

2. **client\_app**

   - **Description**: The `client_app` parameter specifies which application is generating the webhook request. This helps the system identify the context in which the webhook is being triggered.
   - **Possible Values**:
     - **NETFEE**
     - **HISABNIKASH**
     - **BAYANNOPAY**
     - **SHUNNOIT**
     - **ROOT\_BILLING**
   - **Example**: `client_app=NETFEE`

---

## Error Codes

The following error codes may be returned during API usage:

- **404 Not Found**:
  ```json
  {
      "code": 404,
      "message": "Not Found"
  }
  ```
  This error indicates that the requested resource or endpoint could not be found.

- **401 Invalid App or Client Secret**:
  ```json
  {
      "code": 401,
      "message": "Invalid app or client secret"
  }
  ```
  This error indicates that the provided `x-app-secret` or `x-client-secret` is incorrect.

- **409 Duplicate Payment**:
  ```json
  {
      "code": 409,
      "message": "Duplicate payment"
  }
  ```
  This error indicates that a payment with the same transaction ID has already been processed.

---

## Support Contact

For any integration issues or questions, feel free to reach out to **ShunnoIT**:

- **Email**: [shahadat.cseru@gmail.com](mailto:shahadat.cseru@gmail.com)
- **Phone**: +8801717541865

Our team is available to assist you with any challenges you may encounter during integration or setup.

