# NEARamp

### Onboarding

To activate a funder on NEARamp, we will need

1. Developer ID - Account ID of developer's NEAR wallet from which they will deposit funds to the contract (i.e superdeveloper.testnet)
2. RSA Public Key - RSA public key used for verifying JWTs signed by developer - [How to](/generating-rsa-keypair) 

### SDK Integration

1. Import the SDK

  ```html
    <script>
      (function (w,d,s,o,f,js,fjs) {
        w['NEARamp']=o;w[o] = w[o] || function () { (w[o].q = w[o].q || []).push(arguments) };
        js = d.createElement(s), fjs = d.getElementsByTagName(s)[0];
        js.id = o; js.src = f; js.async = 1; fjs.parentNode.insertBefore(js, fjs);
      }(window, document, 'script', 'NR', 'https://sdk.cryptocapable.community/nearamp.js'));
      NR('init', {});
    </script>
  ```
  
2. Load the wallet creation page

  ```js
    /* element into which the page will be loaded  */ 
    // <div id="wallet-box"> </div>
    
    /* sdk call */
    window.NR('show', { developerID: '<developer>.testnet', grantToken: token, targetElement: 'wallet-box' });
  ```

### How does NEARamp work?

When a developer ID is onboarded, a funder account is created in the NEARamp contract which holds the developer's deposit and the disbursal amount.

On the developer's app, the end user will use the account creation page loaded by the SDK to create a new funded wallet. To do this, the SDK will generate a passphrase and corresponding keypair for the new account and send the public key to the our server which invokes the account creation method on the contract. The SDK will also generate a different keypair and store it in browser local storage of the app so `near-sdk-js` can recognize the account without the user having to leave the page. The user can also use the passphrase to login to the near browser wallet separately.

The developer controls access to account creation using a `grant_token` for each SDK instance. The `grant_token` functions like a `JWT` used for authentication - the developer server signs a token using their private key which NEARamp will verify using the developer's public key to check whether the account creation request is valid.     

### Grant Token Creation Example

Using Node.JS: 

```js
const jwt = require('jsonwebtoken');

const privateKey = process.env.NEARAMP_SECRET_KEY; // RSA Private Key

function generateJWT() {
  const exp = Math.floor(Date.now() / 1000) + (60 * 60); // token expiry window
  const token = jwt.sign({ exp }, privateKey, { algorithm: 'RS256'});
  return token;
}
```
