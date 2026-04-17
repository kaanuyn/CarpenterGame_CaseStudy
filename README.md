Multiplayer Carpenter Workshop Case Study

A high performance, networked multiplayer workshop simulation built in Unreal Engine 5.7.
This project demonstrates a server authoritative architecture designed for 4 player cooperative play, focusing on modularity, data driven systems, and efficient network replication.

- Technical Architecture
The project is built with a strict Server Authoritative model to ensure game state integrity across all clients.

- Network Hierarchy & Authority
GameState (Shared State): Manages the shared workshop budget and the active order queue. Replicated to all clients to ensure synchronization.
Server RPCs: All gameplay actions (production, painting, delivery) are routed through validated Server RPCs to prevent desynchronization.
RepNotify & Visual Sync: Used for cosmetic changes (object colors, machine selection) to provide immediate visual feedback while maintaining the server as the source of truth.

- Data Driven Design
Primary Data Assets: Item types (Chairs, Tables, etc.) are defined as Data Assets. This decouples the logic from specific meshes, allowing for easy expansion without modifying code.
Blueprint Interfaces: A centralized BPI_Interact system handles all world interactions. This modular approach allows the player to interact with any object without expensive casting.

- Key Mechanics
The Carving Machine: Synchronized UI: Uses replicated indices to ensure all players see the same selected item on the machine's display.
Server Side Spawning: Production is handled exclusively by the server to ensure consistent object ownership.
Painting & Material System: When an item is painted, the server updates a replicated color variable. The OnRep function then updates the Dynamic Material Instance, ensuring late joining players see the correct state.

- Order & Evaluation System
Validation Algorithm: A server side check compares the delivered object's DataAsset and CurrentColor against the active order.
Dynamic Rewards: Calculates payments (100% for perfect match, 50% for color mismatch) and updates the shared GameState budget.
