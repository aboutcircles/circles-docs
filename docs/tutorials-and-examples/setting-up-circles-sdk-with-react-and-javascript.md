# Setting Up Circles SDK with React

This tutorial demonstrates how to set up the Circles SDK within a React application using Context API. This allows you to initialize the SDK once and access it from any component within the provider's scope.

```typescript
import React, {
  createContext,
  useState,
  useEffect,
  useCallback,
  useContext, // Import useContext
  ReactNode, // Import ReactNode for children prop type
} from "react";
import { BrowserProviderContractRunner } from "@circles-sdk/adapter-ethers";
import { Sdk, circlesConfig as productionConfig } from "@circles-sdk/sdk"; // Import default config
import type { Eip1193Provider } from "ethers"; // Import provider type

// Define the shape of the context value
interface CirclesSDKContextValue {
  sdk: Sdk | null;
  isConnected: boolean;
  adapter: BrowserProviderContractRunner | null;
  circlesProvider: Eip1193Provider | null; // Use specific type
  circlesAddress: string | null;
  initSdk: () => Promise<void>;
  setIsConnected: React.Dispatch<React.SetStateAction<boolean>>; // Add setIsConnected type
}

// Create a context for the Circles SDK with a default value
const CirclesSDKContext = createContext<CirclesSDKContextValue | null>(null);

// Define props for the provider component
interface CirclesSDKProviderProps {
  children: ReactNode;
}

// Provider component to wrap around your application
export const CirclesSDKProvider = ({ children }: CirclesSDKProviderProps) => {
  // State variables with types
  const [sdk, setSdk] = useState<Sdk | null>(null);
  const [isConnected, setIsConnected] = useState<boolean>(false);
  const [adapter, setAdapter] = useState<BrowserProviderContractRunner | null>(null);
  const [circlesProvider, setCirclesProvider] = useState<Eip1193Provider | null>(null);
  const [circlesAddress, setCirclesAddress] = useState<string | null>(null);

  // Configuration for the Circles SDK (using imported production config)
  // You could also define gnosisChainConfig or ringsConfig manually here if needed
  const currentConfig = productionConfig;

  // Function to initialize the SDK
  const initSdk = useCallback(async () => {
    // Avoid re-initialization if already connected
    if (isConnected || sdk) return;

    console.log("Initializing Circles SDK...");
    try {
      const browserAdapter = new BrowserProviderContractRunner();
      await browserAdapter.init(); // Initialize the adapter (connects to wallet)

      setAdapter(browserAdapter); // Store the adapter instance

      const provider = browserAdapter.provider;
      setCirclesProvider(provider); // Store the Ethers provider

      const address = await browserAdapter.getAddress(); // Get connected address
      setCirclesAddress(address); // Store the address

      // Initialize the SDK with the chosen config and the initialized adapter
      const sdkInstance = new Sdk(browserAdapter, currentConfig);
      setSdk(sdkInstance); // Store the SDK instance

      setIsConnected(true); // Update connection status
      console.log("Circles SDK Initialized. Address:", address);

    } catch (error) {
      console.error("Error initializing Circles SDK:", error);
      setIsConnected(false); // Ensure connection status is false on error
      // Reset state if needed
      setSdk(null);
      setAdapter(null);
      setCirclesProvider(null);
      setCirclesAddress(null);
    }
  }, [isConnected, sdk, currentConfig]); // Add dependencies

  // Optional: Attempt initialization on mount if desired,
  // but often better triggered by a user action (e.g., "Connect Wallet" button).
  // useEffect(() => {
  //     initSdk();
  // }, [initSdk]);

  // Provide the SDK context value to child components
  const contextValue: CirclesSDKContextValue = {
    sdk,
    isConnected,
    adapter,
    circlesProvider,
    circlesAddress,
    initSdk, // Expose init function so components can trigger connection
    setIsConnected,
  };

  return (
    <CirclesSDKContext.Provider value={contextValue}>
      {children}
    </CirclesSDKContext.Provider>
  );
};

// Custom hook for easy context consumption
export const useCirclesSDK = (): CirclesSDKContextValue => {
  const context = useContext(CirclesSDKContext);
  if (!context) {
    throw new Error("useCirclesSDK must be used within a CirclesSDKProvider");
  }
  return context;
};

export default CirclesSDKContext; // Keep default export if needed elsewhere

/*
// Example Usage in a Component:

import React from 'react';
import { useCirclesSDK } from './CirclesSDKContext'; // Adjust path as needed

const MyComponent = () => {
  const { sdk, isConnected, circlesAddress, initSdk } = useCirclesSDK();

  const handleGetBalance = async () => {
    if (sdk && circlesAddress) {
      try {
        const balance = await sdk.data.getTotalBalance(circlesAddress);
        console.log(`Balance: ${balance}`);
        alert(`Your Balance: ${balance}`);
      } catch (error) {
        console.error("Error getting balance:", error);
      }
    } else {
      alert("SDK not connected.");
    }
  };

  return (
    <div>
      {!isConnected ? (
        <button onClick={initSdk}>Connect Wallet & Init SDK</button>
      ) : (
        <div>
          <p>Connected Address: {circlesAddress}</p>
          <button onClick={handleGetBalance}>Get My Balance</button>
          {/* Add other SDK interactions here *}
        </div>
      )}
    </div>
  );
};

export default MyComponent;

*/
```

This example provides a `CirclesSDKProvider` component to wrap your app (or relevant part) and a `useCirclesSDK` hook to easily access the `sdk` instance, connection status, user address, and the `initSdk` function within your components.

To access the entire codebase for building frontend applications using React and Circles SDK, check out the Github repo [here](https://github.com/aboutcircles/circles-vite-app/tree/main/src/circles-components) (link updated to main branch).
