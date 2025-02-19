Convert raw user description and API context into an implementation-ready specification.
Make sure we provide a functional web app that meets the user requirements.
Be concise and clear in your documentation.
There is no database available for this project.
We have an existing Farcaster miniapp (Frame v2) template to work with.
We need to customize the template in form and function to meet the user requirements.

## API Context
Neynar API shared info:
- API key is stored in env variable NEYNAR_API_KEY
- it cannot be access from the frontend code and should be used only in the backend
- create serverside functions to use the API key

Farcaster is a decentralized social network built on Ethereum, emphasizing user data ownership and interoperability. Each user connects their Ethereum wallet, ensuring secure identity verification and seamless integration with blockchain-based applications. ￼

Developers can leverage Farcaster’s open protocol to create innovative applications. The platform offers various tools and APIs to facilitate this process. For instance, the Farcaster-py SDK allows developers to programmatically interact with the Farcaster network using Python, enabling tasks such as posting messages, retrieving user information, and more. ￼

Additionally, Farcaster supports the development of “frames,” which are mini-apps that run inside a Farcaster feed. These frames enable rich, interactive experiences without requiring users to leave their social feed. Frameworks like Frog have been developed to simplify the creation of these frames, providing a minimal footprint and utilities for common tasks. ￼

Farcaster’s architecture includes Hubs, which are distributed servers that store and validate data. Developers can run their own Hubs to gain real-time access to Farcaster data, enhancing the decentralization and resilience of the network. ￼

The platform’s integration with blockchain technology ensures that user data is secure and tamper-proof. By leveraging Ethereum’s capabilities, Farcaster provides a censorship-resistant environment where users maintain control over their social interactions. ￼

For more detailed information on building with Farcaster, including tutorials and API documentation, developers can refer to the official Farcaster documentation.￼https://docs.farcaster.xyz/developers/

# Farcaster frames v2

A streamlined guide for building a Farcaster Frames v2 demo application using Next.js, TypeScript, React, Frame SDK, and Wagmi. This documentation outlines the core architecture, key components, and interactions within the application.

Providers

Custom Wagmi Connector

A custom connector bridges the Wagmi library with the Farcaster Frame SDK, enabling interactions with the user’s Farcaster wallet.

// lib/connector.ts
import sdk from "@farcaster/frame-sdk";
import { createConnector } from "wagmi";

export function frameConnector() {
  return createConnector({
    // Connector configuration leveraging sdk.wallet.ethProvider
  });
}

Note: Future SDK releases aim to incorporate this connector directly, simplifying integration.

Wagmi Provider Component

Configures the Wagmi client with necessary chains and connectors, and integrates React Query for data fetching and caching.

// components/providers/WagmiProvider.tsx
import { createConfig, http, WagmiProvider } from "wagmi";
import { base } from "wagmi/chains";
import { QueryClient, QueryClientProvider } from "@tanstack/react-query";
import { frameConnector } from "~/lib/connector";

const config = createConfig({
  chains: [base],
  connectors: [frameConnector()],
  transports: { [base.id]: http() },
});

const queryClient = new QueryClient();

export default function Provider({ children }: { children: React.ReactNode }) {
  return (
    <WagmiProvider config={config}>
      <QueryClientProvider client={queryClient}>
        {children}
      </QueryClientProvider>
    </WagmiProvider>
  );
}

Top-Level Providers

Integrates all provider components into the application using Next.js’ dynamic imports to ensure client-side rendering where necessary.

// app/providers.tsx
"use client";

import dynamic from "next/dynamic";

const WagmiProvider = dynamic(() => import("~/components/providers/WagmiProvider"), {
  ssr: false,
});

export function Providers({ children }: { children: React.ReactNode }) {
  return <WagmiProvider>{children}</WagmiProvider>;
}

Core Components

Demo Component

The central component of the application, handling SDK initialization, context retrieval, and rendering interactive UI elements.

// app/components/Demo.tsx
"use client";

import { useEffect, useState } from "react";
import sdk, { type FrameContext } from "@farcaster/frame-sdk";

export default function Demo() {
  const [isSDKLoaded, setIsSDKLoaded] = useState(false);
  const [context, setContext] = useState<FrameContext>();

  useEffect(() => {
    const initializeSDK = async () => {
      setContext(await sdk.context);
      sdk.actions.ready();
      setIsSDKLoaded(true);
    };
    if (sdk && !isSDKLoaded) {
      initializeSDK();
    }
  }, [isSDKLoaded]);

  if (!isSDKLoaded) return <div>Loading...</div>;

  return (
    <div className="w-[300px] mx-auto py-4 px-2">
      <h1 className="text-2xl font-bold text-center mb-4">Frames v2 Demo</h1>
      {/* Additional UI Elements */}
    </div>
  );
}

Action Buttons

Interactive buttons that invoke specific actions provided by the Frame SDK, such as opening URLs or closing the frame.

import { useCallback } from "react";
import sdk from "@farcaster/frame-sdk";
import { Button } from "~/components/ui/Button";

export default function ActionButtons() {
  const openUrl = useCallback(() => {
    sdk.actions.openUrl("https://www.example.com");
  }, []);

  const closeFrame = useCallback(() => {
    sdk.actions.close();
  }, []);

  return (
    <div>
      <Button onClick={openUrl}>Open Link</Button>
      <Button onClick={closeFrame}>Close Frame</Button>
    </div>
  );
}

Context Display

Displays contextual information provided by the parent Farcaster app, such as user details. Includes toggling functionality for better UX.

import { useState, useCallback } from "react";

export default function ContextDisplay({ context }: { context: FrameContext }) {
  const [isContextOpen, setIsContextOpen] = useState(false);
  const toggleContext = useCallback(() => {
    setIsContextOpen((prev) => !prev);
  }, []);

  return (
    <div className="mb-4">
      <h2 className="font-2xl font-bold">Context</h2>
      <button onClick={toggleContext} className="flex items-center gap-2 transition-colors">
        <span className={`transform transition-transform ${isContextOpen ?

## User Requirements
Build a project based on this social media conversation:
    
        posted by hellno the optimist (@hellno.eth)
         from Berlin, Germany
        
        
        Date: 2025-02-19T21:02:06.000Z
        
        Text: new in @maschine (prompt → Farcaster miniapp) aka frameception
• 10x better frames: added more steps to think. LLM now writes a spec → then a plan → then a todo list from your input. only then starts to write code
• pulls in API docs dynamically (like Neynar or Dune). example: if you want a Farcaster search in your frame, it pulls in relevant docs from Neynar to use it correctly 
• UX: much snappier, separate pages, personalized, new organized sidebar

coming up: make setting up new frames more reliable & make UI simpler
        

        posted by hellno the optimist (@hellno.eth)
         from Berlin, Germany
        
        
        Date: 2025-02-19T21:03:39.000Z
        
        Text: still early, here’s the frame if you want to try privately (or tell @maschine to build a frame)

https://farcasterframeception.vercel.app/
        
    

## Output Format
1. Markdown document with these sections:
   - Functional User Requirements
   - Architecture Diagram
   - Data Flow Specification
   - Error Handling Strategies
2. Technical terms clearly defined
