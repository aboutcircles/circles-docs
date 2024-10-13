# Find groups and memberships

You would be requiring to initialize the Circles data property to find groups and get memberships.

```typescript
const data = sdk.data
```

&#x20;Otherwise you can create an instance like this:

```typescript
const circlesRpc = new CirclesRpc("https://chiado-rpc.aboutcircles.com");
const data = new CirclesData(circlesRpc);
```

## Find Groups

* **Functionality**: This method allows you to fetch a list of groups from the Circles system, with options for pagination and filtering. This is useful for applications that need to display groups or for querying specific groups based on certain criteria.
*   **Parameters**:

    * `pageSize`: A number specifying how many groups should be returned in the response.
    * `params`: An optional parameter that can include various filters for the query, such as group types or statuses.



```typescript
const groupsPageSize = 10; // Define the maximum number of groups to return
const queryParams = { /* Example filter parameters */ };

try {
    const groupsQueryResult = await circles.data.findGroups(groupsPageSize, queryParams);
    console.log('Retrieved groups:', groupsQueryResult);
} catch (error) {
    console.error('Error fetching groups:', error);
}
```

## Get group memberships

This method is designed to fetch all group memberships associated with a specific avatar. This is useful for applications that want to display or manage the groups that a user belongs to.

**Parameters**:

* `avatar`: A string representing the address of the avatar for which group memberships are being requested.
* `pageSize`: A number that specifies the maximum number of group memberships to return.

```typescript
const avatarAddress = '0xYourAvatarAddress'; 
// The address of the avatar
const membershipsPageSize = 5; 
// Define the maximum number of memberships to return

try {
    const membershipsQueryResult = await circles.getGroupMemberships(avatarAddress, membershipsPageSize);
    console.log('Retrieved group memberships:', membershipsQueryResult);
} catch (error) {
    console.error('Error fetching group memberships:', error);
}
```
