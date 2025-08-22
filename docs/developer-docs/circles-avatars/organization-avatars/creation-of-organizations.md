# Creation of Organizations

Organizations are different from groups as you can't mint an organization token, use profiles and trust other avatars to receive tokens from them.

{% tabs %}
{% tab title="Circles v2.0" %}
```typescript
const profile = {  
  name: "Organization Name",  
  description: "About the organization"  
};  
  
// Register a V2 organization avatar with the profile  
const orgAvatarV2 = await sdk.registerOrganizationV2(profile);
```
{% endtab %}
{% endtabs %}



