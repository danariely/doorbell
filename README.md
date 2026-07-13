# 🔔 QR Video Doorbell

A QR code on the door replaces the doorbell. A visitor scans it with any phone,
taps one button, and two things happen:

1. Dan's phone gets an **urgent push notification** ("Someone is at the door").
2. The visitor lands in a **browser video room** — no app, no account, nothing installed.

Dan taps the notification and joins the same room. Two-way video, answerable from
anywhere in the world. The visitor never sees Dan's phone number.

**Live page:** https://danariely.github.io/doorbell/

## One-time setup on Dan's phone (required, ~2 minutes)

1. Install the **ntfy** app (App Store / Google Play — free, no account).
2. In ntfy, tap **+ Subscribe to topic** and enter exactly: `studio-door-f474cd56`
3. Allow notifications when asked. In the topic's settings, set notification
   priority/sound to maximum so it behaves like a real doorbell.
4. Optional but recommended: open https://meet.ffmuc.net once in Safari and allow
   camera + microphone, so answering later is one tap instead of three.

## How answering works

- Notification arrives → tap it → the video room opens in the browser →
  allow camera → you're face-to-face with whoever is at the door.
- Ignore the notification and nothing happens (the visitor waits alone in the
  room and gives up — like an unanswered doorbell).

## Pieces

| Piece | What it is |
|---|---|
| `index.html` | The doorbell page (GitHub Pages, repo `danariely/doorbell`) |
| `print-card.html` | Printable door card with the QR — open and print |
| `qr-doorbell.png` | The bare QR code (points at the live page) |
| ntfy topic `studio-door-f474cd56` | Free push channel via ntfy.sh |
| Jitsi room `StudioDoor-f474cd56` | Anonymous video room on meet.ffmuc.net |

## Design decisions & caveats

- **meet.ffmuc.net, not meet.jit.si** — Jitsi's official server has required a
  logged-in moderator since Aug 2023 (visitors would hang at "Waiting for a
  moderator"). Freifunk München's public server still allows fully anonymous
  rooms. If it ever changes policy, edit `ROOM_URL` at the top of `index.html`
  (any public Jitsi instance works, e.g. framatalk.org) and push.
- **Anyone who scans can ring** — that is the point of a doorbell, but it also
  means the page URL itself is the "button". The room and topic names contain a
  random slug so strangers can't guess them without visiting the door.
- **No number exposure** — nothing on the page or in the video room reveals a
  phone number or email.
- **Deploy** = edit files here, then `git push` (repo `danariely/doorbell`,
  GitHub Pages serves from `main`).

## Testing the bell

Open the live page, tap the button, and Dan's phone should buzz within ~2
seconds. To watch the channel from a terminal instead:
`curl -s "https://ntfy.sh/studio-door-f474cd56/json?poll=1"`
