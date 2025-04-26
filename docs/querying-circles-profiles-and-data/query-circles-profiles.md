---
icon: head-side-gear
---

# Querying Circles Profiles (Direct API)

This section demonstrates how to interact directly with the Circles Profile Service API using HTTP requests (e.g., via `curl`). This provides lower-level access compared to using the SDK's profile methods.

The Profile Service allows you to:

*   Create (pin) new user profiles to IPFS.
*   Retrieve existing profiles by CID.
*   Search profiles using parameters like `name`, `cid`, `description`, or `address`.

**Base URL:** `https://rpc.aboutcircles.com/profiles/` (Production)

### 1. Create/Pin a Profile (POST `/pin`)

Pins the provided profile data JSON to IPFS via the service and returns the CID.

```bash
curl -X POST "https://rpc.aboutcircles.com/profiles/pin" \
     -H "Content-Type: application/json" \
     -d '{
           "name": "John Doe",
           "description": "A blockchain developer",
           "previewImageUrl": "data:image/jpeg;base64,...", # Ensure valid data URL and size/dimensions
           "imageUrl": "https://example.com/image.jpg"
         }'
# Note: 'extensions' field is not part of the standard schema.
```

### 2. Get a Profile by CID (GET `/get`)

Retrieves profile data for a specific CID.

```bash
# Replace Qm... with the actual CID
curl -X GET "https://rpc.aboutcircles.com/profiles/get?cid=Qm12345abcdef"
```

### 3. Get Multiple Profiles by CIDs (GET `/getBatch`)

Retrieves profile data for multiple CIDs provided as a comma-separated list.

{% code fullWidth="false" %}
```bash
# Replace Qm... with actual CIDs
curl -X GET "https://rpc.aboutcircles.com/profiles/getBatch?cids=Qm12345abcdef,Qm67890ghijk"
```
{% endcode %}

### 4. Search Profiles by Name (GET `/search`)

Searches for profiles matching a name (case-insensitive, partial match).

```bash
curl -X GET "https://rpc.aboutcircles.com/profiles/search?name=John"
```

### 5. Search Profiles by Description (GET `/search`)

Searches for profiles matching text within the description (case-insensitive, partial match).

```bash
curl -X GET "https://rpc.aboutcircles.com/profiles/search?description=Circles"
```

### 6. Search Profiles by Address (GET `/search`)

Searches for profiles associated with a specific avatar address.

```bash
# Replace 0x... with the actual address
curl -X GET "https://rpc.aboutcircles.com/profiles/search?address=0x1234567890abcdef"
```

### 7. Search Profiles by CID (GET `/search`)

Searches for profiles matching a specific CID.

```bash
# Replace Qm... with the actual CID
curl -X GET "https://rpc.aboutcircles.com/profiles/search?cid=Qm12345abcdef"
```

### 8. Search Profiles with Multiple Criteria (GET `/search`)

Combines multiple search parameters (AND logic).

```bash
# Replace with actual search values
curl -X GET "https://rpc.aboutcircles.com/profiles/search?name=John&description=blockchain&address=0x1234567890abcdef&cid=Qm12345abcdef"
```
