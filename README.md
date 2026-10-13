# Moh Maya Cafe — Eco-Luxe Table Pre-Booking Website

A polished, single-page static website for **Moh Maya Cafe, Katra, Prayagraj** (Main Market Road, Near Science Faculty). Customers can pre-book a table, pre-order dishes, and pay the minimum cover charge online via UPI. The cafe owner verifies every online payment by hand before a booking is confirmed — no booking moves forward without the owner's tick.

**Cover charges:** Girls ₹100/person, Boys ₹300/person — 100% redeemable against the food & beverage bill. If pre-ordered dishes cost more than the minimum cover, the customer pays the higher total; any dish amount above the cover is payable as balance at the cafe.

## What's inside

- `index.html` — the entire site (booking engine, menu pre-order, My Bookings, Admin dashboard, printable bill)
- `assets/paytm-qr.png` — the cafe's Paytm UPI QR code shown on the payment screen (UPI ID: `9506605427@ptyes`, account name: Mrityunjay Mishra)
- `vercel.json` — minimal config so Vercel serves the folder as a plain static site

## The payment-verification flow (the core of this site)

1. **Book table** — Customer picks date, time slot, table corner, guest breakdown (girls/boys) and optionally pre-orders dishes. The live cover calculator shows the amount to pay.
2. **Pay via QR** — On submitting with "Pay Now via UPI QR", the customer sees a payment screen with the cafe's Paytm QR, the exact amount, the UPI ID and their booking code. The booking is saved as **⏳ Payment Verification Pending** — it is *not* confirmed.
3. **Customer taps "Payment Kar Diya"** — This only records that the customer *claims* to have paid. The booking still stays Pending. Nothing moves forward.
4. **Owner verifies (the tick)** — The owner opens the **Admin** dashboard (button in the header), sees the pending booking highlighted, checks their own Paytm/UPI that the money actually arrived, and clicks the green **✔ Payment Received — Verify** button. Only then does the booking become **Confirmed**. If the money didn't arrive, the owner clicks **Payment Not Received — Reject** and the booking is marked Rejected.
5. **Confirmed + bill** — Once confirmed, the customer's booking shows an **Estimated Serving Time** (order time + 20 minutes) and a **Print Bill** button. The printable bill carries the customer's name in bold, phone, booking code, order time, cover math (girls × ₹100, boys × ₹300), itemised dishes with prices, amount paid, balance at cafe, and "Prices inclusive of all taxes".

Choosing **Pay Min Cover at Counter** skips the QR step — those bookings are confirmed immediately and paid at the cafe.

Every booking records and displays its **order time** (e.g. "13 Oct 2026, 10:42 PM"), booking date + slot, and — while pending — how long ago the order was placed.

## How the owner verifies payments

1. Open the site, click **Admin** in the top bar.
2. The dashboard shows a **Pending Verification** count. Pending bookings are listed with the customer's name, phone, amount and the time they claimed to have paid.
3. Check your Paytm/UPI app for an incoming payment of that exact amount from that customer.
4. Click **✔ Payment Received — Verify** (you'll get a confirmation prompt) — or **Reject** if no money arrived.
5. The customer's "My Bookings" view updates to Confirmed/Rejected (live via the browser storage event, and on any re-render), and the bill becomes printable.

Admin can also mark confirmed bookings as Arrived / Completed / Cancelled, print bills, delete bookings, and manage menu dish availability.

## Important demo note — localStorage storage

This pitch/demo build stores all bookings in the **browser's localStorage** on a single device. That means the full flow (customer books → owner verifies → bill prints) can be demonstrated end-to-end on one phone/laptop, which is ideal for pitching the concept to the cafe.

For real multi-device use — where a customer's booking should appear on the owner's phone in real time — a small shared live backend (e.g. a database + API, or a service like Firebase/Supabase) needs to be added later. The booking data structure and the verification flow in this build are already designed around that upgrade: swapping the storage layer is the only major change required.

## Running / deploying

- Locally: open `index.html` in a browser, or run `python3 -m http.server` in this folder and visit `http://localhost:8000`.
- On Vercel: import/deploy this folder as a static site — no build step needed.
