# Quick Start Guide

Get your HTTP Client-Server up and running in 3 minutes!

## Prerequisites
- Python 3.6 or higher

## Step 1: Start the Server

Open a terminal and run:

```bash
cd src/server/
python3 server.py 8080
```

You should see output indicating the server is listening on port 8080.

## Step 2: Run the Client (New Terminal)

Open another terminal and run:

```bash
cd src/client/
python3 client.py localhost 8080 ../tests/input.txt
```

## Step 3: Observe the Results

- **Server terminal**: Shows incoming GET/POST requests and responses sent
- **Client terminal**: Displays responses received from the server

## What Happened?

The client processed the requests in `tests/input.txt`:
- `client_get index.html` - Fetched the HTML homepage
- `client_post test.txt` - Posted a text file
- Additional requests as specified in the input file

## Troubleshooting

**Port already in use?**
```bash
python3 server.py 9000  # Use a different port
```

Then update the client call:
```bash
python3 client.py localhost 9000 ../tests/input.txt
```

**Connection refused?**
- Make sure the server is running before starting the client
- Verify the port numbers match on both client and server

## Try Custom Requests

1. Create a new file in `src/client/root/`
2. Edit `tests/input.txt` to request it
3. Run the client again

## Project Structure

- `src/server/` - Server code and served files
- `src/client/` - Client code and local files
- `tests/` - Test files and test suite
- `docs/` - Assignment documentation
- `assets/` - Diagrams and resources

---

For detailed information, see [README.md](README.md)
