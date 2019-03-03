# Vault Engine SDK Docuemtation

Vault Engine SDK enables projects to connect to blockchain focused devlopers to integrate key securty, token exhance, account abstraction, and many more features users expect today. By integrating with Vault Engine, devlopers can effecivly introduce a plug and play backend to your application, side stepping responsibility managing and maintaining all of the essential backend features listed here vaultwallet.io/VaultEngineSDK


## Getting Started

These instructions will get you give you the resources needed to integrate Vault Engine into your existing project, or building

~### Prerequisites~

~What things you need to install the software and how to install them~

```
Working React Native Project with Android
```

### ~Installing~ **Need actual bash/cmd install steps**


## Usage in React Native
### Import the module(s)
```
import Vault from 'vault-wrapper';
import { DeviceEventEmitter } from 'react-native';
```
### Authentication
#### Check Account

What does this return? Is this simply a boolean yes no the account exists?

```
VaultWallet.hasAccount((cb) => {
  if (cb) {
    console.log("we have an account")
  } else {
    console.log("we dont have an account")
  }
})
```
#### Add/Update Account

Need more details onwhat the account is. Like is this the account that you AUTH into locally, or the 'account' used on the cloud to identify the user? What is jwt, where is userId initalized/come from? On the backend what does this call do? does it generate the wallet for the account? 

```
vault.addAccount(jwt, userId, name, email)
```
#### Remove Account

Will resolve with clarity on the question above. 
```
vault.removeAccount()
```
### Realtime Connectivity Status
#### Get current connected state

If you need to check if the wallet is currently connected to the blockchain through Vault Wallets backend servers use the following call. 

```
VaultWallet.isConnected(
  (connected) => {
    console.log("Connected: " + connected)
  }
);
```
#### Observing connected state
What are the potental states here? 
```
DeviceEventEmitter.addListener('connectionChange', function(e: Event) {
    console.log("Connected: " + e.connected)
})
```

### Account Managment
**`WalletAccount` is an object representing the `publicAddress`, `type` and `nickname`**

The following documentation is for managing accounts created by the user. 

#### Get all **my** wallet accounts 
By passing the public address of the account you can recieve information on the users accounts. (accounts = AUTH + wallets + contacts? need to define that)

This is public key? What information is passed back in that obj? 
```
VaultWallet.getWalletAccounts((walletAccounts) => {
  console.log(walletAccounts)
})
```
#### Observing **my** wallet accounts
To setup a listenter for one of your wallets, so that you can recieve notifcations of changes in wallet activity, use the following call. 

What are the exact conditions this is fired. Send/Recieve/TX pending etc? 

```
DeviceEventEmitter.addListener('WalletAccounts', function(e: Event) {
    console.log(e)
});
```

#### Get all shared accounts
**`SharedAccount` is an object representing the `public address`, `type`, `isMyAccount`, `nickname`and `userId`** -- All the accounts you share to others and the accounts shared to you

What does accounts shared with others mean? Is this addresses that are generated between you and a contact for the explict purpose of transacting? 
```
var sharedAccounts = vault.getSharedAccounts()
```
#### Observing shared accounts
```
vault.startObservingSharedAccounts(callback)
...
vault.stopObservingSharedAccounts(callback)
```
#### Delete/Stop tracking the account
```
vault.deleteAccount(walletAccount)
```
### Mnemonic
#### Generate a seed (word size can be 16, 20, 24, 28 or 32)
```
var generatedMnemonicSeed = vault.generateSeed(wordSize)
```
#### Import from seed (or generate). 
*This will register the `WalletAccount` automatically with the backend. The key alias can be public -- no need to treat it like a password.*
```
var keyAlias = vault.importMnemonic(menmonicString, addressIndex = 0)
```
### Get WalletAccount for KeyAlias
```
val walletAccount = vault.getWalletAccount(keyAlias)
```
#### Get Public Address of a Mnemonic
*CryptoObject is used in conjunction with the SE/TEE to validate user biometric authentication (optional).*


*We are decomposing the key alias which has the public address of address 0 of the mnemonic*
```
val publicAddress = vault.getPublicAddress(mnemonicString, addressIndex = 0)
// OR
val publicAddress = vault.getPublicAddress(keyAlias, addressIndex = 0, cryptoObject?)
```

#### Generate Another wallet from a mnemonic
*Automatically add the account address to backend*
```
var publicAddress = vault.createWallet(keyAlias, nameOfWallet, addressIndex = 0, cryptoObject?)
// OR
var publicAddress = vault.createWallet(mnemonicString, nameOfWallet, addressIndex = 0, cryptoObject?)
```

### Get Transactions
```
var transactions = vault.getTransactions(walletAccounts)
```
### Observing transaction changes
```
vault.startObservingTransactions(walletAccounts, callback)
...
vault.stopObservingTransactions(callback)
```

### Manage Connections
#### Get all connections

```
var connections = vault.getConnections()
```
### Observing connections changes
```
vault.startObservingConnections(walletAccounts, callback)
...
vault.stopObservingConnections(callback)
```
#### Find a connection
```
vault.findConnection(searchQuery, callbackWithIds)
```
#### Add a connection
```
vault.addConnection(searchResultId)
```

#### Accept a connection request
```
vault.acceptConnectionRequest(userIdOrSearchResultId)
```
#### Decline a connection request
```
vault.declineConnectionRequest(userIdOrSearchResultId)
```
#### Get a pending connection request
```
var pendingRequests = vault.getPendingRequests()
```

#### Observe Pending Connection Requests
```
vault.startObservingConnectionsRequests(callback)
...
vault.stopObservingConnectionRequests(callback)
```
#### Share an account
```
vault.shareAccount(userId, walletAccount)
```
#### Unshare an account
```
vault.unshareAccount(userId, walletAccount)
````

### Send Transaction
Sign and send transaction directly to blockchain
```
vault.postTransaction(accountFromkeyAlias, toAddress, value, cyrptoObject?, callback)
```
### Backup
#### Show Mnemonic
```
vault.showMnemonic(keyAlias, cryptoObject?, callback)
```
#### Cloud backup
```
vault.cloudBackup(keyAlias, cryptoObject?, callback)
```
#### Import cloud backup
```
vault.importCloud()
```

## Versioning

We use [SemVer](http://semver.org/) for versioning. For the versions available, see the [tags on this repository](https://github.com/yoloswaglyfestyle/react-native-wrapper/project/tags). 

## Authors

* **Rahul Behera** - *Initial work* - [Vault Wallet](https://github.com/thebehera)

See also the list of [contributors](https://github.com/your/project/contributors) who participated in this project.

## License

This project is licensed under the [TBD] License - see the [LICENSE.md](LICENSE.md) file for details
