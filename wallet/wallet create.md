# Symfony Wallet Creation API Endpoint

This document provides an overview of the Symfony API endpoint used to create a wallet in the testnet environment. It explains the code functionality, details the API endpoint, and provides step-by-step instructions on how to test it using Postman.

## Table of Contents

- [Overview](#overview)
- [Endpoint Description](#endpoint-description)
- [Code Explanation](#code-explanation)
- [Testing with Postman](#testing-with-postman)
- [Troubleshooting & Notes](#troubleshooting--notes)

## Overview

The `/testnet/wallet/create` endpoint is designed to create a wallet for authenticated users on a testnet. It validates user credentials and project permissions, extracts request parameters, and handles wallet creation by either delegating to a specific controller (for Solana) or making an external API call via cURL for other networks. On success, it saves the wallet data to the database and returns a success response.

## Endpoint Description

- **URL:** `/testnet/wallet/create`
- **HTTP Method:** `POST`
- **Authentication:** Custom authentication is required (the endpoint uses an authenticator to check the logged-in user).
- **Request Parameters:**
  - `name`: The desired wallet name.
  - `user_pin`: The user’s PIN (must be at least 6 characters).
  - `network`: The blockchain network (e.g., `Solana`, `Ethereum`, etc.).
- **Responses:**
  - **Success (HTTP 200/201):** Returns a JSON with a success status, message, and wallet data.
  - **Error (HTTP 400/500):** Returns a JSON with an error message (e.g., missing parameters, invalid project, cURL errors).

## Code Explanation

Below is the annotated Symfony controller method that implements the endpoint:

```php
#[Route('/testnet/wallet/create', name: 'app.wallet.testnet', methods: ['POST'])]
public function createWalletTest(Request $request): Response 
{
    // Authenticate the user; if authentication fails, redirect to the forbidden route.
    $userInfo = $this->authenticator->authenticate($request);    
    if(empty($userInfo)) {
        return $this->redirectToRoute('forbidden');
    }
    
    // Extract the user and application name from the authenticated information.
    $user = $userInfo->getUser();
    $appName = $userInfo->getAppName();
    if(!ProjectList::isAllowedProject($appName)) {
        return $this->json([
            'status' => 'failed',
            'message' => 'Invalid app_name parameter! Kindly contact Empowerverse team'
        ], 400);
    }

    // Define a default control mode and retrieve request parameters.
    $control_mode = 'user';
    $name = $request->get('name');
    $user_pin = $request->get('user_pin');
    $network = $request->get('network');

    // Validate required parameters.
    if (empty($control_mode) || empty($network) || empty($name) || empty($user_pin) || strlen($user_pin) < 6) {
        return $this->json([
            'status' => 'error',
            'message' => 'Invalid request',
        ], 400);
    }
    
    // Determine or generate the ownerID.
    if ($user->getOwnerID()) {
        $ownerID = $this->walletService->getSecureData($user->getOwnerID());
    } else {
        $ownerID = $this->walletService->generateOwnerID();
    }

    // Prepare data for wallet creation.
    $receivedRequest = [
        'control_mode' => $control_mode,
        'name' => $name,
        'user_pin' => $user_pin,
        'network' => $network,
        'owner_id' => $ownerID
    ];
    
    // Special handling for the Solana network.
    if ($network === 'Solana') {
        $data = $this->solanaController->createWalletOnTestnet($user, $receivedRequest);
        return $data;
    }

    // For other networks, call the external testnet API using cURL.
    $curl = curl_init();
    curl_setopt_array($curl, [
        CURLOPT_URL => self::TESTNET_URL . "/wallets",
        CURLOPT_RETURNTRANSFER => true,
        CURLOPT_ENCODING => "",
        CURLOPT_MAXREDIRS => 10,
        CURLOPT_TIMEOUT => 30,
        CURLOPT_HTTP_VERSION => CURL_HTTP_VERSION_1_1,
        CURLOPT_CUSTOMREQUEST => "POST",
        CURLOPT_POSTFIELDS => json_encode($receivedRequest),
        CURLOPT_HTTPHEADER => [
            "X-WalletKit-Project-ID: $this->testnetProjectId",
            "Authorization: $this->testnetAuthorizationToken",
            "Content-Type: application/json"
        ],
    ]);

    $response = curl_exec($curl);
    $err = curl_error($curl);
    curl_close($curl);

    // Handle cURL errors.
    if ($err) {
        return $this->json([
            'status' => 'error',
            'message' => "cURL Error #: $err",
        ], 500);
    } else {
        $responseData = json_decode($response, true);
        $httpStatusCode = curl_getinfo($curl, CURLINFO_HTTP_CODE);
        $this->logger->info("WalletKit: CREATE-WALLET statusCode: ", [$httpStatusCode]);

        // On successful wallet creation, update the user record and persist changes.
        if ($httpStatusCode === 200 || $httpStatusCode === 201) {
            $ownerID = $this->walletService->secureData($ownerID);
            if ($user->getWalletAddress() === "") {
                $result = $user->setWalletAddress($responseData['address']);
            }
            $result = $user->setOwnerID($ownerID);
            $entityManager = $this->getDoctrine()->getManager();
            $entityManager->persist($result);
            $entityManager->flush();

            return $this->json([
                'status' => 'success',
                'message' => 'Wallet created successfully',
                'data' => $responseData
            ], 200);
        } else {
            return $this->json([
                'status' => 'error',
                'message' => $responseData,
            ], $httpStatusCode);
        }
    }
}
