# WalletController Documentation

The `WalletController` is a Symfony controller responsible for handling wallet-related operations for users. It supports both testnet and mainnet environments and integrates with external services via cURL requests. This controller facilitates wallet creation, token transfers, balance inquiries, and swap operations.

## Table of Contents

1. [Overview](#overview)
2. [Dependencies and Imports](#dependencies-and-imports)
3. [Class Properties and Constructor](#class-properties-and-constructor)
4. [Endpoints and Methods](#endpoints-and-methods)
    - [Testnet Endpoints](#testnet-endpoints)
    - [Mainnet Endpoints](#mainnet-endpoints)
    - [Swap APIs](#swap-apis)
    - [Additional Methods](#additional-methods)
5. [Summary](#summary)
6. [Contributing](#contributing)
7. [License](#license)

## Overview

The `WalletController` handles various wallet-related operations such as creating wallets, transferring tokens, fetching wallet balances, and more. It is built to work with both testnet and mainnet environments, ensuring that wallet operations are securely managed and executed. Additionally, it provides swap API endpoints for price discovery, quoting, and transaction execution.

## Dependencies and Imports

The controller is part of the `App\Controller` namespace and imports multiple classes and services, including:

- **Entity Classes:** `User`
- **Enum Classes:** `ProjectList`
- **Service Classes:** `Authenticator`, `Wallet`, `PushNotificationService`, `PostHelper`
- **Repository Classes:** `UserRepository`, `TokenMetadataRepository`, `SolanaWalletRepository`
- **Symfony Components:** `Request`, `Response`, `Route`, `LoggerInterface`, `EventDispatcherInterface`
- **External Controllers:** `SolanaController`
- **Message Classes:** `DailyLoginTask`

## Class Properties and Constructor

The class defines several constants and properties including:
- Response per page limits, token addresses, developer secrets, API URLs, and contract addresses.
- Dependencies are injected via the constructor, ensuring a modular and testable design.

## Endpoints and Methods

### Testnet Endpoints

1. **Create Wallet**  
   - **Route:** `/testnet/wallet/create`  
   - **Method:** POST  
   - **Description:** Creates a new wallet on the testnet.  
   - **Process:**  
     - Authenticates the user.
     - Validates input parameters.
     - Generates an owner ID if not present.
     - Sends a request to the testnet API to create a wallet.
     - Updates the user's wallet address and owner ID in the database.
   - **Request:**  
     - **Headers:** Authentication token  
     - **Body:**  
       ```json
       { "name": "<wallet_name>", "user_pin": "<user_pin>", "network": "<network>" }
       ```
   - **Response:**  
     - **Success:**  
       ```json
       { "status": "success", "message": "Wallet created successfully", "data": <wallet_data> }
       ```
     - **Failure:** Returns an error message.

2. **Wallet Activity**  
   - **Route:** `/testnet/activity`  
   - **Method:** GET  
   - **Description:** Retrieves the activity of a wallet on the testnet.  
   - **Process:**  
     - Authenticates the user.
     - Validates input parameters.
     - Sends a request to the testnet API for wallet activity.
     - Returns activity data.
   - **Request:**  
     - **Headers:** Authentication token  
     - **Query Parameters:** `network`, `wallet_address`, `page`, `page_size`, `last_transaction_hash`
   - **Response:**  
     - **Success:**  
       ```json
       { "status": "success", "message": "Activity retrieved successfully", "data": <activity_data> }
       ```
     - **Failure:** Returns an error message.

3. **Transfer Tokens**  
   - **Route:** `/testnet/transfer-tokens`  
   - **Method:** POST  
   - **Description:** Transfers tokens on the testnet.  
   - **Process:**  
     - Authenticates the user.
     - Validates input parameters.
     - Sends a request to the testnet API to transfer tokens.
     - Updates user activity and sends push notifications.
   - **Request:**  
     - **Headers:** Authentication token  
     - **Body:**  
       ```json
       { "amount": "<amount>", "from": "<from_address>", "network": "<network>", "recipient": "<recipient_address>", "token": "<token>", "user_pin": "<user_pin>" }
       ```
   - **Response:**  
     - **Success:**  
       ```json
       { "status": "success", "message": "Token transfer successful", "data": <transfer_data> }
       ```
     - **Failure:** Returns an error message.

4. **Get Wallet by Address**  
   - **Route:** `/testnet/wallet/get_by_address`  
   - **Method:** GET  
   - **Description:** Retrieves a wallet by its address on the testnet.  
   - **Process:**  
     - Authenticates the user.
     - Validates input parameters.
     - Fetches the wallet data via the testnet API.
   - **Request:**  
     - **Headers:** Authentication token  
     - **Query Parameters:** `network`, `wallet_address`
   - **Response:**  
     - **Success:**  
       ```json
       { "status": "success", "message": "Wallet fetch successfully", "data": <wallet_data> }
       ```
     - **Failure:** Returns an error message.

5. **Change PIN**  
   - **Route:** `/testnet/wallet/change_pin`  
   - **Method:** POST  
   - **Description:** Changes the user PIN for a wallet on the testnet.  
   - **Process:**  
     - Authenticates the user.
     - Validates input parameters.
     - Sends a request to change the user PIN.
   - **Request:**  
     - **Headers:** Authentication token  
     - **Body:**  
       ```json
       { "wallet_address": "<wallet_address>", "new_user_pin": "<new_user_pin>", "old_user_pin": "<old_user_pin>", "network": "<network>" }
       ```
   - **Response:**  
     - **Success:**  
       ```json
       { "status": "success", "message": "Pin changed successfully", "data": <response_data> }
       ```
     - **Failure:** Returns an error message.

6. **Wallet Balance**  
   - **Route:** `/testnet/wallet/balance`  
   - **Method:** GET  
   - **Description:** Retrieves the balance of a wallet on the testnet.  
   - **Process:**  
     - Authenticates the user.
     - Validates input parameters.
     - Fetches balance data from the testnet API.
   - **Request:**  
     - **Headers:** Authentication token  
     - **Query Parameters:** `network`, `wallet_address`
   - **Response:**  
     - **Success:**  
       ```json
       { "status": "success", "message": "balance fetch successfully", "data": <balance_data> }
       ```
     - **Failure:** Returns an error message.

7. **Get Wallet by ID**  
   - **Route:** `/testnet/wallet/id`  
   - **Method:** GET  
   - **Description:** Retrieves a wallet by its ID on the testnet.  
   - **Process:**  
     - Authenticates the user.
     - Validates input parameters.
     - Fetches the wallet via the testnet API.
   - **Request:**  
     - **Headers:** Authentication token  
     - **Query Parameters:** `network`, `id`
   - **Response:**  
     - **Success:**  
       ```json
       { "status": "success", "message": "Wallet fetch successfully", "data": <wallet_data> }
       ```
     - **Failure:** Returns an error message.

8. **Export Wallet**  
   - **Route:** `/testnet/wallet/export`  
   - **Method:** POST  
   - **Description:** Exports a wallet on the testnet.  
   - **Process:**  
     - Authenticates the user.
     - Validates input parameters.
     - Sends a request to export the wallet.
   - **Request:**  
     - **Headers:** Authentication token  
     - **Body:**  
       ```json
       { "wallet_address": "<wallet_address>", "user_pin": "<user_pin>", "network": "<network>" }
       ```
   - **Response:**  
     - **Success:**  
       ```json
       { "status": "success", "message": "Wallet exported successfully", "data": <response_data> }
       ```
     - **Failure:** Returns an error message.

9. **Get Wallet by Owner ID**  
   - **Route:** `/testnet/wallet/get_by_owner_id`  
   - **Method:** GET  
   - **Description:** Retrieves a wallet by its owner ID on the testnet.  
   - **Process:**  
     - Authenticates the user.
     - Validates input parameters.
     - Fetches wallet data via the testnet API.
   - **Request:**  
     - **Headers:** Authentication token  
     - **Query Parameters:** `network`, `owner_id`
   - **Response:**  
     - **Success:**  
       ```json
       { "status": "success", "message": "Wallet fetched successfully", "data": <wallet_data> }
       ```
     - **Failure:** Returns an error message.

### Mainnet Endpoints

1. **Create Wallet**  
   - **Route:** `/mainnet/wallet/create`  
   - **Method:** POST  
   - **Description:** Creates a new wallet on the mainnet.  
   - **Process:** Similar to the testnet wallet creation process.
   - **Request:**  
     - **Headers:** Authentication token  
     - **Body:**  
       ```json
       { "name": "<wallet_name>", "user_pin": "<user_pin>", "network": "<network>" }
       ```
   - **Response:**  
     - **Success:**  
       ```json
       { "status": "success", "message": "Wallet created successfully", "data": <wallet_data> }
       ```
     - **Failure:** Returns an error message.

2. **Wallet Activity**  
   - **Route:** `/mainnet/activity`  
   - **Method:** GET  
   - **Description:** Retrieves wallet activity on the mainnet.  
   - **Request:**  
     - **Headers:** Authentication token  
     - **Query Parameters:** `network`, `wallet_address`, `page`, `page_size`, `last_transaction_hash`
   - **Response:**  
     - **Success:**  
       ```json
       { "status": "success", "message": "Activity retrieved successfully", "data": <activity_data> }
       ```
     - **Failure:** Returns an error message.

3. **Transfer Tokens**  
   - **Route:** `/mainnet/transfer-tokens`  
   - **Method:** POST  
   - **Description:** Transfers tokens on the mainnet.  
   - **Process:** Similar to the testnet transfer process.
   - **Request:**  
     - **Headers:** Authentication token  
     - **Body:**  
       ```json
       { "amount": "<amount>", "from": "<from_address>", "network": "<network>", "recipient": "<recipient_address>", "token": "<token>", "user_pin": "<user_pin>" }
       ```
   - **Response:**  
     - **Success:**  
       ```json
       { "status": "success", "message": "Token transfer successful", "data": <transfer_data> }
       ```
     - **Failure:** Returns an error message.

4. **Get Wallet by Address**  
   - **Route:** `/mainnet/wallet/get_by_address`  
   - **Method:** GET  
   - **Description:** Retrieves a wallet by its address on the mainnet.  
   - **Request:**  
     - **Headers:** Authentication token  
     - **Query Parameters:** `network`, `wallet_address`
   - **Response:**  
     - **Success:**  
       ```json
       { "status": "success", "message": "Wallet fetch successfully", "data": <wallet_data> }
       ```
     - **Failure:** Returns an error message.

5. **Change PIN**  
   - **Route:** `/mainnet/wallet/change_pin`  
   - **Method:** POST  
   - **Description:** Changes the user PIN for a wallet on the mainnet.  
   - **Request:**  
     - **Headers:** Authentication token  
     - **Body:**  
       ```json
       { "wallet_address": "<wallet_address>", "new_user_pin": "<new_user_pin>", "old_user_pin": "<old_user_pin>", "network": "<network>" }
       ```
   - **Response:**  
     - **Success:**  
       ```json
       { "status": "success", "message": "Pin changed successfully", "data": <response_data> }
       ```
     - **Failure:** Returns an error message.

6. **Wallet Balance**  
   - **Route:** `/mainnet/wallet/balance`  
   - **Method:** GET  
   - **Description:** Retrieves the balance of a wallet on the mainnet.  
   - **Request:**  
     - **Headers:** Authentication token  
     - **Query Parameters:** `network`, `wallet_address`
   - **Response:**  
     - **Success:**  
       ```json
       { "status": "success", "message": "balance fetch successfully", "data": <balance_data> }
       ```
     - **Failure:** Returns an error message.

7. **Get Wallet by ID**  
   - **Route:** `/mainnet/wallet/id`  
   - **Method:** GET  
   - **Description:** Retrieves a wallet by its ID on the mainnet.  
   - **Request:**  
     - **Headers:** Authentication token  
     - **Query Parameters:** `network`, `id`
   - **Response:**  
     - **Success:**  
       ```json
       { "status": "success", "message": "Wallet fetch successfully", "data": <wallet_data> }
       ```
     - **Failure:** Returns an error message.

8. **Export Wallet**  
   - **Route:** `/mainnet/wallet/export`  
   - **Method:** POST  
   - **Description:** Exports a wallet on the mainnet.  
   - **Request:**  
     - **Headers:** Authentication token  
     - **Body:**  
       ```json
       { "wallet_address": "<wallet_address>", "user_pin": "<user_pin>", "network": "<network>" }
       ```
   - **Response:**  
     - **Success:**  
       ```json
       { "status": "success", "message": "Wallet exported successfully", "data": <response_data> }
       ```
     - **Failure:** Returns an error message.

9. **Get Wallet by Owner ID**  
   - **Route:** `/mainnet/wallet/get_by_owner_id`  
   - **Method:** GET  
   - **Description:** Retrieves a wallet by its owner ID on the mainnet.  
   - **Request:**  
     - **Headers:** Authentication token  
     - **Query Parameters:** `network`, `owner_id`
   - **Response:**  
     - **Success:**  
       ```json
       { "status": "success", "message": "Wallet fetched successfully", "data": <wallet_data> }
       ```
     - **Failure:** Returns an error message.

### Swap APIs

1. **Get Swap Price**  
   - **Route:** `/swap/price`  
   - **Method:** GET  
   - **Description:** Provides pricing information for a swap transaction.  
   - **Process:**  
     - Authenticates the user.
     - Validates input parameters.
     - Sends a request to the swap API.
   - **Request:**  
     - **Headers:** Authentication token  
     - **Query Parameters:** `sellToken`, `buyToken`, `sellAmount`, `network`
   - **Response:**  
     - **Success:**  
       ```json
       { "status": "success", "message": "Price fetched successfully", "data": <price_data> }
       ```
     - **Failure:** Returns an error message.

2. **Get Swap Quote**  
   - **Route:** `/swap/quote`  
   - **Method:** GET  
   - **Description:** Provides a quote for a swap transaction.  
   - **Request:**  
     - **Headers:** Authentication token  
     - **Query Parameters:** `sellToken`, `buyToken`, `sellAmount`, `network`
   - **Response:**  
     - **Success:**  
       ```json
       { "status": "success", "message": "Quote fetched successfully", "data": <quote_data> }
       ```
     - **Failure:** Returns an error message.

3. **Get Swap Sources**  
   - **Route:** `/swap/sources`  
   - **Method:** GET  
   - **Description:** Returns liquidity sources enabled for a specific chain.  
   - **Request:**  
     - **Headers:** Authentication token  
     - **Query Parameters:** `network`
   - **Response:**  
     - **Success:**  
       ```json
       { "status": "success", "message": "Sources fetched successfully", "data": <sources_data> }
       ```
     - **Failure:** Returns an error message.

4. **Send Swap Transaction**  
   - **Route:** `/swap/send/transaction`  
   - **Method:** POST  
   - **Description:** Executes a swap transaction.  
   - **Process:**  
     - Authenticates the user.
     - Validates input parameters.
     - Sends a request to the swap API to execute the transaction.
   - **Request:**  
     - **Headers:** Authentication token  
     - **Body:**  
       ```json
       { "network": "<network>", "signer_wallet_address": "<signer_wallet_address>", "to": "<to_address>", "data": "<data>", "value": "<value>", "user_pin": "<user_pin>", "sell_token_address": "<sell_token_address>", "priority_fee": "<priority_fee>" }
       ```
   - **Response:**  
     - **Success:**  
       ```json
       { "status": "success", "message": "Swap successfully", "data": <transaction_data> }
       ```
     - **Failure:** Returns an error message.

### Additional Methods

- **getUserWalletAddress:** Retrieves the user's wallet address on the mainnet.
- **getTokensMetadata:** Fetches token metadata from the Alchemy API.
- **getSolanaTokenByContractAddress:** Fetches Solana token data by contract address.
- **getTokenImageUrlByAddress:** Retrieves the token image URL using the contract address.
- **storeCoingeckoTokens:** Stores tokens retrieved from CoinGecko.

## Summary

The `WalletController` class provides a robust API interface for managing wallet operations across testnet and mainnet environments. With endpoints for wallet creation, token transfers, balance inquiries, and swap functionalities, it ensures secure and efficient wallet management while integrating seamlessly with external services.

