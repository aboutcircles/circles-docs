# Setting up Circles SDK with React

```typescript
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
    const circlesConfig = {  
        circlesRpcUrl: "https://rpc.aboutcircles.com/",  
        pathfinderUrl: "https://pathfinder.aboutcircles.com",  
        v1HubAddress: "0x29b9a7fbb8995b2423a71cc17cf9810798f6c543",  
        v2HubAddress: "0xc12C1E50ABB450d6205Ea2C3Fa861b3B834d13e8",  
        nameRegistryAddress: "0xA27566fD89162cC3D40Cb59c87AAaA49B85F3474",  
        migrationAddress: "0xD44B8dcFBaDfC78EA64c55B705BFc68199B56376",  
        profileServiceUrl: "https://rpc.aboutcircles.com/profiles/",   
        baseGroupFactory: "0xD0B5Bd9962197BEaC4cbA24244ec3587f19Bd06d",  
        coreMembersGroupDeployer: "0xFEca40Eb02FB1f4F5F795fC7a03c1A27819B1Ded",
        }
  
    // Function to initialize the SDK  
    const initSdk = useCallback(async () => {  
        try {  
            // Create and initialize the adapter  
            const adapter = new BrowserProviderContractRunner();  
            await adapter.init(); // Initialize the adapter before using it  
              
            setAdapter(adapter);  
  
            // Get the provider and address  
            const provider = adapter.provider;  
            setCirclesProvider(provider);  
              
            const address = await adapter.address;  
            setCirclesAddress(address);  
              
            // Create the SDK instance with the config and adapter  
            const sdk = new Sdk(adapter, circlesConfig);  
            setSdk(sdk);  
            setIsConnected(true);  
        } catch (error) {  
            console.error("Error initializing SDK:", error);  
            setIsConnected(false);  
        }  
    }, []);  
  
    useEffect(() => {  
        initSdk();  
    }, [initSdk]);  
  
    return (  
        <CirclesSDKContext.Provider value={{  
            sdk,  
            isConnected,  
            setIsConnected,  
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

To access the entire codebase for building frontend applications using React and Circles SDK, check out the Github repo [here](https://github.com/aboutcircles/circles-vite-app/tree/circles-sdk-v0.14.0/src/circles-components).
