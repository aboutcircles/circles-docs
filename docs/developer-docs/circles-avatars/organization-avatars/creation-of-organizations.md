# Creation of Organizations

Organizations are different from groups as you can't mint an organization token, use profiles and trust other avatars to receive tokens from them.

{% tabs %}
{% tab title="Circles v2.0" %}
<pre class="language-typescript"><code class="lang-typescript"><strong>const registerV2Organization = async (sdk, profile) => {
</strong>    try {
        const avatar = await sdk.registerOrganizationV2(profile); // Call the V2 method
        console.log('V2 Organization Avatar:', avatar);
    } catch (error) {
        console.error('Error registering organization V2:', error);
    }
};
</code></pre>
{% endtab %}

{% tab title="Legacy Circles" %}
```typescript
const registerLegacyOrganization = async (sdk) => {
    try {
        const avatar = await sdk.registerOrganization(); // Call the legacy method
        console.log('Legacy Organization Avatar:', avatar);
    } catch (error) {
        console.error('Error registering legacy organization:', error);
    }
};
```


{% endtab %}
{% endtabs %}



