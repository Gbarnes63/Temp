
sequenceDiagram
    autonumber
    participant Client
    participant Queue as Message Queue
    participant Server

    Note over Client,Server: Setup
    Client->>Queue: Connect to queue
    Server->>Queue: Create/Connect to queue

    loop Request cycle
        Client->>Queue: Send request (original string + instruction)
        Queue-->>Server: Deliver message
        Server->>Server: Process request (remove substring)
        Server->>Queue: Return processed string
        Queue->>Client: Return processed string
    end

    alt Shutdown
        Client->>Queue: Send "exit"
        Queue-->>Server: Deliver "exit"
        Server->>Server: Exit loop and perform cleanup
    end

    Note over Client,Server: Disconnect
    Client->>Queue: Disconnect
    Server->>Queue: Disconnect and remove queue
