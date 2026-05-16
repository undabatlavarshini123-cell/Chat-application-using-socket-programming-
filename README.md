#include <iostream>
#include <cstring>
#include <unistd.h>
#include <arpa/inet.h>

using namespace std;

void runServer() {
    int server_fd, client_socket;
    struct sockaddr_in address;
    int addrlen = sizeof(address);
    char buffer[1024];

    server_fd = socket(AF_INET, SOCK_STREAM, 0);
    if (server_fd == 0) {
        cout << "Socket failed\n";
        return;
    }

    address.sin_family = AF_INET;
    address.sin_addr.s_addr = INADDR_ANY;
    address.sin_port = htons(8888);

    if (bind(server_fd, (struct sockaddr *)&address, sizeof(address)) < 0) {
        cout << "Bind failed\n";
        return;
    }

    listen(server_fd, 3);
    cout << "Server waiting for connection...\n";

    client_socket = accept(server_fd, (struct sockaddr *)&address, (socklen_t*)&addrlen);
    if (client_socket < 0) {
        cout << "Accept failed\n";
        return;
    }

    cout << "Client connected!\n";

    while (true) {
        memset(buffer, 0, sizeof(buffer));
        read(client_socket, buffer, 1024);
        cout << "Client: " << buffer << endl;

        cout << "Enter message: ";
        cin.getline(buffer, 1024);
        send(client_socket, buffer, strlen(buffer), 0);
    }

    close(server_fd);
}

void runClient() {
    int sock = 0;
    struct sockaddr_in serv_addr;
    char buffer[1024];

    sock = socket(AF_INET, SOCK_STREAM, 0);
    if (sock < 0) {
        cout << "Socket creation error\n";
        return;
    }

    serv_addr.sin_family = AF_INET;
    serv_addr.sin_port = htons(8888);

    if (inet_pton(AF_INET, "127.0.0.1", &serv_addr.sin_addr) <= 0) {
        cout << "Invalid address\n";
        return;
    }

    if (connect(sock, (struct sockaddr *)&serv_addr, sizeof(serv_addr)) < 0) {
        cout << "Connection failed\n";
        return;
    }

    cout << "Connected to server!\n";

    while (true) {
        cout << "Enter message: ";
        cin.getline(buffer, 1024);
        send(sock, buffer, strlen(buffer), 0);

        memset(buffer, 0, sizeof(buffer));
        read(sock, buffer, 1024);
        cout << "Server: " << buffer << endl;
    }

    close(sock);
}

int main() {
    int choice;

    cout << "1. Run as Server\n";
    cout << "2. Run as Client\n";
    cout << "Enter choice: ";
    cin >> choice;
    cin.ignore(); // clear newline

    if (choice == 1) {
        runServer();
    } else if (choice == 2) {
        runClient();
    } else {
        cout << "Invalid choice\n";
    }

    return 0;
}