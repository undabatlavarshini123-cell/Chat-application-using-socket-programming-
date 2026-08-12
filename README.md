# Chat Application Using Socket Programming

## 📌 Description

A simple **C++ client-server chat application** developed using **TCP socket programming**. The user can choose to run the program as either a **Server** or **Client** and exchange messages in real time.

## 🛠️ Technologies Used

* C++
* TCP/IP Socket Programming
* Linux Socket APIs

## ⚙️ Features

* Client-server communication
* Real-time message exchange
* TCP connection using sockets
* Server and client modes in a single program
* Basic socket error handling

## 📂 File

```text
chat.cpp
```

## ▶️ Run

Compile using:

```bash
g++ chat.cpp -o chat
```

Run:

```bash
./chat
```

Then choose:

```text
1. Run as Server
2. Run as Client
```

The server uses **port 8888** and the client connects through **127.0.0.1 (localhost)**.

## 📚 Concepts Used

`socket()` • `bind()` • `listen()` • `accept()` • `connect()` • `send()` • `read()` • `close()`

## 🎯 Objective

To understand the basic working of **TCP/IP communication and client-server architecture using C++ socket programming**.

