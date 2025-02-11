---
icon: head-side-gear
---

# Query Circles profiles

## :bulb: Query Circles Profiles

Retrieve user profile data using the Profiles Nethermind plugin. You can:

* Create new user profiles
* Search existing profiles using parameters such as:
  * Name
  * CID (Content Identifier)
  * Description
  * Wallet Address

### 1. Create a Profile (POST request)

```bash
curl -X POST "https://rpc.aboutcircles.com/profiles/pin" \
     -H "Content-Type: application/json" \
     -d '{
           "name": "John Doe",
           "description": "A blockchain developer",
           "previewImageUrl": "https://example.com/preview.jpg",
           "imageUrl": "https://example.com/image.jpg",
           "extensions": {
             "twitter": "@johndoe",
             "github": "johndoe"
           }
         }'
```

### 2. Get a Profile by CID (GET request)

```bash
curl -X GET "https://rpc.aboutcircles.com/profiles/get?cid=Qm12345abcdef"
```

### 3. Get Multiple Profiles by CIDs (GET request)

{% code fullWidth="false" %}
```bash
curl -X GET "https://rpc.aboutcircles.com/profiles/getBatch?cids=Qm12345abcdef,Qm678bbdj
```
{% endcode %}

### 4. Search Profiles by Name (GET request)

```bash
curl -X GET "https://rpc.aboutcircles.com/profiles/search?name=John"
```

### 5. Search Profiles by Description (GET request)

```bash
curl -X GET "https://rpc.aboutcircles.com/profiles/search?description=Circles"
```

### 6. Search Profiles by Address (GET request)

```bash
curl -X GET "https://rpc.aboutcircles.com/profiles/search?address=0x1234567890abcdef"
```

### 7. Search Profiles by CID (GET request)

```bash
curl -X GET "https://rpc.aboutcircles.com/profiles/search?CID=Qm12345abcdef"
```

### 8. Search Profiles with Multiple Criteria (GET request)

```bash
curl -X GET "https://rpc.aboutcircles.com/profiles/search?name=John&description=blockchain&address=0x1234567890abcdef&CID=Qm12345abcdef"
```
