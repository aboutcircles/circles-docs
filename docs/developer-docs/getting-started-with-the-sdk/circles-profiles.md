# 🪪 Circles Profiles

Circles is built around the ERC1155 token standard which allows tokens to have metadata. Since Circles is all about personal currency, it makes sense to utilize this metadata as a profile.&#x20;

The profile data is stored in IPFS and the Circles avatar references the CIDv0 of the profile on-chain.

### Profile schema

The schema is very simple and only has one required attribute:

```json
{
  "$schema": "http://json-schema.org/draft-07/schema#",
  "type": "object",
  "properties": {
    "name": {
      "type": "string",
      "maxLength": 36,
      "description": "The name of the profile owner"
    },
    "description": {
      "type": "string",
      "maxLength": 500,
      "description": "A description of the profile"
    },
    "previewImageUrl": {
      "type": "string",
      "description": "Can be a base64-encoded image data URL for the profile preview or an external link",
      "maxLength": 2000
    },
    "imageUrl": {
      "type": "string",
      "maxLength": 2000,
      "description": "A URL pointing to the profile image"
    }
  },
  "required": ["name"],
  "additionalProperties": false
}
```

### @circles-sdk/profiles packages

You may want to query the profiles for a bunch of of avatars all at once. For cases like this you can use the `@circles-sdk/profiles` package which contains methods that help you to handle profiles.
