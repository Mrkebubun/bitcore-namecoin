# Namecoin Support Module for Bitcore

[![NPM Package](https://img.shields.io/npm/v/bitcore-namecoin.svg?style=flat-square)](https://www.npmjs.org/package/bitcore-namecoin)
[![Build Status](https://travis-ci.org/brandonrobertz/bitcore-namecoin.svg?branch=master)](https://travis-ci.org/brandonrobertz/bitcore-namecoin)
[![Coverage Status](https://coveralls.io/repos/brandonrobertz/bitcore-namecoin/badge.svg?branch=master)](https://coveralls.io/r/brandonrobertz/bitcore-namecoin?branch=master)

`bitcore-namecoin` adds namecoin support to bitcore for creating namecoin `name_*` transactions, private keys, and scripts in [Node.js](http://nodejs.org/) and web browsers.

Note: This is still experimental software. This module is not intended for use in production environments, or for use where real money is at stake, at this point.

See [the main bitcore repo](https://github.com/bitpay/bitcore) for more information.

## Getting Started

```sh
npm install bitcore-namecoin
```

```sh
bower install bitcore-namecoin
```

To import a namecoin WIF (from vanitygen, for example):

```javascript
// return bitcore with namecoin commands overlayed
var bitcore = require('bitcore-namecoin');
var privateKey = bitcore.PrivateKey.fromWIF('74pxNKNpByQ2kMow4d9kF6Z77BYeKztQNLq3dSyU4ES1K5KLNiz');
var address = privateKey.toAddress();
console.log( address.toString());
// NAMEuWT2icj3ef8HWJwetZyZbXaZUJ5hFT
```

To create a `name_new` transaction:

```javascript
var bitcore = require('bitcore-namecoin');
var tx = new bitcore.Transaction()
  .from(utxo)
  .nameNew('d/name', 'randomvalue', 'mzGfeiJFdQyiuQnhB45aeBYefzHJSsiSfj')
  .change('mpe83RGRVWibHrdgfmkJwTxgufNs9quaZC')
  .fee(0.005)
  .sign(privKeySet); // utxo and nameNew output addrs need to have privKeys here
// now we can broadcast out to the network
var serialized = tx.serialize();
```

To create a `name_firstupdate` transaction:

```javascript
var tx = new bitcore.Transaction()
  .from([utxo, nameNewUtxo])
  .nameFirstUpdate('d/name', '092abbca8a938103abcc', 'VALUE', 'mzGfeiJFdQyiuQnhB45aeBYefzHJSsiSfj')
  .change('mpe83RGRVWibHrdgfmkJwTxgufNs9quaZC')
  .fee(constants.NETWORK_FEE.satoshis)
  .sign([privKeys[inputAddr], privKeys[nameNewAddr]]);
var serialized = tx.serialize();
```
To create a `name_update` transaction:

```javascript
var tx =  new bitcore.Transaction()
  .from(utxos)
  .nameUpdate('AAAAAAAAAA', 'CCCCCCCCCC', 'mkGdewyuvU13uHzpMUZe2t8ii4LKgKC8mE')
  .change('mkVNqbVqcYxi3zB2fRfiRQonf4JjwdAvnE')
  .fee(constants.NETWORK_FEE.satoshis)
  .sign([privKeys[outputAddr],privKeys[nameFirstUpdate]]);
var serialized = tx.serialize();
```

## Details

`bitcore-namecoin` works by pulling in bitcore and then adding Namecoin-specific
version constants, name operation functions onto `Transaction`, and patches
a few bitcore functions that do not allow for altcoin compatability (specifically
`bitcore.Script.fromString` and `Transaction.prototype._fromNonP2SH`). These will
hopefully by replaced by native Bitcore functions as I work to improve Bitcore's
altcoin compatability.

## Contributing

Contributions are welcome! See [CONTRIBUTING.md](https://github.com/bitpay/bitcore/blob/master/CONTRIBUTING.md) on the main bitcore repo for information about how to contribute.

## License

Code released under [the MIT license](https://github.com/bitpay/bitcore/blob/master/LICENSE).

Written in 2015 by Brandon Robertz.
