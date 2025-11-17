# Setting up Circles SDK with React

Here is an example code on how you can setup Circles SDK with a front-end framework like React.

```typescript
  import React, { createContext, useCallback, useEffect, useState } from 'react';
  import { Sdk } from '@aboutcircles/sdk';
  import { circlesConfig } from '@aboutcircles/sdk-core';
  import { SafeBrowserRunner } from '@aboutcircles/sdk-runner';
  import { createPublicClient, http } from 'viem';
  import { gnosis } from 'viem/chains';

  export const CirclesSDKContext = createContext(null);

  export const CirclesSDKProvider = ({ children, safeAddress }) => {
    const [sdk, setSdk] = useState(null);
    const [runner, setRunner] = useState(null);
    const [address, setAddress] = useState(null);
    const [isConnected, setIsConnected] = useState(false);

    const initSdk = useCallback(async () => {
      try {
        if (!window.ethereum || !safeAddress) {
          // Read-only SDK without runner
          setSdk(new Sdk(circlesConfig[100]));
          setIsConnected(true);
          return;
        }

        const publicClient = createPublicClient({
          chain: gnosis,
          transport: http(circlesConfig[100].circlesRpcUrl),
        });

        const safeRunner = new SafeBrowserRunner(publicClient, window.ethereum, safeAddress);
        await safeRunner.init();

        setRunner(safeRunner);
        setAddress(safeAddress);

        const sdkInstance = new Sdk(circlesConfig[100], safeRunner);
        setSdk(sdkInstance);
        setIsConnected(true);
      } catch (err) {
        console.error('Error initializing Circles SDK', err);
        setIsConnected(false);
      }
    }, [safeAddress]);

    useEffect(() => {
      initSdk();
    }, [initSdk]);

    return (
      <CirclesSDKContext.Provider
        value={{ sdk, runner, address, isConnected, initSdk }}
      >
        {children}
      </CirclesSDKContext.Provider>
    );
  };
```

