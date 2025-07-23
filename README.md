# Headless Commerce API Documentation

## Introduction
This document outlines practical work with Salesforce Headless Commerce APIs. We'll go step by step through the process, from creating a cart to placing an order using Postman.  
The original repository with the base implementation is available [here](https://github.com/shane-saltbox/Salesforce-Mojo/tree/main/Headless%20Commerce%20API%27s).

## Getting Started
Before you begin, determine whether you'll be using an existing environment or setting up a new one from scratch.

> [!NOTE]
> If you’re setting up your own environment, follow the steps in the [Environment Setup](#environment-setup) section. If your Commerce instance is already configured and ready, you can skip this part and go straight to using the API via Postman.

### Environment Setup
If you're starting with a fresh environment:
1. Create a Sandbox (or use a Developer Edition org).
2. In **Setup**, search for `Commerce` and open **Commerce Setup Assistant** to install and configure Commerce Cloud.
3. In the **App Launcher**, search for `Commerce`, open the app, and create a B2B or D2C Webstore.
4. On the Webstore **Home** page, complete all required setup tasks to enable store activation.
5. Activate your store.

> [!IMPORTANT]
> Don’t forget to go to the **Customers** tab and verify that your desired account is added to a **Buyer Group**


### Postman Setup
To connect Postman to Salesforce, follow this step-by-step module:  
👉 [Quick Start: Connect Postman to Salesforce](https://trailhead.salesforce.com/content/learn/projects/quick-start-connect-postman-to-salesforce)


# 🧩 Task Execution
## 📌 Task Steps
1. [Create Cart](#🛒-1-create-cart)
2. [Create Cart Items](#➕-2-create-cart-items)
3. [Start Store Checkout](#🧾-3-start-store-checkout)
4. [Check Checkout Status](#⏳-4-check-checkout-status)
5. [Update Checkout Details](#✏️-5-update-checkout-details)
6. [Tokenize CC Payment](#6-tokenize-cc-payment)
7. [Authorize Payment](#7-authorize-payment)
8. [Place Order](#8-place-order)

> [!NOTE]
> For details on variables, request structure, and error handling, refer to the official API documentation linked in each section.

## 🛒 1. Create Cart
**Documentation:**  
🔗 [Create Cart – Salesforce Docs](https://developer.salesforce.com/docs/atlas.en-us.chatterapi.meta/chatterapi/connect_resources_commerce_webstore_carts.htm)

**POST Request:**
```
{{_endpoint}}/services/data/v{{version}}/commerce/webstores/{{webstoreId}}/carts
```

**Body:**
```json
{
  "name": "My Cart",
  "type": "Cart",
  "currencyIsoCode": "USD",
  "effectiveAccountId": "customer_account_id"
}
```
- Replace `customer_account_id` with the real buyer **Account ID**


## ➕ 2. Create Cart Items
**Documentation:**  
🔗 [Create Cart Items – Salesforce Docs](https://developer.salesforce.com/docs/atlas.en-us.chatterapi.meta/chatterapi/connect_resources_commerce_webstore_cart_items.htm)

**POST Request:**
```
{{_endpoint}}/services/data/v{{version}}/commerce/webstores/{{webstoreId}}/carts/{{cartId}}/cart-items?effectiveAccountId=customer_account_id
```
- Replace `customer_account_id` with the real buyer **Account ID** that you used in previous task
- Add variable `cartId` in your **Collection**

**Body:**
```json
{
  "productId": "product_id",
  "quantity": 5,
  "type": "Product"
}
```
- Replace `product_id` with the real **Product ID**


## 🧾 3. Start Store Checkout
**Documentation:**  
🔗 [Start Store Checkout – Salesforce Docs](https://developer.salesforce.com/docs/atlas.en-us.chatterapi.meta/chatterapi/connect_resources_commerce_webstore_checkouts_start_checkout.htm)

**POST Request:**
```
{{_endpoint}}/services/data/v{{version}}/commerce/webstores/{{webstoreId}}/checkouts?effectiveAccountId=customer_account_id
```
- Replace `customer_account_id` with the real buyer **Account ID** that you used in previous task

**Body:**
```json
{
  "cartId": "{{cartId}}"
}
```


## ⏳ 4. Check Checkout Status
**Documentation:**  
🔗 [Check Checkout Status](https://developer.salesforce.com/docs/atlas.en-us.chatterapi.meta/chatterapi/connect_resources_commerce_webstore_checkouts.htm)

**GET Request:**
```
{{_endpoint}}/services/data/v{{version}}/commerce/webstores/{{webstoreId}}/checkouts/active?effectiveAccountId=customer_account_id
```
- Replace `customer_account_id` with the real buyer **Account ID** that you used in previous task


## ✏️ 5. Update Checkout Details
🔗 [Update Checkout Details](https://developer.salesforce.com/docs/atlas.en-us.chatterapi.meta/chatterapi/connect_resources_commerce_webstore_checkouts.htm)

**PATCH Request:**
```
{{_endpoint}}/services/data/v{{version}}/commerce/webstores/{{webstoreId}}/checkouts/active?effectiveAccountId=customer_account_id
```
- Replace `customer_account_id` with the real buyer **Account ID** that you used in previous task

```json
{
  "deliveryAddress": {
    "name": "Alan Johnson",
    "firstName": "Alan",
    "lastName": "Johnson",
    "country": "LV",
    "city": "Riga",
    "street": "Salaspils iela",
    "postalCode": "1234"
  },
  "desiredDeliveryDate": "2021-05-28T16:41:41.090Z",
  "shippingInstructions": "type code 1234 on gate keypad"
}
```
- Check which countries you have for shipping in the store and replace country value


## 💳 6. Tokenize CC Payment

## ✅ 7. Authorize Payment

## 📦 8. Place Order

