# BitTorrent-Style Peer-to-Peer File Sharing

A Python networking project implementing a local tracker and peer swarm: bencoded metainfo, peer discovery, a TCP handshake, piece-based transfer, SHA-1 verification, concurrent peers, and a browser dashboard.

**Author:** [Amirali Yazdiani](https://github.com/amirali83) · **Type:** Individual Data Networks project

**[Read the project report](DN-Final-Project.pdf)** · **[View dashboard screenshot](latex/Images/dashboard.JPG)** · **[Inspect packet capture](WireSharkFiles/handshake.pcapng)**

![Swarm dashboard](latex/Images/dashboard.JPG)

## What to inspect

- [Tracker](Tracker.py): HTTP announce handling, swarm membership, and dashboard endpoints.
- [Peer](peer.py): listeners, handshakes, piece requests, integrity checks, and downloads.
- [Metainfo generator](Torrent_Handler/torrent_generator.py): JSON configuration to bencoded metainfo.
- [Logger](logger.py): structured tracker and peer activity.

## Run a local demonstration

Use **Python 3.10+**. Run commands from the repository root unless stated otherwise.

```bash
python -m venv .venv
# Activate .venv using your shell's activation command.
python -m pip install -r requirements.txt
python Tracker.py
```

In separate terminals with the same environment active:

```bash
python peer.py 1
python peer.py 2
```

Each peer creates a workspace such as `workspace_-AA0001-000000000001/` and chooses an available localhost port from 6881–6889. Before requesting an example file, copy `Torrent_Handler/Transferring_These_Files/` into **peer 1's workspace**; this makes peer 1 the seed. In each peer's prompt, enter:

```text
download 1
```

Register the seeded peer first, then request the same torrent in the second peer. Open **http://127.0.0.1:8000/dashboard** to inspect the swarm. Use `exit` to stop a peer and Ctrl+C to stop the tracker.

The checked-in metainfo files already describe the sample images. To regenerate them after changing sample files, run `python torrent_generator.py` from `Torrent_Handler/`. The `.txt` metainfo files contain bencoded bytes, not plain-text summaries.

## Repository guide

| Location | Contents |
| --- | --- |
| `Tracker.py`, `peer.py`, `logger.py` | Networking implementation |
| `Torrent_Handler/` | Example files, JSON configurations, and metainfo generator |
| `Desktop.html` | Live dashboard served by the tracker |
| `DN-Final-Project.pdf` | Project report |
| `WireSharkFiles/` | Recorded packet-level investigation |
| `latex/` | Report source and figures |

## Scope

The implementation is a **local educational protocol**, not a production BitTorrent client. Peer listeners and dashboard traffic use localhost. Logs and per-peer workspaces are generated at runtime and excluded from Git.

[Explore my other projects →](https://github.com/amirali83?tab=repositories)
