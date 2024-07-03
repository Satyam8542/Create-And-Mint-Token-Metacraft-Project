# MyToken ERC20 Smart Contract

## Overview

This project is an implementation of an ERC20 token using Solidity and OpenZeppelin contracts. The smart contract includes functionality for minting, burning, and transferring tokens, with additional access control for minting operations.

## Features

- **Minting**: Only the contract owner can mint new tokens.
- **Burning**: Any token holder can burn (destroy) their own tokens.
- **Transferring**: Tokens can be transferred between addresses.
- **Allowance and TransferFrom**: Allows an approved address to transfer tokens on behalf of another address.

## Prerequisites

- **Remix IDE**: An online Solidity editor and compiler.
- **MetaMask**: A browser extension for interacting with the Ethereum network.

## Installation and Deployment

### Using Remix IDE

1. **Open Remix IDE**:
   - Navigate to [Remix IDE](https://remix.ethereum.org).

2. **Create a New File**:
   - In the Remix file explorer, click the "+" icon to create a new file.
   - Name the file `MyToken.sol`.

3. **Write the Smart Contract**:
   - Copy and paste the following code into `MyToken.sol`:
     ```solidity
     // SPDX-License-Identifier: MIT
     pragma solidity ^0.8.0;

     import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
     import "@openzeppelin/contracts/access/Ownable.sol";

     contract MyToken is ERC20, Ownable {
         constructor(string memory name, string memory symbol) ERC20(name, symbol) {
             // Initial mint to the owner
             _mint(msg.sender, 1000000 * 10 ** decimals());
         }

         function mint(address to, uint256 amount) public onlyOwner {
             _mint(to, amount);
         }

         function burn(uint256 amount) public {
             _burn(msg.sender, amount);
         }

         function transfer(address to, uint256 amount) public override returns (bool) {
             _transfer(_msgSender(), to, amount);
             return true;
         }

         function transferFrom(address sender, address recipient, uint256 amount) public override returns (bool) {
             _transfer(sender, recipient, amount);
             _approve(sender, _msgSender(), allowance(sender, _msgSender()) - amount);
             return true;
         }
     }
     ```

4. **Compile the Contract**:
   - Click on the "Solidity Compiler" tab on the left sidebar (second icon from the top).
   - Select the appropriate compiler version (e.g., `0.8.0`).
   - Click the "Compile MyToken.sol" button.

5. **Deploy the Contract**:
   - Click on the "Deploy & Run Transactions" tab on the left sidebar (third icon from the top).
   - Make sure "Environment" is set to "JavaScript VM (London)" for testing or "Injected Web3" for using MetaMask.
   - Ensure the contract `MyToken` is selected in the "Contract" dropdown.
   - In the "Deploy" section, provide the name and symbol for your token (e.g., `My Token` and `MTK`).
   - Click the "Deploy" button.
   - Wait for the deployment to complete.
   - The deployed contract will appear in the "Deployed Contracts" section.

## Interacting with the Token

1. **Mint Tokens**:
   - In the "Deployed Contracts" section, expand your deployed contract instance.
   - Find the `mint` function.
   - Provide the address to mint tokens to and the amount (e.g., `0xRecipientAddress`, `1000`).
   - Click the "transact" button. Ensure you are using the account that deployed the contract, as only the owner can mint tokens.

2. **Transfer Tokens**:
   - Find the `transfer` function.
   - Provide the recipient address and the amount to transfer (e.g., `0xRecipientAddress`, `500`).
   - Click the "transact" button.

3. **Burn Tokens**:
   - Find the `burn` function.
   - Provide the amount to burn (e.g., `200`).
   - Click the "transact" button. Ensure you are using the account that holds the tokens to burn.

4. **Check Balances**:
   - Find the `balanceOf` function.
   - Provide an address to check its balance.
   - Click the "call" button to see the balance of tokens for that address.

## Notes

- **Testing**: Use the JavaScript VM environment in Remix for testing your contract before deploying it to a testnet or mainnet. This environment is fast and does not require real Ether.
- **Deployment to Testnet/Mainnet**: When you're ready to deploy to a testnet (e.g., Rinkeby) or mainnet, switch the environment to "Injected Web3" and connect your MetaMask wallet. Ensure you have enough Ether in your wallet for gas fees.
- **Security**: Make sure to review and test your contract thoroughly, especially when dealing with real assets on mainnet.

## License

This project is licensed under the MIT License.
