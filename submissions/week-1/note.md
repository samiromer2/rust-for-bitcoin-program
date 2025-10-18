# Week 1: Bitcoin Fundamentals - Assignment Notes

## Introduction

This assignment aimed to provide a foundational understanding of Bitcoin by interacting with a local Bitcoin node in regtest mode.  The goal was to learn about block creation, transaction management, UTXOs, and the overall Bitcoin workflow.

## 1. Node Setup Verification

*   **Command:** `./bitcoin-cli -regtest getblockchaininfo`
*   **Output (Example):**
    ```json
    {
      "chain": "regtest",
      "blocks": 10,  // Initial block height in regtest mode
      ... other details ...
    }
    ```
*   **Explanation:**  The `chain` value being "regtest" confirms that the Bitcoin Core node is running in regtest mode, which provides a private and isolated blockchain environment for testing. The `blocks` value indicates the current block height of the regtest chain.

## 2. Block Generation

*   **Command:** `./bitcoin-cli -regtest -rpcwallet=sender generatetoaddress 101 bcrt1qlcxyx7ar4j3ew4znhly0380x7n7ra2j3efvhdw`
*   **Command:** `./bitcoin-cli -regtest getblockcount`
*   **Output (Example):** 111
*   **Explanation:**  The block count increased by 101, confirming that the `generatetoaddress` command successfully created the specified number of blocks.

## 3. Blockchain Exploration

*   **Command:** `./bitcoin-cli -regtest getbestblockhash`
*   **Output (Example):** `0000000000000000000000000000000000000000000000000000000000000001`
*   **Command:** `./bitcoin-cli -regtest getblock <blockhash>` (replace `<blockhash>` with the output from `getbestblockhash`)
*   **Explanation:**  `getbestblockhash` retrieves the hash of the most recent block on the blockchain. `getblock` provides detailed information about a specific block, including its timestamp, previous block hash, Merkle root, and transaction data.
*   **Command:** `./bitcoin-cli -regtest getblockheader <blockhash>` (replace `<blockhash>` with the output from `getbestblockhash`)
*   **Explanation:**  `getblockheader` retrieves just the header of a block, which contains metadata like timestamp and previous block hash.

## 4. Wallet Management

*   **Command:** `./bitcoin-cli -regtest createwallet "testwallet"`
*   **Explanation:** Creates a new wallet named "testwallet".
*   **Command:** `./bitcoin-cli -regtest getnewaddress` (repeated several times)
*   **Output (Example):** `mpx43q68w9y7x2z5` (and several other addresses)
*   **Explanation:**  Generates new Bitcoin addresses for the "testwallet". Each address is a unique identifier that can receive and send Bitcoin.
*   **Command:** `./bitcoin-cli -regtest listaddresses`
*   **Explanation:** Lists all the addresses associated with the "testwallet".

## 5. Transaction Sending and Tracking

*   **Command:** `./bitcoin-cli -regtest sendtoaddress <recipient_address> 0.1` (replace `<recipient_address>` with a newly generated address)
*   **Explanation:** Sends 0.1 BTC to the specified recipient address.
*   **Command:** `./bitcoin-cli -regtest gettransaction <txid>` (replace `<txid>` with the transaction ID returned by `sendtoaddress`)
*   **Explanation:** Retrieves detailed information about a specific transaction, including its inputs, outputs, and confirmation status.

## 6. UTXO Inspection

*   **Command:** `./bitcoin-cli -regtest listunspent`
*   **Explanation:** Lists all unspent transaction outputs (UTXOs) associated with the wallet.
*   **Example UTXO Details:**
    *   `txid`: `a1b2c3d4e5f6...`
    *   `vout`: 0
    *   `amount`: 10000 (satoshis)
*   **Explanation:** UTXOs represent the spendable Bitcoin associated with a wallet.  `txid` is the transaction ID that created this output, `vout` is the index of the output within that transaction, and `amount` is the amount of Bitcoin in satoshis.

## 7. Raw Transaction Decoding

*   **Command:** `./bitcoin-cli -regtest getrawtransaction <txid> true` (replace `<txid>` with a transaction ID)
*   **Explanation:** Retrieves the raw transaction data in a human-readable format.  The `true` argument tells Bitcoin Core to decode the transaction into JSON.
*   **Observation:** The output is a complex JSON structure containing details about the transaction's inputs (scriptSig, previous outputs) and outputs (amount, scriptPubKey).

## 8. Payment Workflow Simulation

*   **Wallets:** `sender` and `receiver` created using `./bitcoin-cli -regtest createwallet`.
*   **Addresses:** Addresses generated for both wallets.
*   **Command:** `./bitcoin-cli -regtest sendtoaddress <receiver_address> 0.1` (from `sender`)
*   **Command:** `./bitcoin-cli -regtest generatetoaddress 1 "..."` (from `sender`)
*   **Verification:**  Used `listunspent` on the `receiver` wallet to confirm that it now contains 0.1 BTC.
*   **Explanation:**  Simulated a complete payment workflow by sending coins from one wallet to another and then mining a block to confirm the transaction.

