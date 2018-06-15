# Ripple

One of the best coin for development. Fast transactions. Readable and usable documentation.

Library: [ripple-lib](https://github.com/ripple/ripple-lib).

Docs: [ripple-lib](https://github.com/ripple/ripple-lib/blob/develop/docs/index.md).

Explorers: [livenet/testnet](http://ripplerm.github.io/ripple-wallet/) choose network in upper left corner.

Faucets: [official faucet and wallet generator](https://ripple.com/build/xrp-test-net/).

## Examples
### Common
```
const RippleAPI = require('ripple-lib').RippleAPI;
const api = new RippleAPI({
  server: 'wss://s.altnet.rippletest.net:51233', // testnet
  //server: 'wss://s1.ripple.com', // main
});

await api.connect(); // Call the first connection
//api.disconnect(); // Before unmount app 

async rippleFetchLedgerVersionRange() => {
  const serverInfo = await api.getServerInfo();
  const ledgers = serverInfo.completeLedgers.split('-');
  const minLedgerVersion = Number(ledgers[0]);
  const maxLedgerVersion = Number(ledgers[1]);
  return {
    minLedgerVersion,
    maxLedgerVersion,
  };
};

```

### Create wallet
```
async createWallet() => {
  try {
    const wallet = await api.generateAddress();
    // return {
    //   address: "rwgmJ1NuSsQxt3BwPT26mM113uiKzbZUKe",
    //   secret: "sptxVRTRe3pqJ1LnTaVvLJVvPjjeA"
    // }
  } catch (e) {
    console.error(e);
  }
}
```

### Get balance
```
async fetchBalance() => {
  try {
    const balances = await api.getBalances(address);
    // return [{
    //   currency: "XRP",
    //   value: "1000)"
    // }]
    // return array of object with all balances
  } catch (e) {
    console.error('e');
  }
}
```

### Get transactions list
```
async fetchTransactions() => {
  try {
    const ledgerVersions = await this.rippleFetchLedgerVersionRange();
    const txs = await api.getTransactions(address, ledgerVersions);
    // return array of objects
  } catch (e) {
    console.error('e');
  }
}
```

### Get fee
```
async fetchFee() => {
  try {
    const fee = await api.getFee();
  } catch (e) {
    console.error('e');
  }
}
```

### Send transaction
```
async sendTransaction(address, privateKey, recipient, amount, fee) => {
  try {
    // sign transaction 
    const accInfo = await api.getAccountInfo(address);
    const payload = {
      TransactionType: 'Payment', // https://github.com/ripple/ripple-lib/blob/develop/docs/index.md#transaction-types
      Account: address,
      Fee: fee * 1000000,
      Destination: recipient,
      Amount: amount * 1000000,
      Sequence: accInfo.sequence,
    };
    const payloadStringified = JSON.stringify(payload);
    const signedTX = await api.sign(payloadStringified, privateKey);
    // Submit signed transaction
    api.submit(signedTX.signedTransaction);
  } catch (e) {
    console.error('e');
  }
}
```

## Conclusion

The best and fast cryptoin coin!

