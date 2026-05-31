# P2P File Sharing System

A BitTorrent-inspired peer-to-peer file sharing system with a centralized tracker server, piece-based file transfer, and SHA1 integrity verification.

## Tech Stack

- **Language:** Python
- **Tracker Server:** Flask
- **Database:** SQLite
- **Networking:** Socket programming (TCP), multi-threading
- **Protocol:** BitTorrent-inspired (Bencodepy, magnet links)

## Features

- Centralized tracker server to manage peers and file metadata
- File splitting into pieces with SHA1 hash verification per piece
- Torrent metadata generation and magnet link support
- Multi-threaded peer connections for concurrent file transfer
- Full file reconstruction from distributed pieces across peers
- Flask web interface for file upload and download

## Project Structure

```
├── app.py           # Tracker server (Flask)
├── node.py          # Peer node implementation
├── metadata.json    # File-to-magnet mapping
├── tracker/         # Files stored on tracker
├── peer1/           # Peer 1 file directory
├── peer2/           # Peer 2 file directory
├── peer3/           # Peer 3 file directory
├── uploads/         # Upload staging directory
└── templates/       # Flask HTML templates
```

## Getting Started

### Prerequisites

- Python >= 3.8
- pip

### Installation

```bash
pip install flask bencodepy
```

### Running

**1. Start the tracker server:**

```bash
python app.py
```

Tracker runs on `http://localhost:5000`.

**2. Start peer nodes** (in separate terminals):

```bash
python node.py
```

**3. Upload and share files** via the web interface at `http://localhost:5000`.

## Architecture

```
[Peer 1] ──┐
[Peer 2] ──┼──► [Tracker Server] ──► metadata.json
[Peer 3] ──┘         │
                      └──► Manages peer list & torrent metadata
```

- **Tracker:** Maintains active peer list and file distribution metadata
- **Peers:** Store file pieces locally and serve them to requesting peers
- **Transfer:** Peers discover each other via tracker, then exchange pieces directly

## Key Functions

| Function | Description |
|----------|-------------|
| `split_file_into_pieces()` | Breaks file into fixed-size chunks |
| `create_pieces_string()` | Generates SHA1 hash string for all pieces |
| `merge_pieces_into_file()` | Reconstructs original file from pieces |
| `new_connection()` | Handles peer-tracker handshake |
| `handle_request()` | Processes tracker ping and status requests |
