# Setting up Circles SDK with React

```javascript
import React, { createContext, useState, useEffect, useCallback } from "react";
import { BrowserProviderContractRunner } from "@circles-sdk/adapter-ethers";
import { Sdk } from "@circles-sdk/sdk";

// Create a context for the Circles SDK
const CirclesSDKContext = createContext(null);

// Provider component to wrap around your application
export const CirclesSDK = ({ children }) => {
    const [sdk, setSdk] = useState(null);
    const [isConnected, setIsConnected] = useState(false);
    const [adapter, setAdapter] = useState(null);
    const [circlesProvider, setCirclesProvider] = useState(null);
    const [circlesAddress, setCirclesAddress] = useState(null);

    // Configuration for the Circles SDK on Gnosis Chain
    const gnosisChainConfig = {
        circlesRpcUrl: "https://rpc.aboutcircles.com/",
        pathfinderUrl: "https://pathfinder.aboutcircles.com",
        v1HubAddress: "0x29b9a7fbb8995b2423a71cc17cf9810798f6c543",
        v2HubAddress: "0xc12C1E50ABB450d6205Ea2C3Fa861b3B834d13e8",
        nameRegistryAddress: "0xA27566fD89162cC3D40Cb59c87AAaA49B85F3474",
        migrationAddress: "0xD44B8dcFBaDfC78EA64c55B705BFc68199B56376",
        profileServiceUrl: "https://rpc.aboutcircles.com/profiles/",
        };

    // Function to initialize the SDK
    const initSdk = useCallback(async () => {
        try {
            const adapter = new BrowserProviderContractRunner();
            await adapter.init(); // Initialize the adapter before using it
            
            setAdapter(adapter); // Set the adapter in the state

            const circlesProvider = adapter.provider;
            setCirclesProvider(circlesProvider); // Store the provider
            
            const circlesAddress = await adapter.address;
            setCirclesAddress(circlesAddress); // Set the address
            
            const sdk = new Sdk(chainConfig, adapter); // Pass the initialized adapter to the SDK
            setSdk(sdk); // Set the SDK in the state
            setIsConnected(true); // Update connection status
        } catch (error) {
            console.error("Error initializing SDK:", error);
        }
    }, []);

    useEffect(() => {
        initSdk(); // Call initSdk when the component mounts
    }, [initSdk]);

    // Provide the SDK context to child components
    return (
        <CirclesSDKContext.Provider value={{
            sdk,
            setIsConnected,
            isConnected,
            adapter,
            circlesProvider,
            circlesAddress,
            initSdk,
        }}>
            {children}
        </CirclesSDKContext.Provider>
    );
};

export default CirclesSDKContext;

```
