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
  `
