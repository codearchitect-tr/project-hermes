# Project Hermes 📨

**A Zero-Configuration, Serverless LAN Chat Room.**

Hermes allows anyone on the same local network to instantly join a shared chat room by simply running a single Python script. No central server, no internet connection, no configuration required. It's magic.


![Sentinel Demo GIF](demo.gif)

## Features

-   **Serverless Architecture:** No central server is needed. Clients communicate directly with each other (Peer-to-Peer).
-   **Auto-Discovery:** Automatically discovers other Hermes users on the local network using UDP broadcasting.
-   **Real-time Chat:** Instant messaging in a shared terminal-based room.
-   **Cross-Platform:** Runs on any machine with Python 3.
-   **Lightweight:** Pure Python with no heavy dependencies.

## How It Works: The Magic of Auto-Discovery

The core of Hermes is its serverless discovery mechanism.

1.  **Broadcast:** When a client starts, it shouts a "discovery" message to every device on the network using a UDP broadcast on a specific port (`<IP_BROADCAST_ADDRESS>:PORT`). The message is simple: "Hermes client here! My name is codearchitect-tr."
2.  **Listen & Respond:** Every other running Hermes client is constantly listening on that same port. When they hear a discovery message, they add the new user to their list of participants and respond with their own presence message.
3.  **Chat:** All subsequent chat messages are also sent via UDP broadcast, so every client receives every message, creating a shared chat room.

## Architecture

```
           +---------------------------------------------+
           |           Your Local Network (LAN)          |
           |                                             |
+----------+---------+      +----------+---------+      +----------+---------+
|     Client A       |      |     Client B       |      |     Client C       |
| (You)              |      | (Your Friend)      |      | (Newcomer)         |
|                    |      |                    |      |                    |
|  [Sends "Hi All"]  |----->| [Receives "Hi All"]|      | [Receives "Hi All"]|
|  [Receives "Hey!"] |<-----| [Sends "Hey!"]     |      | [Receives "Hey!"]  |
+--------------------+      +--------------------+      +--------------------+
           |                                             |
           +---------------------------------------------+
```

## Tech Stack

-   **Core Logic:** Python 3
-   **Networking:** `socket` module (UDP for broadcasting, TCP could be an alternative for direct messages).
-   **Concurrency:** `threading` module (to listen for messages and accept user input simultaneously).

## Setup & Usage

**1. Clone the repository:**
```bash
git clone https://github.com/codearchitect-tr/project-hermes.git
cd project-hermes
```

**2. Run the application:**
Open a terminal and run the script. You will be prompted for a nickname.
```bash
python hermes_chat.py
```
**3. Connect with others:**
Have someone else on the same WiFi network do the same. You will see each other join the room and can start chatting immediately!

## Technical Deep Dive: The Main Challenge

The primary engineering challenge in Hermes is **concurrency**. The application must perform two tasks at the same time:
1.  **Listen continuously** for incoming broadcast messages from other clients.
2.  **Wait for the user** to type a message and press Enter.

This is solved by using Python's `threading` module. A separate thread is dedicated to listening to the network socket, while the main thread handles user input. This separation prevents the program from blocking and ensures a smooth, real-time experience.

## Future Enhancements

-   Implement a TCP-based direct messaging (private message) feature.
-   Add a simple encryption layer for messages.
-   Create a more sophisticated Terminal UI using a library like `curses`.
-   Allow file sharing between clients.
