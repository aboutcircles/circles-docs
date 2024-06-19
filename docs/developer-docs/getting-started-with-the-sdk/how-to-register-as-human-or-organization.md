# 📬 How to register as Human or Organization ?

### Using Circles V1

You can signup as Human or an Organisation using v1 Hub contract which acts as a token factory. Upon registration (of a person) it deploys a token for the user and stores the user <--> token relation in a mapping.&#x20;

The deployed tokens adhere to the ERC20 standard with added logic for time-based minting.

1. To register as **v1Human**

```javascript
async function registerAsHuman()
{
   const v1Human = await sdk.registerHuman();
   console.log(v1Human);
}
```

2. To register as **v1Organisation**

```javascript
async function registerAsOrganization()
{
    const v1Org = await sdk.registerOrganization();
    console.log(v1Org);
}
```
