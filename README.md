# 2c.SIMULATING ARP /RARP PROTOCOLS
## AIM
To write a python program for simulating ARP protocols using TCP.
## ALGORITHM:
## Client:
1. Start the program
2. Using socket connection is established between client and server.
3. Get the IP address to be converted into MAC address.
4. Send this IP address to server.
5. Server returns the MAC address to client.
## Server:
1. Start the program
2. Accept the socket which is created by the client.
3. Server maintains the table in which IP and corresponding MAC addresses are
stored.
4. Read the IP address which is send by the client.
5. Map the IP address with its MAC address and return the MAC address to client.
P
## PROGRAM - ARP
### ARP SERVER.PY
```
import socket
s = socket.socket()
s.bind(('localhost', 8000))
s.listen(5)
print("Server is listening...")

c, addr = s.accept()
print(f"Connection established with {addr}")

address = {
    "165.165.80.80": "6A:08:AA:C2",
    "165.165.79.1": "8A:BC:E3:FA"
}

while True:
    ip = c.recv(1024).decode()

    if not ip:  
        break

    try:
        mac = address[ip]  
        print(f"IP: {ip} -> MAC: {mac}")
        c.send(mac.encode())  
    except KeyError:
        print(f"IP: {ip} not found in ARP table.")
        c.send("Not Found".encode())
c.close()
s.close()
```

### ARP CLIENT.PY
```
import socket
c = socket.socket()
c.connect(('localhost', 8000))

while True:
    ip = input("Enter IP address to find MAC (or type 'exit' to quit): ")

    if ip.lower() == "exit":  
        break

    c.send(ip.encode())
    mac = c.recv(1024).decode()
    print(f"MAC Address for {ip}: {mac}")
c.close()

```
## OUPUT - ARP
<img width="1534" height="340" alt="image" src="https://github.com/user-attachments/assets/1e03b22e-099f-4a9c-b6f2-c120fd701895" />

## PROGRAM - RARP
### RARP SERVER.PY
```
import socket

rarp_table = {
    "AA:BB:CC:DD:EE:01": "192.168.1.1",
    "AA:BB:CC:DD:EE:02": "192.168.1.2",
    "AA:BB:CC:DD:EE:03": "192.168.1.3"
}

server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.bind(("localhost", 9998))
server_socket.listen(1)

print("RARP Server is running...")

conn, addr = server_socket.accept()
print(f"Connection from {addr}")

while True:
    mac_address = conn.recv(1024).decode()
    if not mac_address:
        break
    print(f"Received MAC: {mac_address}")

    ip_address = rarp_table.get(mac_address, "IP not found")
    conn.send(ip_address.encode())

conn.close()
server_socket.close()
```

### RARP CLIENT.PY
```
import socket

client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect(("localhost", 9998))

mac_address = input("Enter MAC Address to find IP: ")
client_socket.send(mac_address.encode())

ip_address = client_socket.recv(1024).decode()
print(f"IP Address for {mac_address} is: {ip_address}")

client_socket.close()
```
## OUPUT -RARP
<img width="1454" height="294" alt="image" src="https://github.com/user-attachments/assets/cae096d9-3b0f-4b01-a1ba-074648f162e0" />

## RESULT
Thus, the python program for simulating ARP protocols using TCP was successfully 
executed.
