
# Deploy eosio.assert Doc


## Prerequisites

### 1. Build eosio.assert smart contract

Use [eosio.cdt 1.6.1]([https://github.com/EOSIO/eosio.cdt/tree/v1.6.1](https://github.com/EOSIO/eosio.cdt/tree/v1.6.1)) to build [eosio.assert]([https://github.com/EOSIO/eosio.assert](https://github.com/EOSIO/eosio.assert)) smart contract, the build is in contracts/eosio.assert folder.

checksums:

```
openssl dgst -sha256 contracts/eosio.assert.wasm
SHA256(contracts/eosio.assert.wasm)= 2f1e6ca484eeb88d4c0102a90279dc320edf58e3853784101760530be77f60a2
```

```
openssl dgst -sha256 contracts/eosio.assert.abi
SHA256(contracts/eosio.assert.abi)= 92807134f93622ab1ffaf16ac1688658688394155f5838f61b648f28bd01769b
```

### 2. Prepare chain id, name and logo

You can find the logo of Crypto Kylin Testnet under `public` folder, the checksum is:

```
openssl dgst -sha256 public/CryptoKylin-logo.png
SHA256(public/CryptoKylin-logo.png)= 7d0b4735cf3788d38b972b383a80acb56f97955a50dee022e9b976eca282f754
```

the chain id of Crypto Kylin Testnet is `5fff1dae8dc8e2fc4d5b23b2c7665c97f9e9d8edf2b6485a86ba311c25639191`, you can find it via api nodes, such as https://api-kylin.eoslaomao.com/v1/chain/get_info


## STEP 1/3: Create account eosio.assert

### Prepare create account multisig transaction and propose

1. create eosio.assert account :
```
cleos -u https://api-kylin.eoslaomao.com push action eosio newaccount '{"creator":"eosio","name":"eosio.assert","owner":{"threshold":1,"keys":[],"accounts":[{"permission":{"actor":"eosio","permission":"active"},"weight":1}],"waits":[]},"active":{"threshold":1,"keys":[],"accounts":[{"permission":{"actor":"eosio","permission":"active"},"weight":1}],"waits":[]}}' -p eosio -s -j -d > create_assert.json
```

2. delegate bandwidth to eosio.assert:
```
cleos -u https://api-kylin.eoslaomao.com system delegatebw eosio eosio.assert "10 EOS" "10 EOS" -p eosio -s -j -d >> create_assert.json
```

3. buyram for eosio.assert:
```
cleos -u https://api-kylin.eoslaomao.com system buyram eosio eosio.assert "10 EOS" -p eosio -s -j -d >> create_assert.json
```


Merge actions in file `create_assert.json`, also change expire time in `create_assert.json` into future date, e.g. 10 days later.

You will get a transaction like this:

```
{
  "expiration": "2019-06-15T11:36:47",
  "ref_block_num": 0,
  "ref_block_prefix": 0,
  "max_net_usage_words": 0,
  "max_cpu_usage_ms": 0,
  "delay_sec": 0,
  "context_free_actions": [],
  "actions": [{
      "account": "eosio",
      "name": "newaccount",
      "authorization": [{
          "actor": "eosio",
          "permission": "active"
        }
      ],
      "data": "0000000000ea305590afc2d800ea30550100000000010000000000ea30550000000080ab26a70100000100000000010000000000ea305500000000a8ed3232010000"
    },{
      "account": "eosio",
      "name": "delegatebw",
      "authorization": [{
          "actor": "eosio",
          "permission": "active"
        }
      ],
      "data": "0000000000ea305590afc2d800ea3055a08601000000000004454f5300000000a08601000000000004454f530000000000"
    },{
      "account": "eosio",
      "name": "buyram",
      "authorization": [{
          "actor": "eosio",
          "permission": "active"
        }
      ],
      "data": "0000000000ea305590afc2d800ea3055a08601000000000004454f5300000000"
    }
  ],
  "transaction_extensions": [],
  "signatures": [],
  "context_free_data": []
}
```

We have proposed this transaction on Kylin Testnet : [https://kylin.eosx.io/tools/msig/proposal?proposer=eoslaomaocom&name=createassert](https://kylin.eosx.io/tools/msig/proposal?proposer=eoslaomaocom&name=createassert), please review and verify ASAP. 

```
cleos -u https://api-kylin.eoslaomao.com multisig review eoslaomaocom createassert
```

The actual transaction used in this proposal is in file `create_assert.json`.


## STEP 2/3: Deploy eosio.assert contract

```
cleos -u https://api-kylin.eoslaomao.com set contract eosio.assert > deploy_assert.json
```

Update expiration to a future time, set `ref_block_num` and `ref_block_prefix` to 0, and propose it. 

We have proposed this transaction on Kylin Testnet : [https://kylin.eosx.io/tools/msig/proposal?proposer=eoslaomaocom&name=deployassert](https://kylin.eosx.io/tools/msig/proposal?proposer=eoslaomaocom&name=deployassert), please review and verify ASAP. 

```
cleos -u https://api-kylin.eoslaomao.com multisig review eoslaomaocom deployassert
```



## STEP 3/3: Setup chain info

Now we need to call setup action in eosio.assert contracts to register Kylin Testnet onchain.

Here is the payload:

```
{
      "chain_id": "5fff1dae8dc8e2fc4d5b23b2c7665c97f9e9d8edf2b6485a86ba311c25639191",
      "chain_name": "CryptoKylin Testnet",
      "icon": "7d0b4735cf3788d38b972b383a80acb56f97955a50dee022e9b976eca282f754"
}
```

```
cleos push action eosio.assert setup {"chain_id": "5fff1dae8dc8e2fc4d5b23b2c7665c97f9e9d8edf2b6485a86ba311c25639191","chain_name": "CryptoKylin Testnet","icon": "7d0b4735cf3788d38b972b383a80acb56f97955a50dee022e9b976eca282f754"} -p eosio -s -j -d > setup_assert.json
```


Update `setup_assert.json`, set expiration to a future time, set `ref_block_num` and `ref_block_prefix` to 0, and propose it. 

We have proposed this transaction on Kylin Testnet : [https://kylin.eosx.io/tools/msig/proposal?proposer=eoslaomaocom&name=setupassert](https://kylin.eosx.io/tools/msig/proposal?proposer=eoslaomaocom&name=setupassert), please review and verify ASAP. 

```
cleos -u https://api-kylin.eoslaomao.com multisig review eoslaomaocom setupassert
```
