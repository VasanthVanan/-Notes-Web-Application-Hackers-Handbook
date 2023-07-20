# Chapter-5 Bypassing Client-Side Controls

This chapter exposes the security flaw of web applications depending on client-side controls and explores their vulnerabilities.

## 5.1 Transmitting Data Via the Client 

### 5.1.1 Hidden Form Fields 
- Hidden form fields are used to transmit data via the client in a way that is not directly visible or modifiable by the user.
- The data is stored within the form and sent back to the server when the form is submitted.
- The user can modify the hidden field's value by editing the source code or using an intercepting  proxies like Burp Suite.
  ```html
  <form method=”post” action=”Shop.aspx?prod=1”>
  Product: iPhone 5 <br/>
  Price: 449 <br/>
  Quantity: <input type=”text” name=”quantity”> (Maximum quantity is 50)
  <br/>
  <input type=”hidden” name=”price” value=”449”>
  <input type=”submit” value=”Buy”>
  </form>
  ```

  ```http
  POST /shop/28/Shop.aspx?prod=1 HTTP/1.1
  Host: mdsec.net
  Content-Type: application/x-www-form-urlencoded
  Content-Length: 20
  quantity=1&price=449
  ```

  💡 Tip <br> 
  
    see whether you can submit a negative amount as the price. The attacker receives a refund to his credit card and also the item he ordered — a win-win situation
  
### 5.1.2 HTTP Cookies  
- HTTP cookies are another mechanism for transmitting data via the client.
- Cookies are not displayed on-screen and cannot be directly modified by the user.
- However, cookies can be modified using an intercepting proxy.
  
  ```http
  HTTP/1.1 200 OK
  Set-Cookie: DiscountAgreed=25
  Content-Length: 1530

  POST /shop/92/Shop.aspx?prod=3 HTTP/1.1
  Host: mdsec.net
  Cookie: DiscountAgreed=25
  Content-Length: 10
  quantity=1
  ```

### 5.1.3 URL Parameters  

- Applications often use preset URL parameters to transmit data via the client.
- Parameters can be easily modified by users without tools if they are visible in the browser's location bar (GET request).
- Certain scenarios may attempt to hide or prevent modification of URL parameters, but intercepting proxies can still be used to modify them.
  
  ```http
  http://mdsec.net/shop/?prod=3&pricecode=32
  ```

### 5.1.4 The Referer Header 

- Browsers include the `Referer header` in HTTP requests to indicate the URL of the page from which the request originated.
- This header can be leveraged to transmit data via the client.
- The user can easily modify the Referer header using an intercepting proxy.

    ```http
    GET /auth/472/CreateUser.ashx HTTP/1.1
    Host: mdsec.net
    Referer: https://mdsec.net/auth/472/Admin.ashx
    ```

### 5.1.5 Opaque Data  

Opaque data is encrypted or obfuscated data transmitted via the client, making it unintelligible. It is often part of the application's `session-handling mechanism`.

- Opaque data can be observed in `hidden fields` or `tokens` transmitted via the client.
- These tokens include `session tokens`, `anti-CSRF` tokens, and `one-time URL` tokens.
 
    ```html
    <form method=”post” action=”Shop.aspx?prod=4”>
    Product: Nokia Infinity <br/>
    Price: 699 <br/>
    Quantity: <input type=”text” name=”quantity”> (Maximum quantity is 50)
    <br/>
    <input type=”hidden” name=”price” value=”699”>
    <input type=”hidden” name=”pricing_token”
    value=”E76D213D291B8F216D694A34383150265C989229”>
    <input type=”submit” value=”Buy”>
    </form>
    ```

#### Hack Steps:

- Decipher the obfuscation algorithm if the plaintext behind the opaque string is `known`.
- Leverage application functions to obtain the opaque string resulting from a controlled plaintext value.
- Replay the opaque string's value in other contexts to achieve a malicious effect.
- Attack the server-side logic responsible for decrypting or deobfuscating the opaque string by submitting malformed variations of it.

### 5.1.6 The ASP.NET ViewState 

The `ASP.NET ViewState` is a mechanism for transmitting opaque data via the client in ASP.NET web applications. It is used to `preserve the state` of the current page and can `store arbitrary information`.

- The `ViewState` is a hidden field that contains `serialized information` about the page's state.
- It allows the server to `recreate elements` within the user interface across successive requests.
- Developers can also use the ViewState to store `custom information`.
- The ViewState is `Base64-encoded` and can be easily decoded to access its contents.

💡 Tip <br> 
    
    When decoding Base64-encoded strings, start decoding from positions that are multiples of four, as Base64 is a block-based format where every 4 bytes of encoded data translate into 3 bytes of decoded data.
    

#### Hack Steps:

- Verify if `MAC protection` is `enabled` for the ViewState to prevent tampering.
- Decode the ViewState using Burp Suite to check if sensitive data is transmitted via it.
- Modify a specific parameter within the ViewState without disrupting its structure to test for error messages.
- Review the purpose of each parameter within the ViewState and attempt to submit crafted values to identify vulnerabilities.
- Test each significant page of the application for ViewState hacking vulnerabilities, as MAC protection can be enabled or disabled on a per-page basis. Burp Scanner can help identify pages that use the ViewState without MAC protection.

## 5.2 Capturing User Data: HTML Forms 
HTML forms are commonly used to capture user input and impose restrictions or perform validation checks on the data submitted by clients, including data gathered on the client computer itself.

### 5.2.1 Length Limits  

```html
<form method=”post” action=”Shop.aspx?prod=1”>
  Product: iPhone 5 <br/>
  Price: 449 <br/>
  Quantity: <input type=”text” name=”quantity” maxlength=”1”> <br/>
  <input type=”hidden” name=”price” value=”449”>
  <input type=”submit” value=”Buy”>
  </form>
```

- The server-side application imposes a maximum length of 1 on the quantity field in an HTML form.
- The browser prevents the user from entering more than one character into the input field.
- The restriction can be circumvented by intercepting the request or response and modifying the form or removing the maxlength attribute.
  
### 5.2.2 Script-Based Validation  

- Customized client-side input validation is implemented within scripts in HTML forms.
- Disabling JavaScript bypasses the validation, but it may break the application if it relies on client-side scripting.
- Intercepting the validated submission and modifying the data is a common way to defeat JavaScript-based validation.
  
  ```html
  <form method="post" action="Shop.aspx?prod=2" onsubmit="return validateForm(this)">
  Product: Samsung Multiverse <br/>
  Price: 399 <br/>
  Quantity: <input type="text" name="quantity"> (Maximum quantity is 50) <br/>
  <input type="submit" value="Buy">
    </form>
    <script>
    function validateForm(theForm) {
    var isInteger = /^\d+$/;
    var valid = isInteger.test(quantity) && quantity > 0 && quantity <= 50;
    if (!valid)
        alert('Please enter a valid quantity');
    return valid;
    }
    </script>
  ```

### 5.2.3 Disabled Elements

- A disabled element on an HTML form is grayed out, cannot be edited, and is not sent to the server upon form submission.
- Disabled elements may indicate the presence of unused parameters in the application.
- Test whether the server-side application still processes disabled parameters.

    <img src="https://lh3.googleusercontent.com/pw/AIL4fc974zCdV_fEtHpp1Ikk382taY4Iu8abeFdsuhBpWZmb7wUIxPvdWPMiqA2OifDTDxu_jk7jeQt70mLXmYBpp6odfKLGN3LuMDhf_NjmNFxLq2AnpSCEM9D5DLsnUn53-WwagiNcFU7LdVvQQ9je6sF6=w1478-h582-s-no" width="600" height="200">
