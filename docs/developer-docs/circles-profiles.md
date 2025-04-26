---
icon: address-card
---

# Setting Up Circles Profiles

Circles is built around the ERC1155 token standard, which allows tokens to have associated metadata. Since Circles focuses on personal currency and identity, it uses this metadata capability to store profile information.

The profile data itself is stored off-chain in IPFS, and the Circles avatar contract references the IPFS Content Identifier (CIDv0) of the profile data on-chain.

### Profile Schema

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
      "format": "data-url",
      "pattern": "^data:image\\/(png|jpeg|jpg|gif);base64,",
      "description": "A base64-encoded image data URL for the profile preview"
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

### Profile Picture

You can include a profile picture within the profile document using the `previewImageUrl` field. If you choose to do so, ensure the picture used meets the following requirements:

1.  **Format**: PNG, JPEG, or GIF.
2.  **Dimensions**: Exactly 256x256 pixels.
3.  **File Size**: Must not exceed 150KB.
4.  **Encoding**: Must be base64 encoded and provided as a data URL in the `previewImageUrl` field.

These requirements are enforced by the profile service to ensure consistency and performance.

Below is a TypeScript example demonstrating how to resize an image using a canvas, compress it to JPEG format, and check its size before potentially using it as a `previewImageUrl`.

```typescript
  const img = new Image();
        img.src = reader.result as string; // Assuming reader.result contains the initial image data URL
        img.onload = () => {
          const canvas = document.createElement('canvas');
          const ctx = canvas.getContext('2d');
          const cropWidth = 256;
          const cropHeight = 256;

          if (ctx) {
            canvas.width = cropWidth;
            canvas.height = cropHeight;

            ctx.drawImage(img, 0, 0, cropWidth, cropHeight);

            const imageDataUrl = canvas.toDataURL('image/jpeg', 0.5); // Compress to JPEG with 50% quality

            if (imageDataUrl.length > 150 * 1024) {
              // Handle cases where the compressed image is still too large
              console.warn('Image size exceeds 150 KB after compression');
            } else {
              // Use imageDataUrl for the previewImageUrl field
            }
```

You can find a more complete example within the [5ecret Garden codebase](https://github.com/aboutcircles/5ecret-garden/blob/06da3e5d472b487fa8e1bc726561eddbc97fdd30/circles-app/src/lib/components/ImageUpload.svelte#L33) (Browser; Svelte/TypeScript).

{% hint style="warning" %}
Profiles that do not adhere to the schema and image requirements will not be processed or served by the Circles profile service.
{% endhint %}
