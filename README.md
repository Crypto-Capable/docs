![image](https://user-images.githubusercontent.com/6796491/156786160-f87ace11-de35-4914-9ffc-19e62a80d616.png)

# NEARamp Documentation

### Onboarding

To activate a funder on NEARamp, we will need

1. **Developer ID** - Account ID of developer's NEAR wallet (e.g. `superdeveloper.testnet`).
2. **RSA Public Key** - RSA public key used for verifying JWTs signed by developer - [How to](/keypair.md) 

### SDK Endpoints

   **Mainnet**: `https://sdk.nearamp.dev/nearamp.js`

   **Testnet**: `https://sdk.testnet.nearamp.dev/nearamp.js`

### SDK Integration

1. Import the SDK

  ```html
    <script>
      (function (w,d,s,o,f,js,fjs) {
        w['NEARamp']=o;w[o] = w[o] || function () { (w[o].q = w[o].q || []).push(arguments) };
        js = d.createElement(s), fjs = d.getElementsByTagName(s)[0];
        js.id = o; js.src = f; js.async = 1; fjs.parentNode.insertBefore(js, fjs);
      }(window, document, 'script', 'NR', 'https://sdk.testnet.nearamp.dev/nearamp.js'));
    </script>
  ```
  
2. Load the wallet creation page

  ```js
    /* element into which the page will be loaded  */ 
    // <div id="widget-box"> </div>
    
    /* sdk call */
    window.NR('init', {
      developerID: '<developer.testnet|developer.near>', // Required: Onboarded developer id
      grantToken: token,                  // Required: Token signed by developer private key
      targetElement: 'widget-box'         // Required: ID of Parent DOM element for sdk instance
      loginConfig: {                      // Optional: Pass login config authenticate the newly created NEAR account into the app
        contractId: '<>'                                                      
      },
      darkMode: true|false                // Optional: Use dark mode sdk
     });
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

## Managing NEARamp Contract

Developer funds are deposited and managed by the NEARamp smart contract. Funds can be added/withdrawn by directly calling contract methods via NEAR CLI.

### Contract Name
```sh
export CONTRACT_NAME="v1.neaamp.near"
# Or Testnet:
export CONTRACT_NAME="v1-alpha.nearamp.testnet" 
```

### Check balance
```sh
near view $CONTRACT_NAME get_funder  --args '{"account_id":"<developerid>"}' --accountId <developerid>
```

### Deposit funds
```sh
near call $CONTRACT_NAME self_deposit --accountId <developerid> --deposit "1" 
```

### Withdraw funds into account
```sh
near call $CONTRACT_NAME self_withdraw  --args '{"amount":"1"}' --accountId $CONTRACT_NAME # amount to withdraw in near
```

# Support

Please email us on nearamp@cryptocapable.community, and we'd be happy to help with any integration queries
