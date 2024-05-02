---
description: >-
  Understand how Time Circles is calculate and how you can convert it to CRC and
  vice versa
---

# ⏳ Converting Time Circles (TC) to CRC



## What are Time Circles ?

Time Circles (TC) are a time-dependent display unit for [Circles](https://github.com/CirclesUBI/circles-contracts). The unit is based on the daily UBI payout every user receives in CRC and the fixed inflation rate. It normalizes the daily UBI payout to 24 TC regardless of the inflation.



### Understanding CRC and inflation relation&#x20;

The [Circles Hub contract](https://blockscout.com/xdai/mainnet/address/0x29b9a7fBb8995b2423a71cC17cf9810798F6C543/read-contract#address-tabs) is responsible for the UBI payout to the user. It contains a hardcoded inflation rate of 7% per year. This means the daily amount of CRC a user receives increases every year by 7%.

[Inflation](https://en.wikipedia.org/wiki/Inflation), also known as devaluation of money, occurs when the supply of money increases, but the number of goods and services that can be purchased stays the same. This results in products and services becoming more expensive and the single unit of money being worth less.

### TC & Demurrage

With Time Circles, the daily UBI payout is fixed to 24 TC. At first glance, the inflation rate is not present anymore. However, the inflation rate is still hardcoded in the contract and the actual UBI payout is still in CRC. The additional CRC payout must have an effect on the Time Circles as well. Time Circles resort to so-called "[demurrage](https://www.investopedia.com/terms/d/demurrage.asp)" to represent the loss of value. This means that a Time Circle received today will be worth a little less tomorrow. The effect of demurrage is that the user's TC balance will decrease over time while the prices for goods and services stay the same.



### How to calculate Time Circles in CRC and vice-versa

To simplify the conversion process we will be utilising this npm library [https://www.npmjs.com/package/@circles/timecircles](https://www.npmjs.com/package/@circles/timecircles).

```javascript
npm i @circles/timecircles
```

Once installed, we will import this `{crcToTc, tcToCrc}` function.

<pre class="language-javascript"><code class="lang-javascript"><strong>import {crcToTc, tcToCrc} from "@circles/timecircles";
</strong></code></pre>

```javascript
import {crcToTc, tcToCrc} from "@circles/timecircles";

// Since TC are time-dependent we'll always need a timestamp
const transactionTimestamp = new Date("2022-05-03T04:21:25.000Z");
// The amount in CRC
const transactionCrcAmount = 8.566935185185093;  

// Convert to TC
const tcAmount = crcToTc(transactionTimestamp, transactionCrcAmount);
console.log(`${transactionCrcAmount} CRC are ${tcAmount} TC at ${transactionTimestamp}`);

// Convert back to CRC
const crcAmount = tcToCrc(transactionTimestamp, tcAmount);
console.log(`${crcAmount} TC are ${crcAmount} CRC at ${transactionTimestamp}`);

```

You can also checkout the conversion tool here : [https://circlesubi.github.io/timecircles/](https://circlesubi.github.io/timecircles/)
