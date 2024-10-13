# Setting up Circles SDK with React and javascript

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

    // Configuration for the Circles SDK
    const chainConfig = {
        pathfinderUrl: "https://pathfinder.aboutcircles.com",
        circlesRpcUrl: "https://rpc.falkenstein.aboutcircles.com",
        v1HubAddress: "0x29b9a7fbb8995b2423a71cc17cf9810798f6c543",
        v2HubAddress: "0xa5c7ADAE2fd3844f12D52266Cb7926f8649869Da",
        migrationAddress: "0xe1dCE89512bE1AeDf94faAb7115A1Ba6AEff4201",
        nameRegistryAddress: "0x738fFee24770d0DE1f912adf2B48b0194780E9AD",
        profileServiceUrl: "https://chiado-pathfinder.aboutcircles.com/profiles/",
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
