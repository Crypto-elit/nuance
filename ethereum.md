# Ethereum

A full-fledged, self-contained and convenient system. Gives a wide range of options for creating a wallet

Library: [ethers.js](https://github.com/ethers-io/ethers.js/).

Docs: [ethers.js](https://docs.ethers.io/ethers.js/html/getting-started.html).

Explorers: [livenet](https://etherscan.io/), [testet](https://ropsten.etherscan.io/).

Faucets: [faucet-1](https://ropsten.ethers.io/#!/app-link/0xa5681b1fbda76e0d4ab646e13460a94fdcd3c1c1.ethers.space/), [faucet-2](http://faucet.ropsten.be:3001/).

## Examples
### Common
```
// init wallet with created private key
const privateKey = 'you ethereum wallet private key';
const wallet = new ethers.Wallet(privateKey);
const utils = ethers.utils;

// Provider for test network
const provider = new ethers.providers.getDefaultProvider({
  name: 'ropsten',
  chainId: 3,
});

// Provider for mainnet
const provider = new ethers.providers.EtherscanProvider();

// Provider for custom network
const provider = new ethers.providers.JsonRpcProvider(config.server);

wallet.provider = provider;
```

### Create wallet
```
async createWallet() => {
  try {
    // Create random key
    const wallet = await ethers.Wallet.createRandom();
    
    // or
    
    // Create from seed (12 word phrase)
    const phrase = 'some 12 word for ethereum wallet secret'
    const wallet = await ethers.Wallet.fromMnemonic(phrase);
    
    // or
    
    // Create with username and password
    const wallet = await ethers.Wallet.fromBrainWallet(username, password);
    
    // wallet example:
    // {
    //     address: "0xa40648B1943E903C860ed02A7058bc0204E5eEc5"
    //     defaultGasLimit: 15000
    //     mnemonic: "load owner fork unveil six pipe wife load repair boy bleak fall"
    //     path: "m/44'/60'/0'/0/0"
    //     privateKey: "0x773588b50f8ed61b7e5759d4471f8d4680f0ce8b02f618e1534d997cdfa73a7b"
    //     provider: '...'
    // }
    
  } catch (e) {
    console.error(e)
  }
}
```

### Get balance
```
async fetchBalance() => {
  try {
    const balanceBignumber = await wallet.getBalance(); // return as Bignumber
    const balance = ethers.utils.formatEther(balanceBignumber); // human readable format
  } catch (e) {
    console.error('e');
  }
}
```

### Get transactions list
```
async fetchTransactions() => {
  try {
    const transactions = await etherscanProvider.getHistory(address); // transactions list
  } catch (e) {
    console.error('e');
  }
}
```

### Get fee
```
async fetchTransactions() => {
  try {
    const feeBignumber = await provider.getGasPrice(); // return as Bignumber
    const fee = utils.formatEther(feeBignumber); // human readable format
  } catch (e) {
    console.error('e');
  }
}
```

### Send transaction
```
async sendTransaction(address, recipient, amount, fee, nonce) => {
  const transaction = {
    to: recipient,
    value: utils.parseEther(amount),
    gasPrice: fee,
    nonce, // Transaction order number. Calculate like: get list of all the wallet transactions and get it length
  };

  try {
    transaction.gasLimit = await wallet.estimateGas(transaction);
    const signedTransaction = await wallet.sign(transaction);
    txHash = await provider.sendTransaction(signedTransaction);
    console.log(txHash);
  } catch (e) {
    console.error('e');
  }
}
```

## Conclusion

The library is good and if it were not for two jambs, it would be excellent.
1) You must count transaction nonce
2) There is no date in the transaction object

