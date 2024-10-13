---
icon: address-card
---

# Circles Profiles

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

### Profile picture

You can include a profile picture in the profile document (the `previewImageUrl`). If you choose to do so, make sure the picture you are using has the following properties:

1. **Format**: The image must be in PNG, JPEG, or GIF format.
2. **Dimensions**: The image must be exactly 256x256 pixels.
3. **File size**: The image must not exceed 150KB.
4. **Encoding**: The image must be base64 encoded and included as a data URL in the `previewImageUrl` field.

These requirements are enforced by the server to ensure consistency and performance across the platform.

```typescript
  const img = new Image();
        img.src = reader.result as string;
        img.onload = () => {
          const canvas = document.createElement('canvas');
          const ctx = canvas.getContext('2d');
          const cropWidth = 256;
          const cropHeight = 256;

          if (ctx) {
            canvas.width = cropWidth;
            canvas.height = cropHeight;

            ctx.drawImage(img, 0, 0, cropWidth, cropHeight);

            const imageDataUrl = canvas.toDataURL('image/jpeg', 0.5);

            if (imageDataUrl.length > 150 * 1024) {
              console.warn('Image size exceeds 150 KB after compression');
            }

```

&#x20;[Here](https://github.com/aboutcircles/5ecret-garden/blob/06da3e5d472b487fa8e1bc726561eddbc97fdd30/circles-app/src/lib/components/ImageUpload.svelte#L33) is a code example (Browser; TypeScript) that shows how you can prepare the previewImageUrl.

{% hint style="warning" %}
Profiles that don't adhere to the spec aren't considered and won't be served by Circles' profile service.
{% endhint %}
