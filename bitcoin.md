# Bitcoin

The system revolves around a private key. 
First you generate private key and then from it get address and public key - offline || onclient.
Transaction also generates offline || onclient. Need data: utxo, address from, fee.
After creating need broadcast to insight.

Library: [bitcore-lib](https://github.com/bitpay/bitcore-lib), [bitcore-explorers](https://github.com/bitpay/bitcore-explorers)(contain bitcore-lib).

Docs: [bitcore-lib](https://bitcore.io/api/lib), [bitcore-explorers](https://github.com/bitpay/bitcore-explorers/blob/master/docs/index.md).

Explorers:
  [livenet](https://insight.bitpay.com), [testet](https://test-insight.bitpay.com)

Faucets: [faucet-1](https://testnet.manu.backend.hamburg/bitcoin-cash-faucet), [faucet-2](https://testnet.manu.backend.hamburg/faucet)

## Examples
### Common
```
import explorers from 'bitcore-explorers';
import axios from 'axios';

const bitcore = explorers.bitcore;
bitcore.Networks.defaultNetwork = bitcore.Networks['testnet']; // 'livenet' by default

const insight = new explorers.Insight();
```

### Create wallet
```
const privateKey = new bitcore.PrivateKey(); // Generate random private key
const address = privateKey.toAddress().toString(); // Get address from private key 
const publicKey = privateKey.toPublicKey().toString(); // Get public key from private key 
```

### Get balance
```
async fetchBalance(address) => {
  try {
    const explorer = 'your exlorer url || use above explorer';
    const { data } = await axios.get(`${explorer}/api/addr/${address}`); 
    cosnt balanceBtc = data.balance; // Get balance in btc
    cosnt balanceSat = data.balanceSat; // Get balance in satoshi
  } catch (e) {
    console.error(e);
  }
}
```

### Get transactions list
```
async fetchTransactions(address) => {
  try {
    const { data } = await axios.get(`${explorer}/api/txs/?address=${address}`);
    cosnt transactions = data; // Transactions list
  } catch (e) {
    console.error(e);
  }
}
```

### Get fee
```
async fetchFee() => {
  try {
    const feeObj = await axios.get(`${config.url}/utils/estimatefee`);
    const fee = feeObj.data[2]; // in btc
  } catch (e) {
    console.error(e);
  }
}
```

### Send transaction
```
async sendTransaction(address, privateKey, fee, recipient, amountSat) => {
  // Fetch utxos
  insight.getUtxos(address, (error, utxos) => {
    if (error) {
      console.error( error);
    } else {
      // Create transaction and sign it
      const transaction = new bitcore.Transaction() 
        .from(utxos)
        .to(recipient, amountSat) // Amount in satoshi
        .change(address)
        .fee(fee)
        .sign(privateKey);

      if (!transaction.getSerializationError()) {
        // Broadcast to network
        insight.broadcast(transaction, (err, data) => {
          if (err) {
            console.error('err', err);
          } else {
            console.log(data);
          }
        });
      }
    }
  });
}
```

## Conclusion
The first full-fledged cryptovalual, but due to the lack of a single center there is no complete documentation. There are no such primitive methods as getting a balance or transactions, let alone sending transactions.
There is no way to specify a custom node.
No help from the community.
Collect all the examples was quite difficult, and even consider that this is one of the popular crypto currency is not recommended to integrate into your application or web site.
