# Token Transfer API (Testnet)

This Symfony controller endpoint facilitates the transfer of tokens on a test network. It is designed to process token transfer requests by validating the user, checking required parameters, and handling transfers differently based on the specified network.

## Endpoint Overview

- **URL:** `/testnet/transfer-tokens`
- **Method:** `POST`
- **Purpose:** To allow an authenticated user to transfer tokens on a test network.
- **Networks Supported:** 
  - **Solana:** Uses a dedicated method for handling Solana transfers.
  - **Other Networks:** Validates the recipient’s wallet and forwards the request to an external token transfer service using cURL.

## How It Works

1. **Authentication:**
   - The controller starts by authenticating the incoming request.
   - If the user is not authenticated, the request is redirected to a `forbidden` route.

2. **Project Verification:**
   - After authentication, the controller retrieves the user information and the associated `appName`.
   - It checks if the `appName` is allowed using a project list validation.

3. **Parameter Extraction and Validation:**
   - The required parameters are extracted from the request:
     - `amount`: The number of tokens to transfer.
     - `from`: The sender's wallet address.
     - `network`: The blockchain network (e.g., "Solana", "Ethereum").
     - `recipient`: The recipient's wallet address.
     - `token`: The token identifier.
     - `user_pin`: A security PIN provided by the user.
   - A constant `DEVELOPER_SECRET` is also included automatically.
   - If any mandatory parameter is missing, a JSON response with an error is returned.

4. **Network-Specific Handling:**
   - **For Solana:**
     - If the `network` parameter equals `"Solana"`, the controller calls a dedicated method `transferSolOnTestnet` from the Solana controller.
   - **For Other Networks:**
     - It verifies that the recipient exists by checking the wallet address in the repository.
     - If the recipient does not have a wallet, an error response is returned.
     - The controller then makes a cURL POST request to an external testnet token transfer URL, sending all the gathered request data.
     - It sets necessary headers such as `X-WalletKit-Project-ID`, `Authorization`, and `Content-Type`.

5. **Response Handling:**
   - The external service's response is checked:
     - On success (HTTP status 200 or 201), it validates that a `transaction_id` is included.
     - If the `transaction_id` is missing, an error is logged and returned.
     - When a valid `transaction_id` is received, the controller logs the activity (including push notifications) and returns a success JSON response with the transaction details.
   - If the cURL request fails or returns an error status, an appropriate error JSON response is generated.

## Testing with Postman

To test the `/testnet/transfer-tokens` endpoint using Postman, follow these steps:

1. **Setup the Request:**
   - **Method:** `POST`
   - **URL:** `http://<your-domain>/testnet/transfer-tokens`
     - Replace `<your-domain>` with your actual domain or `localhost` if testing locally.

2. **Headers:**
   - `Content-Type: application/json`
   - Include any required authentication headers (e.g., `Authorization` token) that your API expects.

3. **Body:**
   - Select the **raw** option and choose **JSON** format.
   - Provide a JSON payload with the required parameters. For example:
     ```json
     {
       "amount": "100",
       "from": "0xSenderWalletAddress",
       "network": "Ethereum",
       "recipient": "0xRecipientWalletAddress",
       "token": "TOKEN_SYMBOL",
       "user_pin": "1234"
     }
     ```

4. **Send the Request:**
   - Click the **Send** button.
   - Check the response:
     - **Success:** You will receive a JSON response with a status of `success`, a confirmation message, and transaction data.
     - **Error:** In case of an error (e.g., missing parameters, invalid wallet, or network issues), an error message and appropriate HTTP status code will be returned.

## Logging and Notifications

- The controller uses a logger to record the HTTP status code and any errors encountered during the token transfer process.
- On successful transfers, it also prepares and stores user activity data (used for push notifications) indicating the transfer details.

## Conclusion

This endpoint offers a secure and robust method for transferring tokens on a test network. It includes comprehensive authentication, validation, and error handling while providing different processing logic based on the blockchain network involved.

---
