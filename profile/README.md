# Obscurau

Obscurau provides a virtual machine engine that protects Lua and Luau scripts from reverse engineering. This organization hosts our open-source integration libraries, API clients, and public documentation.

## Core Technology

Our primary product is a closed-source obfuscator powered by **ObscurauVM**. We do not use basic minification or simple variable renaming. We compile human-readable Lua and Luau into a custom bytecode format, which is then executed by our internal virtual machine. 

This process applies multiple protection layers:
- **String Encryption:** Hides plaintext strings from static analysis, preventing attackers from easily extracting URLs or sensitive text directly from the binary.
- **Control Flow Flattening:** Destroys recognizable structures like loops and branch conditions, preventing decompilers from reconstructing the original source code logic.
- **Opcode Randomization:** Generates unique instruction sets for every build. This ensures that generic decompilers cannot parse the bytecode, and custom decompilers must be rebuilt from scratch for every single obfuscated script.
- **Constant Obfuscation:** Masks mathematical algorithms, numerical values, and booleans to prevent logical analysis.
- **Anti-Tamper Mechanisms:** Detects environment modifications and unauthorized analysis tools at runtime to protect the execution integrity.

## Supported Environments

The engine compiles and obfuscates code across multiple dialects. We maintain specific syntax compatibility for:

- **Standard Lua & LuaJIT:** Support for Lua 5.1, 5.2, 5.3, 5.4, 5.5, and LuaJIT. This covers general-purpose backend scripting, legacy applications, and server-side logic.
- **Game Lua:** For environments like FiveM, Garry's Mod, and other custom Lua runtimes. We preserve game-specific globals and environment structures.
- **Luau:** For Roblox and standalone Luau environments. We handle Luau-specific syntax, including type annotations and custom engine data types (like `Vector3` or `CFrame`), ensuring the output remains valid in the engine.

## Open-Source Integrations

ObscurauVM and the billing platform remain closed-source. However, we maintain open-source developer tools to interact with our API. 

Our official API clients allow you to automate script obfuscation within your CI/CD pipelines (e.g., GitHub Actions). Instead of manually uploading scripts to a dashboard, you can pass them directly from your build step to our API, and receive the obfuscated output programmatically.

A typical automated workflow involves:
1. Pushing raw source code to a private repository.
2. A CI/CD runner bundles the Lua files.
3. The runner calls the Obscurau API Client to obfuscate the production bundle.
4. The runner deploys the obfuscated artifact to your servers or game platform.

We currently maintain repositories for official API clients and plan to release wrappers for various languages (C#/.NET, Go, etc.) based on developer demand.

## Security Philosophy and Trade-offs

Obfuscation is not encryption. It does not make reverse engineering impossible; it makes it computationally expensive and severely limits the attacker's return on investment. You should never hardcode database passwords or critical backend secrets inside your scripts, even if they are obfuscated by ObscurauVM. Move authoritative logic to your server.

We design our VM to break decompilers and frustrate manual analysis. The inherent trade-off is performance. Scripts packed in Obscurau run slower than their native counterparts because every instruction must execute through our virtual machine layer. While we continuously research and optimize ObscurauVM to approach native execution speeds, you must still apply obfuscation selectively.

We recommend using our tools only to protect critical security logic, license checks, or proprietary algorithms. Do not obfuscate tight game loops, render steps (like `RenderStepped` in Roblox), or physics calculations. Doing so will drop your application's frame rate. If a function runs 60 times a second, keep it native.

## Access and Billing

Access to the obfuscator API requires an active account on the Obscurau platform. We use a pay-as-you-go token model and subscription plans. API tokens must be generated from your dashboard and stored securely in your CI/CD secrets.

## Support and Contact

If you need help integrating our API clients or find a bug in our open-source libraries, open an issue in the relevant repository. 

- For open-source inquiries or API development, email us at: [git@obscurau.net](mailto:git@obscurau.net)
- For account, billing, or core platform issues, contact us on Discord.
