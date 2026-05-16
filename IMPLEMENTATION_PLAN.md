# Comprehensive PMS Upgrade Plan

This plan outlines the next major evolution of your Production Management System (PMS). We are shifting focus from basic tracking to deep visibility, automated Google Sheets reporting, active data archiving, and a premium User Experience.

## User Review Required
> [!IMPORTANT]
> **Archiving Strategy**: I propose that when you click "Archive" on an order, its data is moved to `Archive_PIs`, `Archive_PI_Items`, etc., within the *same* spreadsheet, and deleted from the active tabs. Is this acceptable, or do you want it moved to a completely different Google Sheet URL?
> 
> **Final Dashboard Update Frequency**: The `Final Dashboard` tab will be rebuilt automatically. To avoid slowing down data entry, I propose updating it *once a day* via a Time-Driven Trigger, OR adding a manual "Sync Dashboard" button to the Sheets menu. Which do you prefer?

## Proposed Changes

### Phase 1: UX & UI Overhaul (Best-in-Class Experience)
We will completely revamp `styles.css` and `app.js` to create a dynamic, premium web app.
*   **Aesthetics**: Implement a modern, vibrant design system with glassmorphism effects, smooth gradients, and subtle micro-animations (hover states, modal transitions).
*   **Typography & Colors**: Introduce a professional Google Font (e.g., Inter or Roboto) and a curated HSL color palette that feels responsive and alive.
*   **Remove New PI**: Delete the "New PI" button and modal from `index.html` and `app.js` as requested, since you rely on Google Sheets directly for order entry.

---

### Phase 2: The Detailed Dashboard
The current simple metrics will be replaced by a comprehensive command center in `app.js`.
*   **Live Dyeing Status**: A new section showing exactly how many lots are currently at which Dyeing House, including total weights and qualities (fabric types).
*   **Live Kora Availability**: A clear breakdown showing how much Kora is available, grouped by Fabric and Order (PI), so you never have to guess where your Kora is sitting.
*   **Dynamic Deductions**: All numbers on this dashboard will update instantly as soon as you assign Kora to Dyeing.

---

### Phase 3: Google Sheets Architecture (Backend)
We will add powerful new macros and functions to `Code.gs`.

#### 1. The "Final Dashboard" Tab
*   I will create a Google Apps Script function that flattens your entire production data into a single, highly readable tab called `Final_Dashboard`.
*   **Columns**: PI No, Customer, Fabric, Colour, Ordered Qty, Total Kora Received, Total Sent to Dyeing, Total Received from Dyeing, Final Balance, and Status.
*   This gives you an instant, bird's-eye view directly in Google Sheets without opening the PWA.

#### 2. The Archive System
*   **New Tabs**: `Archive_PIs`, `Archive_PI_Items`, `Archive_Greige_Lots`, `Archive_Dyeing_Lots`.
*   **Logic**: When an order is fully completed, you can click "Archive". The backend will safely move all related rows to the Archive tabs and delete them from the Active tabs.
*   **Benefit**: Your main PWA and active sheets stay lightning fast and completely decluttered.

#### 3. Bug Fixes
*   **Dyeing House Dropdown**: Fix the bug by adding default Seed Data for Dyeing Houses in `Code.gs` so the dropdown populates correctly upon initialization.

## Verification Plan
1. **Automated Tests**: Run syntax checks on `Code.gs` and `app.js`.
2. **UI Verification**: Open the PWA, verify the "New PI" button is gone, and the new premium Dashboard renders with Dyeing/Kora groupings. Check that the Dyeing House dropdown is populated.
3. **Backend Verification**: Manually trigger the "Archive" function on a test PI and verify it moves successfully between Google Sheets tabs. Trigger the "Final Dashboard" sync and verify the flattened output.
