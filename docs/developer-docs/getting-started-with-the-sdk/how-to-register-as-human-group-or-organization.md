# 📬 How to register as Human, Group or Organization ?

### Using V1 Hub

You can signup as Human or an Organisation using v1 Hub contract which acts as a token factory. Upon registration (of a person) it deploys a token for the user and stores the user <--> token relation in a mapping.&#x20;

The deployed tokens adhere to the ERC20 standard with added logic for time-based minting.

1. To register as **v1Human**

```javascript

async function registerAsv1Human()
{
   const v1Human = await avatar?.v1Hub.signup();
   console.log(v1Human);
}
```

2. To register as **v1Organisation**

```javascript
async function registerAsv1Organization()
{
    const v1Org = await avatar?.v1Hub.organizationSignup();
    console.log(v1Org);
}
```

### Using V2 Hub

V2Hub supports signing as Human, Organisation or as group. It adheres to ERC1155 and stores the balances for each user and token. Since the individual tokens aren't individually deployed contracts, they don't have an address, Instead they are referenced by a tokenID.

1. To register as **v2Human**&#x20;

<pre class="language-javascript"><code class="lang-javascript"><strong>async function registerAsv2Human()
</strong><strong>{
</strong>    const v2Human = await avatar?.registerHuman("Insert IPFS CID"); // pass the ERC 1155 metadata
    console.log(v2Human);
}

</code></pre>

2. To register as v2Organisation

```javascript

async function registerAsv2Organization()
{
    const v2Organisation = await avatar?.registerOrganization("Test", "Insert IPFS CID");
    console.log(v2Organisation);
}
```

3. To register as v2Group

```javascript
function registerAsv2Group()
{
  const v2Group = avatar?.registerGroup(addresses.BaseGroupMintPolicy, "Group", "GRP", "Insert IPFS CID");
  console.log(v2Group);
}
```
