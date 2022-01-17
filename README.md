# NEARamp

### Integration

1. Generate a developer ID on NEARamp
2. Import the SDK

  ```html
    <script async crossorigin src="{URL}/sdk.js"> </script>
  ```
3. Initialize the SDK with developer ID
  
  ```js
    NEARamp.init({
      developerID: '<your developer id'>
    });
  ```
  
4. Load the wallet creation page

  ```js
    /* element into which the page will be loaded  */ 
    // <div id="create-wallet-page"> </div>
    
    /* sdk call */
    NEARamp.loadPage({ id: 'create-wallet-page' });
  ```
  
### How does NEARamp work?

When a developer ID is generated, an account is created in the NEARamp contract which holds the developer's deposit and the disbursal amount.

On the developer's app, the end user will use the account creation page loaded by the SDK to create a new funded wallet. To do this, the SDK will generate a passphrase and corresponding keypair for the new account and send the public key to the our server which invokes the account creation method on the contract. The SDK will also generate a different keypair and store it in browser local storage of the app so `near-sdk-js` can recognize the account without the user having to leave the page. The user can also use the passphrase to login to the near browser wallet separately.
