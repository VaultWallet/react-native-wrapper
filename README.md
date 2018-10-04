# Vault Wallet React Native Android Module

Android Native Module wrapper for React Native clients. Customers can use vault wallet as a library to enable access to blockchain assets, key management, backup, contacts, price feed, and eventually smart contracts. 


## Getting Started

These instructions will get you a copy of the project up and running on your local machine for development and testing purposes. See deployment for notes on how to deploy the project on a live system.

### Prerequisites

What things you need to install the software and how to install them

```
Working React Native Project with Android
```

### Installing

A step by step series of examples that tell you how to get your React Native Project running with the Vault React Native wrapper

#### Automatic
Using npm

```
npm install --save vault-wrapper
```

Link react native

```
react-native link vault-wrapper
```
#### Or if you have trouble, make the following additions to the given files manually:

**android/settings.gradle**

Add the Vault wrapper as a new android module
```
include ':vault-wrapper'
project(':vault-wrapper').projectDir = new File(rootProject.projectDir, '../node_modules/vault-wrapper/')
```
**android/build.gradle**

Ensure that the `minSdkVersion`, `compileSdkVersion` and `targetSdkVersion` are set to a secure android level. Using a version too old is not supported as the OS will NOT automatically update security providers using the `AndroidKeyStore`. This is a requirement for our security model.
```
buildscript {
	ext {
        ...
        minSdkVersion = 23
        compileSdkVersion = 28
        targetSdkVersion = 26
        ...
    }
    ...
}
    
```

**android/app/build.gradle**
Include the wrapper in your `app` module's build as a depenency
```
dependencies {
   ...
   implementation project(':vault-wrapper')
```
*Note if you are using an older version of gradle, you can use the now deprecated `compile` instead of `implementation`*

**MainApplication.java**

On top, where imports are:
```
import io.vaultwallet.sdk.wrapper.reactnative.VaultPackage;
```
Add the `new VaultPackage()` class to your list of exported packages.
```
@Override
protected List<ReactPackage> getPackages() {
    return Arrays.asList(
            new MainReactPackage(),
            new VaultPackage()
    );
}

```

## Usage in React Native
### Import the module
```
import Vault from 'vault-wrapper';
```
### Authentication
#### Add Account
```
vault.addAccount(jwt, userId, name, email)
```
#### Remove Account
```
vault.removeAccount(userId)
```
### Realtime Connectivity Status
#### Get current connected state
```
var isConnected = vault.isConnected()
```
#### Observing connected state
```
vault.startObservingConnectivity(callback)
...
vault.stopObservingConnectivity(callback)
```

### Account Managment
**`WalletAccount` is an object representing the `publicAddress`, `type` and `nickname`**

#### Get all **my** wallet accounts 
```
var myWalletAccounts = vault.getWalletAccounts()
```
#### Observing **my** wallet accounts
```
vault.startObservingWalletAccounts(callback)
...
vault.stopObservingWalletAccounts(callback)
```

#### Get all shared accounts
**`SharedAccount` is an object representing the `public address`, `type`, `isMyAccount`, `nickname`and `userId`** -- All the accounts you share to others and the accounts shared to you
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
