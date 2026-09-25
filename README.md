# iOS 27 microphone goes silent: test case (plain JS)

A test case for an issue in Safari on iOS / iPadOS 27.0 where a microphone being sent over WebRTC goes silent.
When the only media element on the page is a MediaStream `<video>` waiting to start playing through `autoplay`,
the microphone stops delivering audio while its track stays `live` and `muted == false`, and the track later becomes `ended`.

## Files

| File | Contents |
|---|---|
| `index.html` | Test case to attach to the bug report: the bug and the workaround (calling `play()`), with an automatic verdict and a log |

## Running

Serve `index.html` over HTTPS (e.g. GitHub Pages) and open it on an iPhone / iPad.

## Checking (index.html)

1. Tap **Start** and keep talking.
2. Tap **Remount (bug)** or **Remount + play() (workaround)** and keep talking. The remount runs from a `setTimeout(0)` callback, not inside the click handler (if it runs directly inside the click handler, the bug reproduces only sometimes).
3. A verdict appears within a few seconds (`REPRODUCED` after 3 silent seconds in a row, `not reproduced` after 5 seconds of audio in a row).
4. Copy the log with **Copy log**, reload the page, and try the other button.

| Button | Result on iOS 27.0 |
|---|---|
| Remount (bug) | Reproduced |
| Remount + play() (workaround) | Not reproduced |

- Reload the page before each check (a broken capture does not recover).
- Keep Safari in the foreground while checking (going to the background affects capture through a different path).
