# Finding Groups and Memberships

To find groups and retrieve membership information, you typically use the `data` module accessible via your initialized SDK instance (`sdk.data`). This module interacts with the Circles RPC Query API.

```typescript
// Assuming 'sdk' is your initialized SDK instance
const data = sdk.data;
```

Alternatively, if you are not using the main SDK instance, you can instantiate `CirclesData` directly using `CirclesRpc` from the `@circles-sdk/data` package (ensure you have it installed):

```typescript
// Example using direct instantiation (less common if using the main SDK)
// import { CirclesRpc, CirclesData } from '@circles-sdk/data';
// const circlesRpc = new CirclesRpc("https://rpc.aboutcircles.com/"); // Use appropriate RPC URL
// const data = new CirclesData(circlesRpc);
```

## 1. Find Groups

This method fetches a list of registered Group Avatars from the Circles system, supporting pagination and filtering.

*   **Parameters**:
    *   `pageSize` (number): Specifies the maximum number of groups to return per page.
    *   `params` (object, optional): An object containing filter criteria. The available filter options depend on the RPC Query API implementation (e.g., filtering by name, owner, etc.). Consult the API documentation for specific filter possibilities.

```typescript
// Assuming 'data' is your initialized data module instance (e.g., sdk.data)
const groupsPageSize = 10; // Max groups per page
const queryParams = { /* owner: '0x...' */ }; // Example: Filter by owner address (verify actual filter keys)

try {
    const groupsQueryResult = await data.findGroups(groupsPageSize, queryParams);
    console.log('Retrieved groups:', groupsQueryResult);
    // groupsQueryResult likely contains an array of group details and pagination info
} catch (error) {
    console.error('Error fetching groups:', error);
}
```

## 2. Get Group Memberships

This method fetches all group memberships associated with a specific avatar address, useful for displaying the groups a user belongs to.

*   **Parameters**:
    *   `avatarAddress` (string): The address of the avatar whose memberships you want to retrieve.
    *   `pageSize` (number): Specifies the maximum number of memberships to return per page.

```typescript
// Assuming 'data' is your initialized data module instance (e.g., sdk.data)
const avatarAddress = '0xYourAvatarAddress'; // The address of the avatar
const membershipsPageSize = 5; // Max memberships per page

try {
    const membershipsQueryResult = await data.getGroupMemberships(avatarAddress, membershipsPageSize);
    console.log('Retrieved group memberships:', membershipsQueryResult);
    // membershipsQueryResult likely contains an array of membership details and pagination info
} catch (error) {
    console.error('Error fetching group memberships:', error);
}
```
