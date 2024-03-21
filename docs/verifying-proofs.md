# Verifying Proofs API Reference

## Overview

This section of the documentation details the REST API exposed by the Verifier tool, a system designed for the verification of proofs of file ownership and integrity. This tool is especially relevant for ensuring the authenticity of proofs of replication, a crucial aspect of decentralized file sharing systems. It leverages a CGo/Rust implementation based on the Filecoin proof library.

---

## Setup and Configuration

1. Install Rust and the required build tools on your system.
2. Clone the Verifier repository from GitHub to your local machine.
3. Follow the provided instructions to build the Verifier tool.
4. Start the Verifier service to expose the REST API endpoint.

Ensure your firewall and network settings allow API requests on the designated port.

---

## API Usage

### Endpoint

`POST /verify-seal`

### Description

This endpoint accepts proof data and verifies the integrity and ownership of files within the decentralized network. It specifically checks the validity of proofs of replication.

### Request Parameters

- **Body** (application/json):
  - `SectorID` (object): Contains the `Miner` (number) and `Number` (number) identifying the sector.
  - `SealedCID` (string): CID of the sealed sector.
  - `SealProof` (string): Type of the seal proof.
  - `Proof` (binary): The actual proof data.
  - `DealIDs` (array of numbers, optional): Array of deal IDs included in the sector.
  - `Randomness` (binary): Randomness used in the sealing process.
  - `InteractiveRandomness` (binary): Randomness used in the interactive phase of the sealing process.
  - `UnsealedCID` (string): CID of the unsealed sector.

### Success Response

- **Code**: `200 OK`
- **Content**:
  ```json
  {
    "isValid": true,
    "message": "Proof verification succeeded."
  }
  ```

### Error Response

- **Code**: `400 Bad Request`
- **Content**:
  ```json
  {
    "error": "Invalid proof data provided."
  }
  ```

- **Code**: `500 Internal Server Error`
- **Content**:
  ```json
  {
    "error": "Proof verification failed due to server error."
  }
  ```

### Example Request

```bash
curl -X POST "http://localhost:8080/verify-seal" \
     -H "Content-Type: application/json" \
     -d '{
           "SectorID": {"Miner": 101010, "Number": 2},
           "SealedCID": "bafkreib...",
           "SealProof": "stackedProofV1",
           "Proof": "eW91cnByb29maGVyZQ==",
           "DealIDs": [123456, 789012],
           "Randomness": "YmxpbmRyYW5kb21uZXNz",
           "InteractiveRandomness": "aW50ZXJhY3RpdmVyYW5kb21uZXNz",
           "UnsealedCID": "bafkreib..."
         }'
```

### Interpreting the Results

Upon a successful request, the API will return a JSON object indicating whether the proof is valid. A `true` value under `isValid` confirms the file's integrity and the authenticity of its ownership. Any failure in the proof or data will result in an `isValid` value of `false` and include an error message detailing the discrepancy.

---

This API provides a structured and programmable interface to interact with the Verifier tool, allowing automated verification of proofs within the network, enhancing security, and integrity checks within the decentralized file sharing environment.