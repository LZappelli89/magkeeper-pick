# MagKeeper Pro 6 — Mobile

A single-page field companion for **MagKeeper Pro 6**. It runs in a phone browser
(no install), reads a data file MK6 writes into your shared folder, and produces
a pick file MK6 reads back. Built to be hosted on GitHub Pages, exactly like the
MK5 mobile app.

## What it does

Two sections, reached from the home screen:

1. **Stock Levels** — current on-hand by product and magazine, straight from the
   MK6 data file. Read-only; MK6 keeps the file current (on open, and on close).
2. **Pick** — scan each box's QR label, then its batch barcode. The QR tells the
   app exactly what it's looking for (a full box, or a part-box piece count) and
   shows it after the first scan. When you're done, review and submit — the app
   writes `MK6_pick_READY.json`, which you drop into your OneDrive folder so MK6
   picks it up next time it opens.

Works with a **Bluetooth scanner** (recommended — it types the code and Enter) or
the phone **camera** (where the browser supports `BarcodeDetector`).

## The round-trip

```
MK6 (desktop)  ──writes──▶  MK6_mobile_data.json  ──OneDrive──▶  phone app (reads)
                                                                      │
phone app  ──writes──▶  MK6_pick_READY.json  ──OneDrive──▶  MK6 (detects on open)
                                                                      │
                              MK6 fills the matching pending order lines,
                              renames the file *_IMPORTED.json, and you open
                              each order and press "Pick" to confirm & transfer.
```

The join key is the QR **label id**: `MK6|<order>|L<line>|B<box>|<sap>|<batch>` —
the same id printed on each physical box label, so every scan lands on the right
line with no pick list to generate first.

## Files

| File | What it is |
|---|---|
| `index.html` | The whole app — open this on the phone (or host on GitHub Pages) |
| `sample_MK6_mobile_data.json` | Example of what MK6 exports — load it to try the app without MK6 |

## Hosting on GitHub Pages

1. Put `index.html` in the repo root.
2. Settings → Pages → deploy from `main` / root.
3. Open the Pages URL on your phone and add it to the home screen.

## Using it

1. Open the app. Tap **Open** and choose the MK6 data JSON in your OneDrive
   folder (the file MK6 wrote). Stock and pending picks load.
2. **Stock Levels** to check on-hand; **Pick** to start.
3. On the Pick screen, keep the input focused and scan: box QR → batch barcode →
   repeat for every box. Delete any mistake and re-scan it. The same box QR can't
   be scanned twice.
4. **Review & submit.** Save the resulting `MK6_pick_READY.json` into your
   OneDrive folder (the Share sheet lets you save straight there; otherwise it
   downloads and you move it).
5. Open MK6 — it detects the pick file, fills the matching lines, and you confirm
   each order with **Pick**.

## Notes / limits

- A phone browser can't silently write into a synced folder, so submitting either
  opens the **Share sheet** (save straight to OneDrive) or **downloads** the file
  for you to move. That's a browser security boundary, not a bug.
- Unknown QR scans are still captured; MK6 flags them on import so you can match
  them manually rather than losing the scan.
- This is a static page — no data leaves the device except the pick file you save.

## Version

v1.0 — first GitHub build. Look & feel matches the MK6 desktop (SAP blue).
