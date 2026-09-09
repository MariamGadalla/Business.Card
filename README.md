# RFID / NFC Business Card

## Terminology first
RFID is the whole family of radio tags. NFC is the 13.56 MHz slice of it that every
modern phone can read with no app and no pairing. A card that works when a stranger
taps their phone on it must be **NFC**. So this project is an NFC card.

## What actually gets built

Three separate things:

1. **The physical card** — a blank PVC (or metal) card with an NTAG chip inside.
   Bought, not built. NTAG215 (504 bytes) is the safe default.
2. **The digital card page** — a small web page: photo, name, title, tap-to-call,
   tap-to-email, and a "Save to Contacts" button that hands over a .vcf file.
   Hosted free on GitHub Pages.
3. **The write** — putting one NDEF record (the page's URL) onto the chip using a
   phone. Takes about five seconds per card.

## Why a URL and not the contact details directly

The chip can hold a vCard directly. It works offline and needs no hosting. But the
details are then frozen in plastic: change job, change number, and every card you
ever handed out is wrong, and the ones in other people's drawers can never be fixed.

Writing a URL instead means the chip holds a pointer. Edit the page, every card
already in the wild updates itself. It also allows a visit counter, and iOS pops up
the link banner more reliably for URLs than for raw vCards.

Cost of the URL approach: the card needs a network connection at tap time, and the
page must stay hosted. Both acceptable.

## Steps

1. Fill in `card.yaml`.
2. Generate `index.html` (the page) and `contact.vcf` (the download) from it.
3. Push to a GitHub Pages repo, get the live URL, put it back in `card.yaml`.
4. Buy blank NTAG215 cards.
5. Install "NFC Tools" (free, iOS and Android). Write → Add a record → URL → paste →
   Write → hold the card to the top of the phone.
6. Optional: in NFC Tools, "Lock tag" makes it read-only forever. Do this only after
   testing, it cannot be undone.

## Hardware notes

- An iPhone 7 or newer can both read and write NFC tags. A Mac cannot, without a
  USB reader such as an ACR122U. The phone is the easier path.
- Metal cards need "NFC-compatible / on-metal" tags. Plain NTAG stickers fail on metal.
- Printing on blank PVC cards needs a card printer. A print shop is cheaper for
  small runs, or buy pre-printed blanks and keep the design minimal.

## Files

- `card.yaml`   — the single source of truth for all details
- `index.html`  — generated, the hosted page
- `contact.vcf` — generated, the download target
