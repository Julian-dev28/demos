This is a simple README file for the repository that covers `Getting Started with Stellar and Soroban.

## Getting Started with Stellar and Soroban

After intros, you would go to slide 2.Okashi,
The speaker would do a quick overview of how Oksahi works and how it is used for Stellar smart contracts, while encouraging the audience to follow along through the QR code.

The presenter can start with the following code:

```rust
#![no_std]

use soroban_sdk::{contract, contractimpl, Env, Symbol, String, symbol_short};

const TITLE: Symbol = symbol_short!("TITLE");

#[contract]
pub struct TitleContract;

#[contractimpl]
impl TitleContract {}
```

The speaker would then explain the existing code and how it works, then explain how to create a setter and getter for the `TITLE` variable.

```rust
// Soroban uses no standard library so we need to declare it at the top of the file
// This is done to optimize the size of the contract
#![no_std]

// Import the necessary dependencies from the Soroban SDK
use soroban_sdk::{contract, contractimpl, Env, Symbol, String, symbol_short};

// Declare a constant variable called TITLE and assign it a Symbol value
const TITLE: Symbol = symbol_short!("TITLE");

// Declare a contract called TitleContract
// This is where the contract functions will be implemented
#[contract]
pub struct TitleContract;

// Implement the contract
// This is where the functions that will be used in the contract are declared
#[contractimpl]
impl TitleContract {
    // Declare a function called set_title that takes in an environment and a title
    pub fn set_title(env:Env, title:String){
        // Set the title in the storage
        // The storage is a key-value store that is used to store data
        env.storage().instance().set(&TITLE,&title);
    }
    // Declare a function called get_title that takes in an environment
    pub fn get_title(env:Env) -> String {
        // Get the title from the storage
        // If the title is not found, return a default message
        env.storage().instance().get(&TITLE).unwrap_or(String::from_str(&env, "Default Message"))
    }
}
```

The key things to cover are:
- How to declare a constant variable in Soroban
- How to declare functions in Soroban
- How to use the storage in Soroban with the `env.storage().instance().set` and `env.storage().instance().get` functions

The speaker would then explain how to deploy the contract and interact with it using the Stellar network.

## Switching to the Stellar-CLI

After the initial overview of Okashi, the speaker would switch to the Stellar-CLI and explain how to deploy the contract and interact with it using the Stellar network.

The speaker would start by explaining how to create a new identity using the Stellar-CLI:

```bash
stellar keys generate new_kp --network testnet
```

The speaker would then explain how to show the key pair:

```bash
stellar keys address new_kp && stellar keys show new_kp
```

::: note
The speaker would explain that the `generate` command creates and funds a new account on the testnet, while the `address` and `show` command displays the public and secret keys of the account respesctively.
:::

The speaker would then explain how to create a new bootstrapped soroban project using the Stellar-CLI:

```bash
stellar contract init hello-world
```

The speaker would then explain how to deploy the contract to the Stellar network using the Stellar-CLI:

First they would copy and paste the `TitleContract` contract code into the `src/lib.rs` file in the `hello-world` directory.

Then they would compile the contract into wasm:

```bash
stellar contract build
```

Then they would deploy the contract to the Stellar network using the previously created identity:

```bash
stellar contract deploy --network testnet --source new_kp --wasm target/wasm32-unknown-unknown/release/hello_world.wasm
```

The speaker could then recap stellar addresses and contracts(Addresses begin with G Contracts begin with C):

```bash
<Deployed Contract ID>
```

```bash
stellar keys address new_kp && stellar keys show new_kp
<Public Key>
<Private Key>
```

The speaker would then explain how to call a function using the Stellar-CLI:

`set-title` function:

```bash
stellar contract invoke --id <Contract Id> --network testnet --source new_kp -- set-title --title 'hello world'
```

`get-title` function:

```bash
stellar contract invoke --id <Contract Id> --network testnet --source new_kp -- get-title
```

## Running the set-title Dapp

The speaker would then explain how to run the `set-title` Dapp ussing the command:
    
```bash
npm create soroban-dapp@latest
```

The user will be prompted to enter a project name. This will create a new directory with the project name and the necessary files to run the Dapp. 

::: note
It's better to use yarn instead of npm to install the dependencies.
:::

The speaker would tell the user to update the `env.example` file with a PK and the Stellar network endpoint:

```bash
# Stellar accounts Secret Keys
# Example of a secret key: SCYU4RG6GWRZXLCMC6L5OZUQJDBSJFUIZ7MMLUJ3T3VZADA6HADJAKRX
ADMIN_SECRET_KEY=<Secret Key>

# RPC Setup
MAINNET_RPC_URL=https://soroban-testnet.stellar.org
```

The speaker would then explain how to run the Dapp using the following commands:

```bash
cd contracts/greeting
stellar contract build
cd ../
yarn
yarn deploy testnet greeting
```

The speaker would then explain how to interact with the Dapp using the following commands:

```bash
# take the user back to the root directory
cd ../
# run the Dapp in development mode
yarn dev
```

The speaker would then explain how to interact with the Dapp using the browser and the Stellar network.
1. Connect a wallet
2. Set the title
3. See the change reflected in the Dapp

## Conclusion

The speaker would then conclude the presentation by summarizing the key points covered and encouraging the audience to explore Stellar and Soroban further by showing slide 4.conlusion
