# HTTP Client-Server Implementation

A lightweight, multithreaded HTTP client-server implementation written in Python, demonstrating core networking concepts including socket programming, HTTP protocol handling, and concurrent connection management.

## Overview

This project implements a functional HTTP server and client capable of handling GET and POST requests. The server supports static file serving with proper MIME type handling, connection pooling, and request/response processing. The client can execute multiple HTTP requests from input files and handle various file types.

## Project Structure

```
HTTP-Client-Server/
├── README.md                          # This file
├── src/                               # Source code
│   ├── server/
│   │   ├── server.py                 # HTTP server implementation
│   │   └── root/                     # Server root directory (served files)
│   │       └── index.html
│   └── client/
│       ├── client.py                 # HTTP client implementation
│       └── root/                     # Client local files directory
├── tests/                             # Test files and test suite
│   ├── test_client.py
│   ├── input.txt                     # Sample requests (GET/POST)
│   ├── input2.txt
│   └── input3.txt
├── docs/                              # Documentation
│   └── Networks Programming Assignment 1.pdf
├── assets/                            # Static assets and diagrams
│   └── myplot.png
└── .git/                              # Git repository
```

## Features

### Server (`src/server/server.py`)

- **Multithreaded Architecture**: Handles multiple concurrent client connections using Python threading
- **HTTP Protocol Support**: Implements GET and POST request handling
- **Static File Serving**: Serves files from the `src/server/root/` directory
- **MIME Type Detection**: Automatically detects and sends appropriate content types (HTML, plain text, images, etc.)
- **Connection Management**: Tracks active connections with thread-safe operations using locks
- **Request Validation**: Validates HTTP requests and returns appropriate status codes
- **Graceful Request Handling**: Supports request timeouts and connection limits

### Client (`src/client/client.py`)

- **HTTP Request Formation**: Constructs valid HTTP GET and POST requests
- **File Type Detection**: Identifies file types for appropriate request handling
- **Batch Request Processing**: Executes multiple requests from input files
- **Response Parsing**: Splits and processes HTTP response headers and body
- **Error Handling**: Manages file not found and connection errors gracefully

## Prerequisites

- **Python 3.6+**
- Standard library modules: `socket`, `sys`, `threading`, `datetime`, `pytz`
- No external dependencies required

## Installation & Setup

1. **Clone the repository**:
   ```bash
   git clone https://github.com/saifullah-m/HTTP-Client-Server.git
   cd HTTP-Client-Server
   ```

2. **Verify Python installation**:
   ```bash
   python3 --version
   ```

## Usage

### Starting the Server

Run the server on a specific port (default: 8080):

```bash
cd src/server/
python3 server.py <port>
```

**Example**:
```bash
python3 server.py 8080
```

The server will:
- Listen for incoming connections on the specified port
- Display connection information and request details
- Serve static files from the `root/` directory
- Maintain connection logs

### Running the Client

Execute HTTP requests from an input file:

```bash
cd src/client/
python3 client.py <host> <port> <input_file>
```

**Example**:
```bash
python3 client.py localhost 8080 ../tests/input.txt
```

### Input File Format

The client processes instructions from text files. Each line contains a request specification:

```
<method> <file_path> <host> <port>
```

**Supported Methods**:
- `client_get` - HTTP GET request
- `client_post` - HTTP POST request

**Example Input File** (`tests/input.txt`):
```
client_get index.html 123 123
client_post test.txt 123 123
client_get macbook.webp 123 123
```

**Note**: Host and port parameters in input files are parsed for flexibility; the actual connection uses the host/port provided as command-line arguments.

### Serving Static Files

1. Place files in `src/server/root/` directory
2. Files will be automatically served by the HTTP server
3. Supported file types: HTML, CSS, plain text, images (JPG, PNG, WebP, etc.)

## Testing

Run the included test suite:

```bash
cd tests/
python3 test_client.py
```

Test files included:
- **input.txt** - Basic GET/POST requests
- **input2.txt** - Additional test scenarios
- **input3.txt** - Extended test cases

## How to Test Manually

1. **Terminal 1 - Start Server**:
   ```bash
   cd src/server/
   python3 server.py 8080
   ```

2. **Terminal 2 - Run Client**:
   ```bash
   cd src/client/
   python3 client.py localhost 8080 ../tests/input.txt
   ```

3. **Observe Output**:
   - Server console displays incoming requests and responses sent
   - Client console shows responses received from the server

## Technical Details

### HTTP Protocol Implementation

- **Request Format**: Follows HTTP/1.1 specification
- **Response Headers**: Includes Content-Type, Content-Length, Status codes
- **Status Codes**: 200 (OK), 404 (Not Found), 400 (Bad Request)
- **Request/Response Parsing**: Uses CRLF (`\r\n`) as line separators per HTTP spec

### Threading & Concurrency

- **Thread-Safe Operations**: Uses `threading.Lock()` for managing shared resources
- **Connection Counter**: Thread-safe tracking of active connections
- **Request Timeout**: Implements 20-second timeout for inactivity
- **Concurrent Handling**: Each client connection runs in a separate thread

### MIME Type Mapping

The server automatically determines content types:
- `.html` → `text/html`
- `.txt` → `text/plain`
- `.jpg`, `.jpeg` → `image/jpeg`
- `.png` → `image/png`
- `.webp` → `image/webp`
- `.css` → `text/css`

## Example Scenarios

### Scenario 1: GET Request
```
Client sends: GET /index.html HTTP/1.1
Server responds with: 200 OK
Returns: HTML file content
```

### Scenario 2: POST Request
```
Client sends: POST /test.txt HTTP/1.1
Server responds with: 200 OK (or 404 if file not found)
```

### Scenario 3: File Not Found
```
Client requests: GET /nonexistent.html HTTP/1.1
Server responds with: 404 Not Found
```

## Troubleshooting

| Issue | Solution |
|-------|----------|
| **Port already in use** | Use a different port number: `python3 server.py 9000` |
| **Connection refused** | Ensure server is running before starting client |
| **File not found** | Verify file exists in `src/server/root/` directory |
| **Permission denied** | Check file permissions or run with appropriate privileges |
| **Module not found** | Verify Python version and installed packages |

## Performance Considerations

- **Connection Limit**: Server can handle multiple concurrent connections (default: 20-second timeout)
- **Buffer Size**: Receives up to 2048 bytes per request
- **Thread Pool**: Dynamically creates threads per client connection
- **Scalability**: Suitable for educational purposes; production use would require additional optimization

## Assignment Context

This project was developed as part of a Networks Programming assignment, focusing on:
- Socket programming fundamentals
- HTTP protocol implementation
- Client-server architecture
- Multithreaded server design
- Network communication protocols

For assignment details, see [Networks Programming Assignment 1.pdf](docs/Networks%20Programming%20Assignment%201.pdf)
