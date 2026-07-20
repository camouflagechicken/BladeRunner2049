# Decentralized HLS Cinema Architecture

A minimalist, highly optimized web theater designed to stream massive video payloads using a decentralized data pipeline. 

## The Architecture
This project circumvents standard web hosting limits (such as GitHub's 100MB file cap) by decoupling the frontend UI from the backend data core:
* **Frontend (GitHub Pages):** A lightweight, zero-dependency HTML/CSS/JS player utilizing `hls.js`.
* **Data Core (Hugging Face):** The monolithic video file is cleaved into 983 separate `.ts` transport stream chunks and hosted in a specialized machine-learning dataset repository.

## Core Features
* **HLS Streaming:** Dynamically requests timestamped video chunks rather than loading a massive Blob into browser VRAM, preventing out-of-memory fatal crashes on mobile devices.
* **State Persistence:** Utilizes `localStorage` to log the playback position and prompt a resume state upon returning to the URL.
* **Mobile Optimization:** Implements the Screen Wake Lock API to prevent device sleep, alongside custom touch gestures (double-tap to skip, vertical screen swipe for brightness and volume control).
* **Zero-Dependency UI:** All interface icons are drawn using inline mathematical SVG paths to eliminate external asset loading times.
* **Hardware Hotkeys:** Full keyboard mapping for playback (Space/K), scrubbing (J/L), volume mapping (Arrows), Picture-in-Picture (P), and theater mode (T). 

## The Pipeline
1. **Cleave:** Original MP4 processed via FFmpeg into 10-second `.ts` chunks with a master `index.m3u8` ledger.
2. **Push:** Chunks uploaded asynchronously to a Hugging Face dataset via the Python `huggingface_hub` CLI (`upload-large-folder`) to bypass standard Git HTTP buffer timeouts.
3. **Render:** The frontend reads the `.m3u8` ledger and reconstructs the data stream chronologically in the browser.
