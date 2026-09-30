# BrightChamps · Booking Follow-up Tracker

**Component Prototype: Daily Booking-Assistance Follow-up for Recent Unbooked Leads**  
*Single-page, self-contained HTML application with zero external dependencies.*

---

## 1. Executive Summary & Business Decision

### The Leak
In the historical 5,000-lead dataset covering two months, **1,771 leads had no recorded demo booking**. At the supplied blended marketing cost of **₹900 per lead**, this represents:
$$\frac{1,771}{2} \times ₹900 = ₹7,96,950 \text{ per average acquisition month}$$
This figure represents associated acquisition spending allocated to this drop-off stage. It is **not** proven lost revenue or guaranteed savings.

### The Selected Lever
A daily booking-assistance workflow where a coordinator:
1. Verifies that the lead remains unbooked and that outreach is appropriate.
2. Checks whether the parent still wants a demo.
3. Offers two suitable available demo times matching student grade and timezone.
4. Records the parent's response and next operational action.

*Note on rejected alternatives:* As documented in `fresh_submission/Lever_Decision.md`, standard booking reminders provide less tailored help, earlier-slot offers address post-booking attendance rather than the unbooked gap, and post-demo follow-up addresses purchase objections rather than initial demo scheduling.

---

## 2. One-Minute Walkthrough

You can test the entire workflow in about one minute:

1. **Open the Tracker**:
   Double-click `Booking_Followup_Tracker.html` in any ordinary web browser (Chrome, Safari, Firefox, Edge). No server, setup, or internet connection is required.

2. **Step 1: Review the Active Queue (30 seconds)**
   - Notice the **Summary Metric Cards** at the top showing distinct counts: 5 leads in the Active Queue, 2 in Needs Review, 1 in Cooldown, 1 Booked, 3 Declined/Out of Scope (12 total).
   - Review the table reasons: e.g., unbooked >24 hours, last outreach cooldown passed, or missing CRM records.
   - Click the **Needs Review** tab to see how incomplete records (like `LEAD-106` missing a timestamp and `LEAD-107` with unconfirmed booking status) are held for verification. Their panels do not offer a copyable message or an outreach form.

3. **Step 2: Inspect Slot Offer & Copy Message (15 seconds)**
   - Click the **Active Queue** tab and select **LEAD-101 (Priya Sharma)**.
   - Inspect the right-hand panel: see student metadata, grade, timezone, and two pre-matched slots (`Thu, Oct 1 · 4:00 PM IST` and `Fri, Oct 2 · 6:00 PM IST`).
   - Click **📋 Copy Message** to verify the clipboard copy.
   - Next, click **LEAD-108 (David Chen)** to see the handling when slots are unavailable: the system displays a clear **"No Pre-allocated Slots Available"** notice and a **"Find suitable times"** button that gives coordinator guidance instead of fabricating fake availability.

4. **Step 3: Record an Outcome & Verify Updates (15 seconds)**
   - Return to **LEAD-101**. Under *Record Coordinator Outcome*, select **Demo booked (Confirmed in existing system)**.
   - Add note: *"Parent confirmed Friday 6 PM slot"*.
   - Click **💾 Save Outcome**.
   - Notice immediate updates:
     - Active Queue count drops from **5 to 4**.
     - Demo Booked count increments from **1 to 2**.
     - `LEAD-101` leaves the active queue and moves to **Demo Booked** and **All Leads** with next action *"None (Demo confirmed in existing booking system)"*.
   - Click **📥 Download CSV** to inspect the spreadsheet-safe export.
   - Click **🔄 Reset Demo** to restore all 12 example leads back to initial state.

---

## 3. Proposed Operating Assumptions

The prototype evaluates example leads against four proposed pilot defaults:
1. **Recency Window**: Lead created within the last 7 days ($\le 168$ hours). Older leads are archived as outside pilot scope.
2. **Self-Serve Grace Period**: Lead unbooked for at least 24 hours. Leads under 24 hours are left for natural parent self-booking.
3. **Outreach Cooldown**: No contact within the last 24 hours. Recent contacts remain in cooldown to prevent parent harassment.
4. **No Recorded Decline**: Leads marked "Not interested" or opted out are immediately excluded from active queues.
5. **Conservative Review Gate**: Missing booking status or contact logs are flagged as **Needs Review**, never assumed eligible.

*Important Note:* These rules are proposed pilot operational assumptions, **not** statistical findings derived from the historical dataset.

---

## 4. Demonstration Anchor & Design Boundaries

- **Fixed Demo Date**: All calculations anchor to **Wednesday, Sep 30, 2026, 10:00 AM IST** so that example records remain permanently usable and reproducible regardless of when reviewers inspect the prototype.
- **Local Storage**: Data persists in the browser's `localStorage` (`brightchamps_booking_tracker_v1`). If storage is blocked, the interface remains functional in-memory with CSV export available.
- **Count Integrity**: Metrics count unique leads rather than click events; recording an outcome changes a lead's category without creating another lead.
- **Reply Handling**: During a contact cooldown, the coordinator can record a parent's reply or confirmed booking without starting another outreach attempt.
- **Prototype Boundaries**:
  - The prototype records coordinator actions and confirmed bookings made in BrightChamps' existing scheduling system; it does **not** create calendar events or send automated WhatsApp/SMS messages.
  - Recording outcomes demonstrates workflow usability, **not** actual business impact or causal conversion lift.

---

## 5. Delivery and Verification

- **Offline Independence**: The implementation in `Booking_Followup_Tracker.html` is completely standalone with vanilla HTML5, CSS3, and ES6 JavaScript. It has no external font, CDN, or stylesheet dependencies.
- **Hosting**: `index.html` is the same page and is ready to use as the entry point for a static host. The reviewer can open `Booking_Followup_Tracker.html` directly; the live URL can be added after deployment.
- **Checks**: Run `node verify_tracker.cjs` for the dependency-free logic and safety regression checks. See `TEST_RECORD.md` for what was checked and what still needs a manual browser pass.
