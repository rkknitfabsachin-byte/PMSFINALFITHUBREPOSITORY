# Implementation Plan: Remove Individual Kora Logic & Fix Errors

The core issue is that the system currently divides total Kora (Greige) receipts proportionally among individual color items (e.g., assigning 328.12kg to Arm Taupe and 231.87kg to Windsor Wine). **This makes no sense because Kora is colorless.** 

This plan will completely remove that individual calculation logic and stabilize the system.

## 1. Remove Individual Kora Calculations
*   **Stop Proportional Splitting**: The backend (`Code.gs`) will no longer calculate or distribute a "ratio" of Kora to individual PI Items.
*   **Item Level**: Individual items (colors) will now ONLY track what has been explicitly assigned to dyeing (`dyeing_sent_qty`) and what has returned (`dyeing_received_qty`). Their `greige_produced_qty` will no longer be artificially inflated with proportional Kora.
*   **Group Level**: The total Kora received will only be tracked and validated as a single, combined pool for the entire fabric group.

## 2. FIFO Dyeing Assignment (Total Pool Deduction)
When you send fabric to dyeing:
*   You will see the **Total Available Kora** for the fabric.
*   You simply enter the total weight being sent.
*   The system will automatically deduct this weight from the oldest Kora receipts first (FIFO). 
*   If you send 100kg, but the oldest receipt only has 40kg left, the system will seamlessly split the record in the background to use 40kg from the first receipt and 60kg from the next.

## 3. Technical Fixes
*   **Fix `recalculateItem_`**: Remove the `itemGreigeQty` ratio logic.
*   **Fix `recalculateGreigeLot_`**: Repair this broken function so it properly tallies `sent_weight` and `balance_weight` without causing "Save failed" errors.
*   **Implement `splitDyeingLotFifo_`**: Add the automated background splitting logic so you don't have to manually select specific Kora lots.

## 4. Verification Plan
*   **Test Case**: Order 750kg Arm Taupe and 530kg Windsor Wine. Receive 500kg of Kora.
*   **Expected Result**: The total Kora pool shows 500kg. Neither Arm Taupe nor Windsor Wine will show any "divided" Kora. When you send 200kg of Arm Taupe to dyeing, the Kora pool reduces to 300kg.
