# Multi-Blockchain Wallet in Python

  Working on a new start up, we must create a portfolio managing system that supports traditional assets and crypto assets as well. We'll build a system that can create HD wallets. We'll build it out using Python and the command line tool `hd-wallet-derive` that supports BIP32, BIP39, and BIP44 and other non-standard derivation paths. Once the wallet is integrated, there is potential to manage billions of addresses for over 300+ coins. 

  First make sure you have the appropriate dependencies. 
    * PHP must be installed on your operating system.
    * You will need to clone the `hd-wallet-derive` tool.
    * `bit` Python Bitcoin library.
    * `web3.py` Python Ethereum library.
## Installing `hd-wallet-derive`

  * Create a project directory called `wallet` and `cd` into it.

  * Clone the `hd-wallet-derive tool` into this folder and install it.

  * Create a symlink called `derive` for the `hd-wallet-derive/hd-wallet-derive.php` script into the top level project directory like so: `ln -s hd-wallet-derive/hd-wallet-          derive.php derive`

  * This will clean up the command needed to run the script in the code, to call `./derive` instead of `./hd-wallet-derive/hd-wallet-derive.php.exe`

  * Test run the `./derive` script properly, use one of the examples on the repo's README.md.

  * Note: If one gets an error running `./derive`, as it happens in Windows machine then use: `./hd-wallet-derive/hd-wallet-derive.php.exe`

  * Create a file called `wallet.py` -- universal wallet script.

## Deriving Wallet Keys 

The following code derives the wallet keys for ETH And BTCTEST

`def derive_wallets(coin=BTC, mnemonic=mnemonic, depth=3):
    command = f'php ./derive -g --mnemonic="{mnemonic}" --cols=all --coin={coin} --numderive={depth} --format=json'
    p = subprocess.Popen(command, stdout=subprocess.PIPE, shell=True)
    output, err = p.communicate()
    p_status = p.wait()
    return json.loads(output)`
    
 `coins = {
    ETH: derive_wallets(coin = ETH),
    BTCTEST: derive_wallets(coin = BTCTEST),
}
print(coins)`
  
  
  
<img width="960" alt="2021-07-20" src="https://user-images.githubusercontent.com/78872373/126400756-15aeaaab-ce5e-40f0-a43a-ad2ca247aa84.png">

## Preparing Transactions 

`def create_tx(coin, account, recipient, amount):
    if coin == ETH: 
        gasEstimate = w3.eth.estimateGas(
            {"from":eth_acc.address, "to":recipient, "value": amount}
        )
        return { 
            "from": eth_acc.address,
            "to": recipient,
            "value": amount,
            "gasPrice": w3.eth.gasPrice,
            "gas": gasEstimate,
            "nonce": w3.eth.getTransactionCount(eth_acc.address)
        }
    
    elif coin == BTCTEST:
        return PrivateKeyTestnet.prepare_transaction(account.address, [(recipient, amount, BTC)])`

## Deploying Transactions

`def send_tx(coin, account, recipient, amount):
    if coin == ETH: 
        trx_eth = create_tx(coin,account, recipient, amount)
        sign = account.signTransaction(trx_eth)
        result = w3.eth.sendRawTransaction(sign.rawTransaction)
        print(result.hex())
        return result.hex()
    elif coin == BTCTEST:
        trx_btctest= create_tx(coin,account,recipient,amount)
        sign_trx_btctest = account.sign_transaction(trx_btctest)
        from bit.network import NetworkAPI
        NetworkAPI.broadcast_tx_testnet(sign_trx_btctest)       
        return sign_trx_btctest`
        
  ### Executing Transaction
  
  
<img width="960" alt="2021-07-19 (6)" src="https://user-images.githubusercontent.com/78872373/126401296-9ede7a68-aaef-488a-96e2-cdda4a1b7bb4.png">

