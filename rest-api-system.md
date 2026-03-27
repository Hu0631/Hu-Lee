# KIT214/514 Assignment 2 – Game & Room REST API Service

## Overview

This project implements a full **multi-VM architecture** integrating Room Services, Game Servers, and a Web Frontend.

| Component | Description |
|------------|-------------|
| **VM3 – Room Server** | Provides REST APIs to manage games, rooms, and players. |
| **VM2 – Game Server** | Host and process the logic for each game (Hello World, Voting, Reaction Timer). |
| **VM1 – Web Server** | A Node.js web application that interacts with the Room Server to list, create, and delete games. |

---
## Configuration for the VM

- **Technologies:** Node.js, Express, EJS, mysql2

---
##  1. Room Server API (VM3)

### **Endpoints**

| **Method** | **Endpoint** | **Description** |
|-------------|---------------|-----------------|
| **GET** | `/game` | List all games |
| **GET** | `/game/:id` | Get details of a specific game |
| **POST** | `/game` | Create a new game |
| **PUT** | `/game/:id` | Update an existing game |
| **DELETE** | `/game/:id` | Delete a game |
| **GET** | `/game/:id/room` | List all rooms for a specific game |
| **POST** | `/game/:id/room` | Create a new room |
| **GET** | `/game/:id/room/:roomId` | Retrieve a room (includes state and players) |
| **DELETE** | `/game/:id/room/:roomId` | Delete a room |
| **POST** | `/game/:id/room/:roomId/player` | Add a player to the room |
| **DELETE** | `/game/:id/room/:roomId/player/:name` | Remove a player from the room |

Uses appropriate HTTP codes:  
`200 OK`, `201 Created`, `400 Bad Request`, `404 Not Found`, `500 Server Error`.

---

## 2. Game Server APIs (VM2)

Each game server follows a consistent REST structure:

| **Method** | **Endpoint** | **Description** |
|-------------|---------------|-----------------|
| **GET** | `/` | Returns game info |
| **GET** | `/room` | Lists all rooms for this game |
| **GET** | `/room/:id` | Retrieves a specific room object |
| **POST** | `/room` | Creates a new room  |
| **DELETE** | `/room/:id` | Deletes a specific room |
| **GET** | `/room/:id/state` | Returns the current game state for a given room |
| **GET** | `/room/:id/input` | Retrieves the list of player inputs for the current room |
| **POST** | `/room/:id/input` | Applies a player action to the game |
| **GET** | `/room/:id/tick` | Periodically updates the game state |
| **GET** | `/settings` | Returns configurable settings for the game |

---

### 2.1 Hello World Game

A simple click-to-count demonstration game.

#### **Behavior**
- Displays “{ Count Number } + Append Text”.
- Each click increases the counter by 1.

#### **Endpoints**

| **Method** | **Endpoint** | **Description** |
|-------------|---------------|-----------------|
| **GET** | `/room/:id/state` | Display current count |
| **POST** | `/room/:id/input` | Increase count |
| **GET** | `/room/:id/tick` | Return updated count |

---

### 2.2 Voting Game

A multiplayer voting game where players vote between multiple options .  
Votes are visualized as colored bars, and the leading option is highlighted in orange.

#### **Behavior**
- Each player can vote for one option.
- Clicking the same option again removes their vote.
- The current leader’s bar becomes orange.

#### **Endpoints**

| **Method** | **Endpoint** | **Description** |
|-------------|---------------|-----------------|
| **GET** | `/room/:id/state` | Displays voting bars |
| **POST** | `/room/:id/input` | Registers or toggles a vote |
| **GET** | `/room/:id/tick` | Updates visuals periodically |

---

### 2.3 Reaction Timer Game (Custom Game)

A custom reaction-speed test game.  
After a random delay (10–30 seconds), the screen changes to “CLICK NOW!”.  
The first player to click wins — clicking too early shows “Too Soon!”.

#### **Logic Flow**
1. Phase starts as `"ready"`.
2. Changes to `"wait"` → waits randomly between 10–30 seconds.
3. Then changes to `"go"` → screen turns green and shows “CLICK NOW!”.
4. The first valid click records the winner and reaction time.

#### **Endpoints**

| **Method** | **Endpoint** | **Description** |
|-------------|---------------|-----------------|
| **GET** | `/room/:id/state` | Shows “Wait for GREEN”, “CLICK NOW”, or winner |
| **POST** | `/room/:id/input` | Handles player click and timing logic |
| **GET** | `/room/:id/tick` | Manages timer and transitions between phases |

---

## 3. Web Frontend (VM1)

### Technology Stack
- Node.js + Express + EJS  
- HTTPS with self-signed certificate  

### Features

| **Function** | **Description** |
|---------------|-----------------|
| **List Games** | Retrieves all games using `GET /game` |
| **Create Game** | Adds a new game `POST /game` |
| **Delete Game** | Deletes selected games using `DELETE /game/:id` |
| **Error Handling** | Displays API error messages on the web page |
| **Secure HTTPS** | Uses HTTPS server with certificate |

#### **Workflow**
1. The frontend calls `GET /game` → loads game list.  
2. The user creates a game → `POST /game`.  
3. The user deletes a game → `DELETE /game/:id`.  
4. Errors from the API appear visibly in the browser.

---

### Optional: Restricted Access (x-api-key Header)

The Room Server can require an API key for restricted access.

---

## Reference

- **Link for Using AI (ChatGPT):**  https://chatgpt.com/share/69071a42-7dd4-800e-9edb-a5fde19bcea2

