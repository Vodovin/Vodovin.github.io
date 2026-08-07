---
title: 'Zombfection - Authoritative Multiplayer Game'
description: Transformation of a single-player prototype into an authoritative asymmetric multiplayer game using Unity and Netcode for GameObjects (NGO), featuring client-side prediction and server reconciliation.
publishDate: 'May 28 2025'
isFeatured: true
seo:
  image:
    src: '../../assets/images/project-5.jpg'
---

![Project preview](../../assets/images/project-5.jpg)

**Project Overview:**
This project involved re-architecting a single-player prototype ("Zombfection", inspired by classic infection mods like *Zombie Panic!*) into a fully functional, authoritative multiplayer game. Developed in Unity using Netcode for GameObjects (NGO), the project focused on robust network architecture, state synchronization, and advanced multiplayer mechanics.

## Design Objectives

The primary goal was to transition the game from local execution to a **Server-Authoritative model**, ensuring that the server acts as the single source of truth to prevent client-side cheating. This required refactoring core systems (GameManager, LevelManager, PlayerController) to handle concurrent connections, state replication, and network events without introducing race conditions.

## Main Features & Implementation

1. **Server-Authoritative Architecture & Lobby System:**
   * Implemented a custom `GameManager` using NGO to handle player connections, disconnections, and lobby creation.
   * Developed a "Ready" system ensuring matches only start when a minimum number of players are prepared.
   * Engineered an automated, randomized team assignment algorithm balancing asymmetric factions (Zombies vs. Humans).

2. **Advanced Network Techniques (Client-Side Prediction):**
   * Implemented **Client-Side Prediction** and **Server Reconciliation** to mask network latency. The client simulates movement locally based on user input, while the server continuously validates actions. Any discrepancies result in smooth client corrections, ensuring a responsive gameplay feel regardless of ping.

3. **Asymmetric Gameplay Synchronization:**
   * Synchronized complex game states across all clients using `NetworkVariables` and `RPCs` (Remote Procedure Calls).
   * Managed cooperative events, such as a shared coin collection counter for the Human team, and collision-based infection events (Human-to-Zombie conversion) validated exclusively on the server.
   * Developed logic to handle match termination gracefully, managing scenarios like total infection, time-outs, objective completion, or player abandonment.

4. **Combat System & Asymmetric Data Sharing (Minimap):**
   * Designed a combat and respawn system integrating weapon mechanics, ammunition pickups, and kill tracking.
   * Implemented an intelligent Mini-map system relying on asymmetric data transmission: the server selectively sends data so Humans only see allies and nearby Zombies, while Zombies receive full map visibility.

## Technology Stack

- **Game Engine:** Unity (2022.3 LTS).
- **Programming Language:** C# for gameplay, logic refactoring, and UI management.
- **Networking Library:** Netcode for GameObjects (NGO).
- **Networking Patterns:** Server-Authoritative Architecture, Client-Side Prediction, Server Reconciliation, RPCs, Object Spawning.