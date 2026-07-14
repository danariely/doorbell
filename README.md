# 🔔 QR Doorbell

A QR code on the door replaces the doorbell. A visitor scans it with any phone
and taps one button:

1. Dan's phone gets an **urgent Pushover notification** ("Someone is at the door")
   within a couple of seconds, wherever he is.
2. The visitor stays on the page and can *optionally* attach a **photo** (their
   phone camera opens, the picture arrives inside a Pushover notification) or a
   short **note** ("חבילה מהסופר").

No app, no account, nothing installed for the visitor. Dan's phone number is
never exposed. There is no video call — an earlier version had a Jitsi video
room, but it was removed 2026-07-14 for simplicity (history is in git if it's
ever wanted again).

**Live page:** https://danariely.github.io/doorbell/

## Dan's phone setup (done 2026-07-14)

- **Primary ring: Pushover** (device `dan26iphone`). The page sends via the
  "Doorbell" Pushover application (priority 1, persistent sound). Photos arrive
  as image attachments in the notification.
- **Backup channel: ntfy** topic `studio-door-f474cd56` — the page still pings
  the ring (not photos/notes); useful for debugging
  (`curl -s "https://ntfy.sh/studio-door-f474cd56/json?poll=1"`) or as a second
  subscriber. ntfy delivery proved unreliable on Dan's iPhone (messages arrived
  in-app, no notifications), which is why Pushover is primary.

## Pieces

| Piece | What it is |
|---|---|
| `index.html` | The doorbell page (GitHub Pages, repo `danariely/doorbell`) |
| `print-card.html` | Printable door card with the QR — open and print |
| `qr-doorbell.png` | The bare QR code (points at the live page) |
| Pushover app token `az4p…` + user key | Primary push (rotate at pushover.net if ever abused) |
| ntfy topic `studio-door-f474cd56` | Backup push channel via ntfy.sh |

## Design decisions & caveats

- **Ring first, extras after** — the Pushover ping fires on the first tap and
  never waits for the photo/note, so a courier in a hurry still rings you.
- **Photos are downscaled on-device** (max 1280 px, JPEG) before being sent as
  `attachment_base64`, so they send fast even on cellular.
- **Anyone who scans can ring** — that is the point of a doorbell, but it also
  means the page URL itself is the "button". If the public Pushover token is
  ever abused, regenerate it at pushover.net and update `index.html`.
- **No number exposure** — nothing on the page reveals a phone number or email.
- **Deploy** = edit files here, then `git push` (repo `danariely/doorbell`,
  GitHub Pages serves from `main`).

## Testing the bell

Open the live page, tap the button, and Dan's phone should buzz within ~2
seconds. To watch the backup channel from a terminal:
`curl -s "https://ntfy.sh/studio-door-f474cd56/json?poll=1"`
