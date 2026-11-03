# 2b IMPLEMENTATION OF SLIDING WINDOW PROTOCOL
## AIM
## ALGORITHM:
1. Start the program.
2. Get the frame size from the user
3. To create the frame based on the user request.
4. To send frames to server from the client side.
5. If your frames reach the server it will send ACK signal to client
6. Stop the Program
## PROGRAM
client
```
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

s.connect(('localhost', 8001))

window_size = 3

total_frames = int(input("Enter number of frames: "))

i = 1

while i <= total_frames:

    print("\nSending frames:")

    for j in range(i, min(i + window_size, total_frames + 1)):
        print("Frame", j, "sent")
        s.send(str(j).encode())

        ack = s.recv(1024).decode()
        print(ack)

    i += window_size

s.send("exit".encode())

s.close()
```
server
```
import socket

s = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

s.setsockopt(socket.SOL_SOCKET, socket.SO_REUSEADDR, 1)

s.bind(('localhost', 8001))

s.listen(5)

print("Server is waiting for connection...")

c, addr = s.accept()

print("Connected to:", addr)

while True:
    data = c.recv(1024).decode()

    if not data or data.lower() == "exit":
        print("Connection closed")
        break

    print("Received Frame:", data)

    ack = "ACK for Frame " + data

    c.send(ack.encode())

    print("Acknowledgement sent:", ack)

c.close()
s.close()
```
## OUPUT
client
<img width="1917" height="1078" alt="Screenshot 2026-11-03 143505" src="https://github.com/user-attachments/assets/2f9a2710-02a4-424b-8d8c-154e98dafd1f" />
server
<img width="1917" height="1078" alt="Screenshot 2026-11-03 143456" src="https://github.com/user-attachments/assets/b6c83abf-7d49-4613-a0f6-ad656de9b547" />


## RESULT
Thus, python program to perform stop and wait protocol was successfully executed
