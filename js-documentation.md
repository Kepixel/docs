# Anubis.js Documentation

## Table of Contents
- [Introduction](#introduction)
- [Installation](#installation)
- [Basic Usage](#basic-usage)
- [Configuration](#configuration)
- [Tracking Functions](#tracking-functions)
  - [E-commerce Tracking](#e-commerce-tracking)
  - [User Interaction Tracking](#user-interaction-tracking)
  - [Page and Content Tracking](#page-and-content-tracking)
  - [User Authentication Tracking](#user-authentication-tracking)
  - [Custom Events](#custom-events)
- [User Data](#user-data)
- [Custom Data](#custom-data)
- [Error Handling and Validation](#error-handling-and-validation)
- [Advanced Usage](#advanced-usage)
- [Custom Ecommerce Tracking](#custom-ecommerce-tracking)
  - [Data Measured and Reported by Ecommerce Tracking](#data-measured-and-reported-by-ecommerce-tracking)
  - [Manual Ecommerce Tracking: Quick Start](#manual-ecommerce-tracking-quick-start)
  - [Advanced: Manually Tracking Ecommerce Actions in Kepixel](#advanced-manually-tracking-ecommerce-actions-in-kepixel)
  - [Tracking Product Views in Kepixel](#tracking-product-views-in-kepixel-optional)
  - [Tracking Ecommerce Actions](#tracking-ecommerce-actions)
  - [Tracking Cart Updates in Kepixel](#tracking-cart-updates-in-kepixel-optional)
  - [Tracking Orders to Kepixel](#tracking-orders-to-kepixel-required)
- [Troubleshooting](#troubleshooting)
  - [Common Issues and Solutions](#common-issues-and-solutions)
  - [Debugging Tips](#debugging-tips)
- [Performance Optimization](#performance-optimization)
  - [Loading Optimization](#loading-optimization)
  - [Tracking Optimization](#tracking-optimization)
  - [Memory Usage](#memory-usage)
- [Migration Guides](#migration-guides)
  - [Migrating from Google Analytics](#migrating-from-google-analytics)
  - [Migrating from Adobe Analytics](#migrating-from-adobe-analytics)
- [Browser Compatibility](#browser-compatibility)

## Introduction

Anubis.js is a powerful JavaScript tracking library designed to help you collect and analyze user behavior data on your website or web application. It provides a comprehensive set of tracking functions for various user interactions, including e-commerce transactions, page views, user authentication events, and more.

## Installation

### Standard Installation

Add the following code to the `<head>` section of your HTML pages:

```html
<!-- Kepixel -->
<script>
    var _paq = window._paq = window._paq || [];
    _paq.push(['trackPageView']);
    _paq.push(['enableLinkTracking']);
    (function() {
        _paq.push(['setAppId', 'YOUR_APP_ID']);
        var d=document, g=d.createElement('script'), s=d.getElementsByTagName('script')[0];
        g.async=true; g.src='https://edge.kepixel.com/anubis.js'; s.parentNode.insertBefore(g,s);
    })();
</script>
<!-- End kepixel Code -->
```

Replace `'YOUR_APP_ID'` with your actual application ID and update the script source URL to point to your hosted version of anubis.js.

## Basic Usage

Anubis.js uses an asynchronous tracking approach with a command queue. You push commands to the `_paq` array, and they will be executed in order once the library is loaded.

### Tracking a Page View

```javascript
_paq.push(['trackPageView']);
```

### Tracking an Event

```javascript
_paq.push(['trackEvent', 'Category', 'Action', 'Name', 'Value']);
```

### Interactive Examples

For interactive examples of all tracking functions, please visit our [JavaScript Examples](js_examples.html) page. This page provides runnable examples with code snippets for each tracking function, allowing you to see how they work in real-time.

## Configuration

### Setting the Application ID

```javascript
_paq.push(['setAppId', 'YOUR_APP_ID']);
```

### Setting User ID

You must assign a unique and persistent non-empty string that represents each logged-in user. Typically, this ID will be an email address or a username provided by your authentication system.

```javascript
_paq.push(['setUserId', 'user@example.com']);
```

**Important**: You must set the user ID for each pageview, otherwise the pageview will be tracked without the user ID set. A common pattern is to set the user ID immediately before tracking a page view:

```javascript
// Set the user ID before tracking the page view
_paq.push(['setUserId', 'user@example.com']);
_paq.push(['trackPageView']);
```

### Getting Visitor ID

```javascript
var visitor_id;
_paq.push([function() { visitor_id = this.getVisitorId(); }]);
```

## Tracking Functions

Anubis.js provides a wide range of tracking functions for different types of user interactions.

### E-commerce Tracking

#### trackPurchase

Records a purchase event with order ID, currency, value, items, description, user data, and custom data.

```javascript
_paq.push(['trackPurchase',
    'ORDER123',                // order_id
    'USD',                     // currency
    69.97,                     // value
    items,                     // items array
    'Example purchase',        // description
    user_data,                 // user_data object
    custom_data                // custom_data object
]);
```

#### trackAddToCart

Records an add to cart event with currency, value, items, description, user data, and custom data.

```javascript
_paq.push(['trackAddToCart',
    'USD',                     // currency
    49.98,                     // value
    items,                     // items array
    'Example add to cart',     // description
    user_data,                 // user_data object
    custom_data                // custom_data object
]);
```

#### trackInitiateCheckout

Records an initiate checkout event with value, currency, items, user data, and custom data.

```javascript
_paq.push(['trackInitiateCheckout',
    69.97,                     // value
    'USD',                     // currency
    items,                     // items array
    user_data,                 // user_data object
    custom_data                // custom_data object
]);
```

#### trackAddPaymentInfo

Records an add payment info event with value, currency, items, user data, and custom data.

```javascript
_paq.push(['trackAddPaymentInfo',
    69.97,                     // value
    'USD',                     // currency
    items,                     // items array
    user_data,                 // user_data object
    custom_data                // custom_data object
]);
```

#### trackAddToWishlist

Records an add to wishlist event with items, user data, and custom data.

```javascript
_paq.push(['trackAddToWishlist',
    items,                     // items array
    user_data,                 // user_data object
    custom_data                // custom_data object
]);
```

### User Interaction Tracking

#### trackSearch

Records a search event with search string, user data, and custom data.

```javascript
_paq.push(['trackSearch',
    'example search query',    // search_string
    user_data,                 // user_data object
    custom_data                // custom_data object
]);
```

#### trackSiteSearch

Records a site search event with keyword, category, and results count.

```javascript
_paq.push(['trackSiteSearch',
    'Banana',                  // Search keyword
    'Organic Food',            // Search category (optional)
    0                          // Number of results (optional)
]);
```

#### trackEvent

Records a custom event with category, action, name, and value.

```javascript
_paq.push(['trackEvent', 
    'Menu',                    // category
    'Click',                   // action
    'Navigation',              // name (optional)
    1                          // value (optional)
]);
```

### Page and Content Tracking

#### trackPage

Records a page view event with page ID, name, category, type, user data, and custom data.

```javascript
_paq.push(['trackPage',
    'page123',                 // id
    'Example Page',            // name
    'content',                 // category
    'article',                 // type
    user_data,                 // user_data object
    custom_data                // custom_data object
]);
```

#### trackViewContent

Records a view content event with content ID, name, currency, type, value, user data, and custom data.

```javascript
_paq.push(['trackViewContent',
    '123',                     // id
    'Test Product',            // name
    'USD',                     // currency
    'product',                 // type
    19.99,                     // value
    user_data,                 // user_data object
    custom_data                // custom_data object
]);
```

#### trackListView

Records a list view event with list ID, name, category, type, user data, and custom data.

```javascript
_paq.push(['trackListView',
    'list123',                 // id
    'Featured Products',       // name
    'products',                // category
    'featured',                // type
    user_data,                 // user_data object
    custom_data                // custom_data object
]);
```

### User Authentication Tracking

#### trackSignUp

Records a sign up event with user data and custom data.

```javascript
_paq.push(['trackSignUp',
    user_data,                 // user_data object
    custom_data                // custom_data object
]);
```

#### trackLogin

Records a login event with user data and custom data.

```javascript
_paq.push(['trackLogin',
    user_data,                 // user_data object
    custom_data                // custom_data object
]);
```

#### trackCompleteRegistration

Records a complete registration event with parameters, user data, and custom data.

```javascript
_paq.push(['trackCompleteRegistration',
    {method: 'email'},         // params
    user_data,                 // user_data object
    custom_data                // custom_data object
]);
```

### Other Tracking Functions

#### trackAppOpen

Records an app open event with user data and custom data.

```javascript
_paq.push(['trackAppOpen',
    user_data,                 // user_data object
    custom_data                // custom_data object
]);
```

#### trackContact

Records a contact event with user data and custom data.

```javascript
_paq.push(['trackContact',
    user_data,                 // user_data object
    custom_data                // custom_data object
]);
```

#### trackCustomEvent

Records a custom event with parameters, user data, and custom data.

```javascript
_paq.push(['trackCustomEvent',
    {event_name: 'test_event'}, // params
    user_data,                 // user_data object
    custom_data                // custom_data object
]);
```

#### trackGoal

Manually triggers a goal conversion.

By default, Goals in Kepixel are defined as "matching" parts of the URL (starts with, contains, or regular expression matching). You can also track goals for given page views, downloads, or outlink clicks.

In some situations, you may want to register conversions on other types of actions, for example:

- When a user submits a form
- When a user has stayed more than a given amount of time on the page
- When a user does some interaction in your Flash application
- When a user has submitted their cart and has done the payment: you can give the Kepixel tracking code to the payment website which will then register the conversions in your Kepixel database, with the conversion's custom revenue

To trigger a goal conversion:

```javascript
// logs a conversion for goal 1
_paq.push(['trackGoal', 1]);
```

You can also register a conversion for this goal with a custom revenue. For example, you can generate the call to trackGoal() dynamically to set the revenue of the transaction:

```javascript
// logs a conversion for goal 1 with the custom revenue set
_paq.push(['trackGoal', 1, 10.50]);
```

For PHP applications, you might use:

```php
_paq.push(['trackGoal', 1, <?php echo $cart->getCartValue(); ?>]);
```

This allows you to dynamically insert the cart value from your server-side code.

## User Data

The `user_data` parameter is an object that should contain at least one of the following properties:

- `email`: User's email address
- `phone`: User's phone number
- `name`: User's name
- `id`: User's ID

Example:

```javascript
var user_data = {
    email: "user@example.com",
    phone: "1234567890",
    name: "John Doe",
    id: "user123"
};
```

## Custom Data

The `custom_data` parameter is an optional object that can contain any custom properties you want to track.

Example:

```javascript
var custom_data = {
    source: "homepage",
    session_id: "abc123",
    referrer: "google.com"
};
```

## Items Format

For tracking functions that accept an `items` parameter, each item should be an object with the following properties:

- `id`: Item ID (required)
- `name`: Item name (required)
- `price`: Item price (required)
- `quantity`: Item quantity (required)

Example:

```javascript
var items = [
    {
        id: "123",
        name: "Product A",
        price: 19.99,
        quantity: 2
    },
    {
        id: "456",
        name: "Product B",
        price: 29.99,
        quantity: 1
    }
];
```

## Error Handling and Validation

Anubis.js performs validation on the parameters passed to tracking functions and logs warnings to the console when invalid data is provided.

### User Data Validation

The `user_data` parameter is validated to ensure it's an object with at least one of the required properties (email, phone, name, or id). If validation fails, a warning is logged to the console.

### Items Validation

For tracking functions that accept an `items` parameter, each item is validated to ensure it has the required properties (id, name, price, and quantity). If validation fails, a warning is logged to the console.

## Advanced Usage

### Callback Functions

You can pass a callback function to any tracking command:

```javascript
_paq.push([function() {
    // This code will be executed in the context of the tracker
    var visitorId = this.getVisitorId();
    console.log('Visitor ID:', visitorId);
}]);
```

### Link Tracking

Enable automatic link tracking:

```javascript
_paq.push(['enableLinkTracking']);
```

### Custom Page Title

By default Kepixel uses the title of the HTML page to track the page title. You can customize it by using the `setDocumentTitle` function:

```javascript
_paq.push(['setDocumentTitle', 'Custom Page Title']);
```

This is useful when:
- You want to have a different title in your tracking than what's displayed in the browser
- You're tracking single-page applications where the HTML title might not change
- You want to add additional context to the page title for analytics purposes
- You need to standardize page titles across different pages

### Accurately Measure Time Spent on Page

By default, when a user visits only one page view during a visit, Kepixel will assume that the visitor has spent 0 second on the website. This has a few consequences:

- When the visitor views only one page view, the "Visit duration" will be 0 second.
- When the visitor views more than one page, then the last page view of the visit will have a "Time spent on page" of 0 second.

It is possible to configure Kepixel so that it accurately measures the time spent in the visit. To better measure time spent in the visit, add to your JavaScript code the following:

```javascript
// accurately measure the time spent in the visit
_paq.push(['enableHeartBeatTimer']);
```

Kepixel will then send requests to count the actual time spent in the visit, as long as the user is actively viewing the page (i.e. when the tab is active and in focus). The heart beat request is executed when:

- Switching to another browser tab after the current tab was active for at least 15 seconds (can be configured see below).
- Navigating to another page within the same tab.
- Closing the tab.

You can also configure how long a tab needs to be active to be counted as viewed:

```javascript
// Change how long a tab needs to be active to be counted as viewed in seconds
// Requires a page to be actively viewed for 30 seconds for any heart beat request to be sent
_paq.push(['enableHeartBeatTimer', 30]);
```

Note: When testing the heart beat timer, remember to make sure the browser tab has focus and not eg. the developer tools or another panel.

## Custom Ecommerce Tracking

Anubis.js provides powerful Ecommerce tracking capabilities that can be integrated with various e-commerce platforms. This section explains how to implement Ecommerce tracking in your online store.

Anubis.js supports integration with the following e-commerce platforms: WordPress, WooCommerce, MemberPress, Dokan, OpenCart, XT:Commerce, ClicShopping, osCommerce, PrestaShop, Magento, Shopware, and ZenCart.

### Data Measured and Reported by Ecommerce Tracking

A standard Ecommerce tracking setup within Kepixel collects and reports on both individual and aggregate transaction data.

Within the reports, you can benefit from the following metrics:

- **Ecommerce Orders** – Keep track of the total number of website sales in aggregate, or over specific periods.
- **Products Purchased** – You can track the number of products that have been purchased within your total orders. It is also possible to break this down further with the Products report to get detailed statistics.
- **Total Revenue** – The amount of revenue generated from your sales (including tax, shipping, and subtracted discounts). See how much you've made within specific periods at a glance.
- **Subtotals** – The order subtotal, excluding shipping and tax. This can help you get a better understanding of your sales revenue without having to account for varying regional and international charges.
- **Tax** – The total amount of tax charged on all sales. Tracking or excluding this value can help you to understand your pre-tax and post-tax profits across your website.
- **Shipping** – The total shipping costs charged on all sales. When plotted against revenue, you can work out the effectiveness of free shipping offers.
- **Discounts** – The total discount applied to all sales. This helps to quantify the effective marketing costs of discount campaigns.
- **Average Order Value (AOV)** – This is calculated from the total revenue divided by the total number of orders. It can be a useful metric to help you design and track the efficiency of campaigns to increase the value of your orders and ultimately increase revenue.
- **Ecommerce Orders Conversion Rate** – This tells you what percentage of people that visit your website make a purchase. For an ecommerce focused website, this is likely to be one of your key performance indicators (KPI).
- **Abandoned Carts** – The total number and potential revenue of visits where people added products to their shopping cart but ultimately left the site without making a purchase. Kepixel also tracks this as a percentage. While this might not reach zero, you should always be aiming to decrease this number.

### Manual Ecommerce Tracking: Quick Start

This guide provides an example of how to manually implement Ecommerce tracking for a site which uses a custom Ecommerce platform. If you are using an Ecommerce platform that is not custom built, see the platform-specific sections below.

#### Managing Cart Items

Keeping track of cart inventory requires maintaining a list of cart items server-side. This provides the ability to create the `cartProducts` variable with each new page load. If a customer does not yet have items in their cart it is still a good idea to keep track of the empty cart by assigning an empty array to `cartProducts`. This will allow you simply push a new product to the array whether or not there are existing items, preventing the need to check if the array exists before adding an item.

There are two ways to manage cart items in Kepixel:
1. Manage changes to individual items by only tracking the items that are reduced in quantity, removed or are added to the cart.
2. Clear all cart items in Kepixel and track the full list of cart items with each cart update.

For the simplicity of this guide and implementation, option 2 is used below.

#### Advanced: Manually Tracking Ecommerce Actions in Kepixel

If your website isn't powered by a content management system or ecommerce platform that integrates with Kepixel, then it may need to be manually integrated. You can manually integrate Kepixel with any cart software by adding snippets of JavaScript code to your checkout process. The code, described below, sends your user's shopping cart data to Kepixel to track key actions for your analytics.

There are several categories of actions that you will need to integrate for a full Ecommerce tracking setup within Kepixel:

- Product Views (Optional) – This enables per-product conversion rates.
- Cart Updates (Optional) – This enables tracking abandoned cart statistics.
- Order Updates (Required) – This is a required component of Ecommerce tracking.

#### Tracking Product Views in Kepixel (Optional)

By default, Kepixel can let you know your conversion rate for your website as a whole. Additionally you can track how many people have visited a page where your product is available for sale. Then Kepixel will automatically calculate a product conversion rate for each product.

Collecting product and category specific conversion rates can be helpful to identify where certain products may not have enough information, or where specific product categories are under-performing on your site.

There is one JavaScript method for pushing both products and category view data to Kepixel, and that is 'setEcommerceView'. The only difference between tracking products and categories is which parameters are attached to the call. For both, you will also need to make sure that you include a 'trackPageView' call. You can find more details and examples for both below.

##### Tracking Product Views in Kepixel

The following parameters are recommended for tracking product views:

- productSKU (Required) – String – A unique product identifier.
- productName (Required) – String – The name of the product.
- categoryName (Optional) – String/Array – This is either the category name passed as a string, or up to five unique categories as an array, e.g. ["Books", "New Releases", "Technology"]
- price (Optional) – Integer/Float – The cost of the item.

Example Product View Snippet:

```javascript
// Push Product View Data to Kepixel - Populate parameters dynamically
_paq.push(['setEcommerceView',
    "0123456789", // (Required) productSKU
    "Ecommerce Analytics Book", // (Optional) productName
    "Books", // (Optional) categoryName
    9.99 // (Optional) price
]);

// You must also call trackPageView when tracking a product view 
_paq.push(['trackPageView']);
```

##### Tracking Category Views in Kepixel

The following parameters are required and recommended for tracking a category pageviews (not a product page):

- productSKU – Boolean – This should be equal to false.
- productName – Boolean – This should be equal to false.
- categoryName (Required) – String/Array – This is either the category name passed as a string, or up to five unique categories as an array, e.g. ["Books", "New Releases", "Technology"]

Example Category View Snippet:

```javascript
// Push Category View Data to Kepixel - Fill category dynamically
_paq.push(['setEcommerceView',
    false, // Product name is not applicable for a category view.
    false, // Product SKU is not applicable for a category view.
    "Books", // (Optional) Product category, or array of up to 5 categories
]);

// You must also call trackPageView when tracking a category view 
_paq.push(['trackPageView']);
```

#### Tracking Ecommerce Actions

For ease of tracking Ecommerce actions, it is recommended to include all cart items within a single JavaScript variable such as `cartProducts`. This will allow you to loop through the items when reporting Ecommerce actions to Kepixel. Here is an example of how your variable might be created. You would need to use custom script to assign the correct details for each product:

```javascript
//array containing all items in the cart along with product details
var cartProducts = [
{
    productSKU: "0123456789",
    productName: "Ecommerce Analytics Book",
    productCategory: ["Books", "Best sellers"],
    price: 9.99,
    quantity: 1
},
{
    productSKU: "9876543210",
    productName: "Product 2",
    productCategory: ["Electronics", "Gadgets"],
    price: 19.99,
    quantity: 2
},
{
    productSKU: "4567891230",
    productName: "Product 3",
    productCategory: ["Clothing", "T-Shirts"],
    price: 14.99,
    quantity: 3
},
{
    productSKU: "7890123456",
    productName: "Product 4",
    productCategory: ["Toys", "Kids"],
    price: 24.99,
    quantity: 2
}
];
```

#### Tracking Cart Updates in Kepixel (Optional)

To track cart additions and removals, your cart system will need to send the details for every item that remains in the cart after a user adds or removes an item, including those already submitted from prior Add to Cart clicks. This is important to collect accurate information for the abandoned cart feature within Kepixel. The data should be included within a "push", which is the function that enables sending of structured data to Kepixel via JavaScript. You will also need to supply the total value of all items in the cart. so the two required elements for cart tracking are:

1. A push of 'addEcommerceItem' for each product currently in the cart, including the name and SKU at a minimum.
2. A final 'trackEcommerceCartUpdate' push with the cart total passed as a parameter.

Example Product Cart Update:

Each item processed as part of the cart update can contain the following parameters but must include the name and SKU at a minimum.

- productSKU (Required) – String – A unique product identifier.
- productName (Recommended) – String – The name of the product.
- categoryName (Optional) – String/Array – This is either the category name passed as a string, or up to five unique categories as an array e.g. ["Books", "New Releases", "Technology"]
- price (Optional) – Integer/Float – The cost of the item.
- quantity (Optional) – Integer – How many of this item are in the cart. Defaults to 1.

The snippet below contains example data in the format required by Kepixel. All of the parameters should be dynamically filled by your ecommerce platform.

```javascript
// An addEcommerceItem push should be generated for each cart item, even the products not updated by the current "Add to cart" click.
_paq.push(['addEcommerceItem',
    "0123456789", // (Required) productSKU
    "Ecommerce Analytics Book", // (Optional) productName
    ["Books", "Best sellers"], // (Optional) productCategory
    9.99, // (Recommended) price
    1 // (Optional, defaults to 1) quantity
]);

// Pass the Cart's Total Value as a numeric parameter
_paq.push(['trackEcommerceCartUpdate', 15.5]); 
```

Now that you have a variable containing all of your customer's cart items, let's look at how we can track these items to Kepixel. The script below will loop through each item and call the method `addEcommerceItem` while providing product details. It then aggregates the total value of all items in the `cartProducts` variable and finally calls the method `trackEcommerceCartUpdate` to send the cart data to Kepixel:

```javascript
//reset the cart items in Kepixel
_paq.push(['clearEcommerceCart']);

//loop through each item to report cart data to Kepixel
cartProducts.forEach(function(product) {
    _paq.push(['addEcommerceItem',
    product.productSKU,
    product.productName,
    product.productCategory,
    product.price,
    product.quantity
]);
});

//calculate the sum total of all cart items
var totalCartValue = cartProducts.reduce(function(total, product) {
    return total + (product.price * product.quantity);
}, 0);

//push the cart data to Kepixel
_paq.push(['trackEcommerceCartUpdate', totalCartValue]);
```

#### Tracking Orders to Kepixel (Required)

Tracking orders is the minimum required configuration for a successful ecommerce tracking implementation. Order metrics are likely to be the most valuable for any ecommerce store, so you need to make sure that you set up order tracking correctly.

There are two main elements required to push an ecommerce purchase to Kepixel:

1. Product details for each product purchased, containing the Product SKU at a minimum.
2. Order details; containing a unique order ID and the total revenue at a minimum.

The order is typically tracked on the order confirmation page after payment has been confirmed.

Note: The unique order ID generated by your cart software ensures the same order isn't tracked twice.

Example of Adding a Product to the order:

The snippet below contains example data in the format required by Kepixel. All of the parameters, which are shown within double quotation marks, should be dynamically filled by your ecommerce platform.

- productSKU (Required) – String – A unique product identifier.
- productName (Recommended) – String – The name of the product.
- productCategory (Optional) – String/Array – This is either the category name passed as a string, or up to five unique categories as an array, e.g. ["Books", "New Releases", "Technology"]
- price (Optional) – Integer/Float – The cost of the item.
- quantity (Optional) – Integer – How many of this item are in the order. Defaults to 1.

The Code Example:

```javascript
// Product Array
_paq.push(['addEcommerceItem',
  "01234567890", // (required) SKU: Product unique identifier
  "Ecommerce Analytics Book", // (optional) Product name
  "Books", // (optional) Product category. You can also specify an array of up to 5 categories eg. ["Books", "New releases", "Biography"]
  9.99, // (Recommended) Product Price
  1 // (Optional - Defaults to 1)
]);
```

Example of Tracking the Ecommerce Order:

The second part of the order update code passed to Kepixel is a summary of the order. At a minimum, it should include an order ID and the total revenue value. The variables passed, in order, are:

- orderId (Required) – String – A unique reference number to avoid duplication.
- grandTotal (Required) – Integer/Float – The order total revenue including tax & shipping with any discounts subtracted.
- subTotal (Optional) – Integer/Float – The order total excluding shipping.
- tax (Optional) – Integer/Float -The amount of tax charged.
- shipping (Optional) – Integer/Float – The amount charged for shipping.
- discount (Optional) – Integer/Float/Boolean – Discount offered? Default to false. Otherwise, you should include a numeric value.

The Code Example:

```javascript
// Order Array - Parameters should be generated dynamically
_paq.push(['trackEcommerceOrder',
    "000123", // (Required) orderId
    10.99, // (Required) grandTotal (revenue)
    9.99, // (Optional) subTotal
    1.5, // (optional) tax
    1, // (optional) shipping
    false // (optional) discount
]);
```

For tracking the order, we can use the same cart value as calculated in the previous step when calling the `trackEcommerceOrder` method. If you need to add tax or shipping cost, provide a value for these within this call:

```javascript
_paq.push(['trackEcommerceOrder',
    "000123", // (Required) orderId
    totalCartValue, // (Required) grandTotal (revenue)
    0, // (Optional) subTotal
    0, // (optional) tax
    0, // (optional) shipping
    false // (optional) discount
]);
```

#### Developer Warning: Currency Variables must be passed as an Integer or Float

The following currency parameters must be supplied as integers or floats, not as strings:

addEcommerceItem() Parameters:
- Price

trackEcommerceOrder() Parameters:
- grandTotal
- subTotal
- tax
- shipping
- discount

For example, all the following values are not valid currency variables:

- "5.3$"
- "EUR5.3"
- "5,4"
- "5.4"
- "5.44"

The following values are valid currency variables:

- 5
- 5.3
- 5.44

If your Ecommerce software provides the values as string only, you can call the Javascript function parseFloat() to convert the values into a valid format. First make sure the string you want to work with does not contain currency symbols or other characters, for example "554.30". You can then pass that value through the function as shown:

```javascript
parseFloat("554.30");
```

Note: The JavaScript function parseFloat() does not support comma separated decimal values "25,3" so you might also have to replace the commas with dots first.

#### Other JavaScript functions that you may find useful

The following options may be useful especially when your Ecommerce shop is a Single Page App:

- removeEcommerceItem(productSKU) – This removes a product from the order by SKU. You still need to call trackEcommerceCartUpdate to record the updated cart in Kepixel.
- clearEcommerceCart() – This clears the order completely. You still need to call trackEcommerceCartUpdate to record the updated cart in Kepixel.
- getEcommerceItems() – Returns all ecommerce items currently in the untracked ecommerce order. The returned array will be a copy, so changing it won't affect the ecommerce order. To affect what gets tracked, use the methods: addEcommerceItem(), removeEcommerceItem(), clearEcommerceCart(). Useful to see the list of what will be tracked before you track an order or cart update.

#### Tracking Product Views

Tracking product views can work in a similar way to tracking cart times. When a product page is loaded, pass the product details to a variable named `viewProduct`, for example. This variable would contain details for the viewed product. For instance:

```javascript
var viewProduct = {
    name: "Neon Lamp",
    sku: "SKU1",
    category: ["Category1", "Subcategory1"]
};
```

Just before tracking the page view, call the `setEcommerceView` method while passing the details from your variable:

```javascript
//set the commerce view
_paq.push(['setEcommerceView',
    viewProduct.name, // Product name is not applicable for a category view.
    viewProduct.sku, // Product SKU is not applicable for a category view.
    viewProduct.category, // Product category, or array of up to 5 categories
]);

//you must call trackPageView when tracking an Ecommerce view
_paq.push(['trackPageView']);
```

## Troubleshooting

This section covers common issues you might encounter when implementing Anubis.js and how to resolve them.

### Common Issues and Solutions

#### Tracking Not Working

If your tracking is not working as expected, check the following:

1. **Script Loading Issues**
   - Ensure the Anubis.js script is loading correctly. Check your browser's network tab for any 404 errors.
   - Make sure the script is included in the `<head>` section of your HTML.
   - Verify that your AppID is correct.

2. **Command Queue Issues**
   - Ensure you're using the correct command format: `_paq.push(['commandName', param1, param2, ...])`.
   - Check that the `_paq` array is properly initialized before use: `var _paq = window._paq = window._paq || [];`.

3. **Tracking Timing Issues**
   - Some tracking commands must be called in a specific order. For example, `setEcommerceView` must be called before `trackPageView`.
   - User ID should be set before tracking page views: `_paq.push(['setUserId', 'user@example.com']); _paq.push(['trackPageView']);`.

#### Data Validation Errors

Anubis.js validates the data you pass to tracking functions and logs warnings to the console when invalid data is provided:

1. **User Data Validation**
   - Ensure `user_data` is an object with at least one of the required properties (email, phone, name, or id).
   - Example of valid user data:
     ```javascript
     var user_data = {
         email: "user@example.com",
         phone: "1234567890",
         name: "John Doe",
         id: "user123"
     };
     ```

2. **Items Validation**
   - For tracking functions that accept an `items` parameter, each item must have the required properties (id, name, price, and quantity).
   - Example of valid items:
     ```javascript
     var items = [
         {
             id: "123",
             name: "Product A",
             price: 19.99,
             quantity: 2
         }
     ];
     ```

3. **E-commerce Tracking Validation**
   - Currency values must be passed as numbers (integers or floats), not as strings.
   - Invalid: `"5.3$"`, `"EUR5.3"`, `"5,4"`, `"5.4"`, `"5.44"`
   - Valid: `5`, `5.3`, `5.44`
   - If your e-commerce software provides values as strings, use `parseFloat()` to convert them: `parseFloat("554.30")`.

#### Browser Compatibility Issues

If you're experiencing issues in specific browsers:

1. **Internet Explorer Issues**
   - Anubis.js requires IE8+ and certain features like `window.JSON`.
   - For older browsers, consider using a polyfill for JSON.

2. **Mobile Browser Issues**
   - Ensure your tracking code works correctly on mobile devices by testing on various devices and browsers.
   - Be aware that some mobile browsers handle cookies differently, which can affect visitor tracking.

3. **Cookie Blocking**
   - Modern browsers increasingly block third-party cookies, which can affect tracking.
   - Consider using the `setCookieDomain` function to ensure cookies are set correctly for your domain.

### Debugging Tips

1. **Console Logging**
   - Anubis.js logs warnings and errors to the console. Check your browser's console for messages.
   - Add your own console.log statements to debug tracking calls:
     ```javascript
     _paq.push([function() {
         console.log('Visitor ID:', this.getVisitorId());
     }]);
     ```

2. **Network Monitoring**
   - Use your browser's network tab to monitor tracking requests.
   - Look for requests to your Kepixel server and check the query parameters.

3. **Callback Functions**
   - Use callback functions to verify tracking:
     ```javascript
     _paq.push(['trackPageView']);
     _paq.push([function() {
         console.log('Page view tracked!');
     }]);
     ```

## Performance Optimization

Optimize your Anubis.js implementation for better performance:

### Loading Optimization

1. **Asynchronous Loading**
   - Anubis.js is designed to load asynchronously, which means it won't block your page rendering.
   - Ensure you're using the async loading pattern shown in the installation section.

2. **Script Placement**
   - Place the Anubis.js initialization code in the `<head>` section of your HTML to start tracking as early as possible.
   - This ensures you capture user behavior from the moment they land on your page.

### Tracking Optimization

1. **Minimize Tracking Calls**
   - Combine tracking calls where possible to reduce the number of network requests.
   - For example, set custom dimensions and user ID before tracking a page view, rather than making separate calls.

2. **Batch Processing**
   - For high-traffic sites, consider batching multiple tracking events together.
   - This reduces the number of network requests and improves performance.

3. **Use the Heart Beat Timer Wisely**
   - The heart beat timer is useful for measuring time spent on page, but it generates additional requests.
   - Set an appropriate interval (e.g., 30 seconds) to balance accuracy and performance.

### Memory Usage

1. **Clear E-commerce Items**
   - After tracking an order, e-commerce items are automatically cleared.
   - For cart updates, consider using `clearEcommerceCart()` when appropriate to free up memory.

2. **Avoid Memory Leaks**
   - Be careful with event listeners and callbacks to avoid memory leaks.
   - Clean up any custom event listeners when they're no longer needed.

## Migration Guides

### Migrating from Google Analytics

If you're migrating from Google Analytics to Kepixel with Anubis.js, here's how to map common tracking functions:

| Google Analytics                      | Anubis.js Equivalent                   |
|--------------------------------------|---------------------------------------|
| `gtag('config', 'GA_MEASUREMENT_ID')` | `_paq.push(['trackPageView'])`         |
| `gtag('event', 'view_item', {...})`   | `_paq.push(['trackViewContent', ...])` |
| `gtag('event', 'add_to_cart', {...})` | `_paq.push(['trackAddToCart', ...])`   |
| `gtag('event', 'purchase', {...})`    | `_paq.push(['trackPurchase', ...])`    |
| `gtag('set', 'user_id', 'USER_ID')`   | `_paq.push(['setUserId', 'USER_ID'])`  |

Key differences to be aware of:

1. **Initialization**
   - Google Analytics uses the `gtag` function, while Anubis.js uses the `_paq` array.
   - Anubis.js requires an AppID instead of a Measurement ID.

2. **Event Structure**
   - Google Analytics uses a generic `event` method with different event names.
   - Anubis.js has specific methods for different types of events.

3. **Custom Dimensions**
   - In Google Analytics, custom dimensions are set as parameters in the event object.
   - In Anubis.js, use the `setCustomDimension` method.

4. **E-commerce Tracking**
   - Google Analytics uses the `purchase` event with an items array.
   - Anubis.js requires calling `addEcommerceItem` for each item before calling `trackEcommerceOrder`.

### Migrating from Adobe Analytics

If you're migrating from Adobe Analytics to Kepixel with Anubis.js, here's how to map common tracking functions:

| Adobe Analytics                      | Anubis.js Equivalent                   |
|------------------------------------|---------------------------------------|
| `s.t()`                            | `_paq.push(['trackPageView'])`         |
| `s.tl(this, 'o', 'Link Name')`     | `_paq.push(['trackLink', url, 'link'])` |
| `s.products = '...'`               | `_paq.push(['addEcommerceItem', ...])` |
| `s.events = 'purchase'`            | `_paq.push(['trackEcommerceOrder', ...])` |
| `s.eVar1 = 'value'`                | `_paq.push(['setCustomDimension', 1, 'value'])` |

Key differences to be aware of:

1. **Initialization**
   - Adobe Analytics uses the `s` object, while Anubis.js uses the `_paq` array.
   - Anubis.js has a simpler initialization process.

2. **Event Structure**
   - Adobe Analytics uses variables like `s.events` and `s.products`.
   - Anubis.js has specific methods for different types of events.

3. **Custom Variables**
   - Adobe Analytics uses eVars and props.
   - Anubis.js uses custom dimensions and custom variables.

4. **E-commerce Tracking**
   - Adobe Analytics uses the `s.products` string and `s.events`.
   - Anubis.js requires calling `addEcommerceItem` for each item before calling `trackEcommerceOrder`.

## Browser Compatibility

Anubis.js is compatible with modern browsers and IE8+. It requires the following minimum browser features:

- ECMAScript 3 (ECMA-262, edition 3)
- try..catch and for..in statements
- Named anonymous functions
- Array.push method
- encodeURIComponent and decodeURIComponent functions
- getElementsByTagName method
- window.JSON object
