---
tags:: typescript, t3, tRPC
creator:: web-dev-simplified
---
### What is tRPC
- provides type check safety between client and server
	- helps reduce bugs
### Basic Usage
- Use with typescript
- client and server need to be in same repository
#### Implementing tRPC on your server
- first install the package with `npm i @trpc/server` to the repository
- run the server again
- then in the server: 
```typescript
import { initTRPC } from "@trpc/server"
```
- then create a variable to house the 