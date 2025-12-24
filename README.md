<div align="center">

# 🚀 Build-My-Own-Redis

[![Typing SVG](https://readme-typing-svg.herokuapp.com?font=Fira+Code&weight=600&size=24&duration=3000&pause=1000&color=F7F7F7&center=true&vCenter=true&width=700&lines=Build+My+Own+Redis;A+Lightweight+Key-Value+Store;Implemented+Purely+in+Java+%E2%98%95;Supports+Streams%2C+Transactions+%26+More!;)](https://git.io/typing-svg)

![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Redis](https://img.shields.io/badge/redis-%23DD0031.svg?style=for-the-badge&logo=redis&logoColor=white)
![Socket Programming](https://img.shields.io/badge/Socket-Programming-blue?style=for-the-badge)

<br>

**Build-My-Own-Redis** is a custom-built, lightweight, in-memory key-value store inspired by Redis, implemented purely in **Java**! ☕

This project is a deep dive into the **core concepts of distributed systems**, including **socket programming 🔌**, **multithreading 🧵**, and the **Redis Serialization Protocol (RESP) 📜**. It simulates a real Redis server, handling client connections, parsing commands, and managing replication!

</div>

---

## ✨ Key Features

This isn't just a simple key-value map; it's a feature-rich server!

-   **🔑 Core Key-Value Operations**: Support for `SET` (with `EX`/`PX` expiry), `GET`, `INCR`, and `KEYS`.
-   **🌊 Redis Streams Support**: Full implementation of `XADD`, `XRANGE`, and `XREAD` (including blocking reads!) for event-driven architectures.
-   **👯 Replication (Master-Slave)**:
    -   Run instances as a **Master** or a **Replica**.
    -   Dynamic handshake process (`PING`, `REPLCONF`, `PSYNC`).
    -   Real-time command propagation from Master to Replicas.
-   **💾 RDB Persistence**: Capable of parsing and loading Redis Database (RDB) files on startup to restore state.
-   **⚡ Transactions**: Atomic execution of command blocks using `MULTI`, `EXEC`, and `DISCARD`.
-   **🧵 Multithreaded Architecture**: Handles multiple concurrent client connections using a dedicated thread-per-client model.
-   **🧹 Automatic Expiry**: Background thread monitoring to automatically clean up expired keys.

---

## 🏗️ Architecture Under the Hood

The project is structured to be modular and easy to understand:

| Component | Description |
| :--- | :--- |
| **`Main.java`** | The entry point. Initializes the server, handles RDB loading, and accepts incoming connections. |
| **`ClientHandler.java`** | A dedicated thread for each connected client. Parses RESP commands and routes them to specific handlers. |
| **`SetGetHandler.java`** | Manages `SET`, `GET`, and `INCR` logic, including expiry management and Master-Slave propagation. |
| **`StreamHandler.java`** | Handles complex Stream operations (`XADD`, `XRANGE`), ensuring IDs are strictly increasing. |
| **`ReplicaClient.java`** | Logic for Replicas to connect to a Master, perform the handshake, and process incoming replication commands. |
| **`RDBConfig/KeyHandler`** | Parsers for reading the binary RDB file format to load initial dataset. |

---

## 🚀 Getting Started

Follow these steps to get your own Redis server running!

### Prerequisites
-   **Java Development Kit (JDK) 8** or higher.

### 🛠️ Compilation

Navigate to the source directory and compile the Java files:

```bash
cd src
javac Main/Main/*.java
```

### 🏃‍♂️ Running the Server

#### 1. Run as Master (Default)
Start the server on the default port `6379`:

```bash
java Main.Main
```

You can also specify a custom port:

```bash
java Main.Main --port 6380
```

#### 2. Run as Replica
Start a server as a replica of another instance (e.g., Master on 6379). This instance will sync data from the master!

```bash
java Main.Main --port 6381 --replicaof localhost 6379
```

---

## 🎮 Usage Examples

Connect using `redis-cli` or any TCP client (like `netcat`):

### 1. Basic Operations
```redis
> SET mykey "Hello World"
OK
> GET mykey
"Hello World"
> SET temp "expired" EX 5
OK
(Wait 5 seconds...)
> GET temp
(nil)
```

### 2. Streams
```redis
> XADD mystream * sensor-id 1234 temperature 19.8
"1638543925000-0"
> XRANGE mystream - +
1) 1) "1638543925000-0"
   2) 1) "sensor-id"
      2) "1234"
      3) "temperature"
      4) "19.8"
```

### 3. Transactions
```redis
> MULTI
OK
> INCR counter
QUEUED
> INCR counter
QUEUED
> EXEC
1) (integer) 1
2) (integer) 2
```

---

## 📚 Supported Commands

### String Operations
-   `PING`: Check server health.
-   `ECHO [message]`: Echoes the message back.
-   `SET [key] [value] [EX seconds] [PX millis] [NX|XX]`: Set a key with optional expiry and conditions.
-   `GET [key]`: Get the value of a key.
-   `INCR [key]`: Increment the integer value of a key.
-   `KEYS *`: List all keys.

### Stream Operations
-   `XADD [key] [ID] [field value ...]`: Appends a new entry to a stream.
-   `XRANGE [key] [start] [end]`: Query a range of entries from a stream.
-   `XREAD [BLOCK ms] STREAMS [key ...] [ID ...]`: Read data from one or multiple streams, with blocking support.

### Server & Replication
-   `INFO replication`: Show master/slave status.
-   `REPLCONF`: Used during replication handshake.
-   `PSYNC`: Initiates replication synchronization.
-   `WAIT`: Wait for synchronous replication.
-   `CONFIG GET [param]`: Get configuration parameters (dir, dbfilename).

### Transactions
-   `MULTI`: Start a transaction block.
-   `EXEC`: Execute all queued commands.
-   `DISCARD`: Discard the transaction.

---
