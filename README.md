# Deploy eosio.assert Doc


## Prerequisites

### 1. Build eosio.assert smart contract

Use eosio.cdt 1.6.1 to build eosio.assert smart contract, the build is in contracts/eosio.assert folder. 

checksums:

`openssl dgst -sha256 contracts/eosio.assert.wasm`
`SHA256(contracts/eosio.assert.wasm)= 2f1e6ca484eeb88d4c0102a90279dc320edf58e3853784101760530be77f60a2`

`openssl dgst -sha256 contracts/eosio.assert.abi`
`SHA256(contracts/eosio.assert.abi)= 92807134f93622ab1ffaf16ac1688658688394155f5838f61b648f28bd01769b`

### 2. Prepare chain id, name and logo

You can find the logo of Crypto Kylin Testnet under `public` folder, the checksum of it is:

`openssl dgst -sha256 public/CryptoKylin-logo.png`
`SHA256(public/CryptoKylin-logo.png)= ce56c4ad6a92871ca24a493f39be4a3dae209d49b5ed8e950ec50f5efc005ab1`

the chain id of Crypto Kylin Testnet is `5fff1dae8dc8e2fc4d5b23b2c7665c97f9e9d8edf2b6485a86ba311c25639191`, you can find it via api nodes, such as https://api-kylin.eoslaomao.com/v1/chain/get_info


## Create account eosio.assert

### Prepare create account multisig transaction and propose

`cleos -u https://api-kylin.eoslaomao.com push action eosio newaccount '{"creator":"eosio","name":"eosio.assert","owner":{"threshold":1,"keys":[],"accounts":[{"permission":{"actor":"eosio","permission":"owner"},"weight":1}],"waits":[]},"active":{"threshold":1,"keys":[],"accounts":[{"permission":{"actor":"eosio","permission":"owner"},"weight":1}],"waits":[]}}' -p eosio -s -j -d > newaccount.json`

Change expire time in `newaccount.json` into future date, e.g. 10 days later.

We have submitted a `createassert` proposal on Kylin Testnet: TODO. The actual transaction used in kylin testnet is `create_assert.json`.


## Deploy eosio.assert contract 

TODO


## Setup chain info

TODO
