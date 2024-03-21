# File Transfer API Reference (Version 0.0.1)

## Overview

In this initial version of the decentralized file sharing network's API, we provide basic functionality for file uploading and downloading. This version assumes all deals are accepted due to the limited node set in the current network configuration. As such, metadata is streamlined to primarily involve the content identifier.

---

## Uploading Content

### Endpoint

`POST /upload`

### Description

Allows for the uploading of files to the network. Each file uploaded is securely stored and receives a unique content identifier, which serves as the primary metadata in version 0.0.1.

### Request Parameters

- **Body**:
  - `file` (binary): The file data to be uploaded.

### Success Response

- **Code**: `200 OK`
- **Content**:
  ```json
  {
    "contentId": "unique_content_identifier"
  }
  ```

### Error Response

- **Code**: `400 Bad Request`
- **Content**:
  ```json
  {
    "error": "Detailed error description"
  }
  ```

### Example Request

```bash
curl -X POST "http://*tbd.com/upload" \
     -H "Content-Type: multipart/form-data" \
     -F "file=@/path/to/file"
```

*Note*: In this initial release, the metadata associated with files is simplified to only the content ID. The domain name tbd.com is a placeholder.

---

## Downloading Content

### Endpoint

`GET /files/{contentId}`

### Description

This endpoint supports downloading files from the network using their unique content identifier. This version simplifies interactions by not involving extensive metadata or authentication processes, reflecting the network's initial scope.

### Path Parameters

- `contentId` (string): The unique identifier of the content to be downloaded.

### Success Response

- **Code**: `200 OK`
- **Content**: The file data.

### Error Response

- **Code**: `404 Not Found`
- **Content**:
  ```json
  {
    "error": "Content not found"
  }
  ```

### Example Request

```bash
curl -X GET "http://*tbd.com/files/{contentId}" \
     -O -J
```

*Note*: The focus is on essential functionalities, aligning with the streamlined approach of this version. Future updates may expand metadata capabilities and introduce new features as the network evolves. The domain name tbd.com is a placeholder.

---