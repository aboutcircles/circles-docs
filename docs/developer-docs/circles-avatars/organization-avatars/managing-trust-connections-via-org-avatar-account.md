# Managing trust connections via Org avatar account

Organizations can establish trust relationships with other avatars:



1.  **Trust other avatars**:

    ```typescript
    await orgAvatar.trust(otherAvatarAddress);  // Or trust multiple avatars at once  await orgAvatar.trust([avatar1Address, avatar2Address]);
    ```
2.  **Revoke trust**:

    ```typescript
    await orgAvatar.untrust(otherAvatarAddress);
    ```
3.  **Check trust relationships**:

    ```typescript
    const isTrusting = await orgAvatar.trusts(otherAvatarAddress);
    const isTrustedBy = await orgAvatar.isTrustedBy(otherAvatarAddress);
    ```
4.  **Get all trust relations**:

    ```typescript
    const trustRelations = await orgAvatar.getTrustRelations();
    ```

\
