# iPad Canvas

Share your iPad live to another device, straight from the browser. No app, no account, no server.

**Live site:** https://mv3d-ember.github.io/iPad-canvas/

## How to use

1. On the iPad, open the site and tap **Share**.
2. Pick what to share: **Drawing canvas**, **Camera**, or **Full screen** (where supported).
3. Tap the **Code** button at the top to see the room code, link and QR code.
4. On the other device, open the site and type the code, or scan the QR code / open the link.
5. Tap **Fullscreen** on the viewer. Tap **Stop** on the iPad when you're done.

## What works where

| | Full screen share | Drawing canvas | Camera | Watching |
|---|---|---|---|---|
| iPad / iPhone (any browser) | ❌ not allowed by iPadOS | ✅ | ✅ | ✅ |
| Mac / Windows / Android (Chrome, Edge, Firefox, Safari) | ✅ | ✅ | ✅ | ✅ |

Safari on iPad doesn't give websites `getDisplayMedia()`, and every iPad browser uses Safari's engine, so a web page **can't** capture the whole iPad screen. The site checks for support and falls back to:

- **Live drawing canvas**: Apple Pencil (with pressure) or touch, colours, brush size, eraser, undo/redo, clear, light/dark paper, "Pencil only" palm rejection, save as PNG. It's streamed with `canvas.captureStream()`.
- **Camera**: front or back, streamed with `getUserMedia()`.

To mirror the *whole* iPad screen, use iPadOS **Screen Mirroring** (AirPlay) or FaceTime screen sharing.

## How it works

- One static `index.html` (HTML/CSS/JS), hosted free on GitHub Pages over HTTPS.
- Video goes peer-to-peer over **WebRTC** using [PeerJS](https://peerjs.com/). PeerJS's free public cloud server only introduces the two devices to each other.
- The room code maps to a PeerJS ID (`ipadcanvas-mv3d-<code>`). The viewer connects with a data channel, then the sharer calls it with the video stream. Switching source swaps the track live with `replaceTrack()`.
- QR codes are made with [qrcodejs](https://github.com/davidshimjs/qrcodejs).

## Tips

- Keep Safari in the foreground while sharing. iPadOS pauses background pages, so the stream freezes if you switch apps.
- Same Wi-Fi works best. Some school or work networks block peer-to-peer traffic.
- If it gets stuck, tap **Try again** on the viewer.

## Run locally

Camera and screen capture need HTTPS or `localhost`:

```sh
python3 -m http.server 8000
# open http://localhost:8000
```
