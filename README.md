# Go Load Balancer

A lightweight, high-performance HTTP load balancer implemented in Go using the round-robin algorithm. This load balancer distributes incoming HTTP requests across multiple backend servers to optimize resource utilization and improve application reliability.

## � Demo

![Load Balancer Demo](screenshot/Screenshot%202026-01-15%20015000.png)

## �🚀 Features

- **Round-Robin Algorithm**: Evenly distributes requests across all available backend servers
- **Reverse Proxy**: Built on Go's `httputil.ReverseProxy` for efficient request forwarding
- **Health Check Ready**: Infrastructure for server health monitoring (extensible)
- **Simple Configuration**: Easy to add or remove backend servers
- **Zero External Dependencies**: Uses only Go standard library

## 📋 Prerequisites

- Go 1.25.4 or higher
- Basic understanding of HTTP and load balancing concepts

## 🔧 Installation

1. Clone the repository:
```bash
git clone https://github.com/yourusername/go-loadbalancer.git
cd go-loadbalancer
```

2. Initialize Go modules (if needed):
```bash
go mod download
```

## 🏃 Usage

### Running the Load Balancer

```bash
cd src
go run main.go
```

The load balancer will start on `localhost:8000` by default.

### Testing the Load Balancer

Open your browser or use curl to test:

```bash
curl http://localhost:8000
```

Each request will be forwarded to a different backend server in a round-robin fashion.

## ⚙️ Configuration

To modify the backend servers, edit the `servers` slice in `main.go`:

```go
servers := []Server{
    newSimpleServer("https://www.google.com"),
    newSimpleServer("https://www.bing.com"),
    newSimpleServer("https://www.duckduckgo.com"),
}
```

To change the port:

```go
lb := NewLoadBalancer("8000", servers) // Change "8000" to your desired port
```

## 🏗️ Architecture

### Components

- **Server Interface**: Defines the contract for backend servers
  - `Address()`: Returns the server address
  - `IsAlive()`: Checks server health status
  - `Serve()`: Handles the request forwarding

- **simpleServer**: Concrete implementation of the Server interface
  - Wraps `httputil.ReverseProxy` for request forwarding

- **LoadBalancer**: Core load balancing logic
  - Maintains a list of backend servers
  - Implements round-robin selection algorithm
  - Routes incoming requests to available servers

### Request Flow

```
Client Request
    ↓
Load Balancer (localhost:8000)
    ↓
Round-Robin Selection
    ↓
Backend Server (Reverse Proxy)
    ↓
Response to Client
```

## 🔍 How It Works

1. **Client Request**: A client sends an HTTP request to the load balancer
2. **Server Selection**: The load balancer uses round-robin algorithm to select the next available server
3. **Request Forwarding**: The request is forwarded to the selected backend server via reverse proxy
4. **Response**: The backend server's response is sent back to the client

## 🛠️ Development

### Project Structure

```
go-loadbalancer/
├── go.mod              # Go module definition
├── README.md           # Project documentation
└── src/
    └── main.go         # Main application code
```

### Extending the Load Balancer

**Add Health Checks**: Implement actual health checking in the `IsAlive()` method:
```go
func (s *simpleServer) IsAlive() bool {
    resp, err := http.Head(s.addr)
    if err != nil {
        return false
    }
    return resp.StatusCode == http.StatusOK
}
```

**Add Different Load Balancing Algorithms**: Implement weighted round-robin, least connections, or IP hash algorithms.

**Add Logging**: Integrate a logging framework for better observability.

**Add Metrics**: Implement request counting, latency tracking, and server performance metrics.

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the project
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## 📝 License

This project is open source and available under the [MIT License](LICENSE).

## 👤 Author

Your Name - [@Tanishq4501](https://github.com/Tanishq4501)

## 🙏 Acknowledgments

- Built with Go's standard library
- Inspired by production load balancers like NGINX and HAProxy

## 📚 Resources

- [Go net/http/httputil Documentation](https://pkg.go.dev/net/http/httputil)
- [Load Balancing Algorithms](https://en.wikipedia.org/wiki/Load_balancing_(computing))
- [Reverse Proxy Pattern](https://en.wikipedia.org/wiki/Reverse_proxy)
