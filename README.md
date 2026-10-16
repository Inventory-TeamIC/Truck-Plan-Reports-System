# Truck Plan Reports System

A single-page HTML tool that imports **Truck Nº** from the *Transport Plan* report into the *Draft Transfer Order Listing Report* (sheet **Agent Order**), using matching and allocation (split) rules. Everything runs in the browser: files are read locally and never uploaded.

## How to use

1. Open `Truck Plan Reports System.html` in a browser (internet is needed once to load the SheetJS library).
2. Select the **Draft Transfer Order Listing Report** (.xlsx).
3. Select the **Transport Plan** (.xlsx).
4. *(Optional)* Tick the fallback box to also match by To Code + Item Nº only.
5. Click **Process Files**, check the **Summary Result** and preview, then click **Download Excel**.

## Rules

1. Only rows with Status **Release** are processed. Other rows are kept unchanged.
2. **Matching** by To Code, TQ No, TSO Nº, Item Nº and Order qty. An exact qty match imports the Truck Nº directly, with no split.
3. **Allocation**: when the qty does not match (the agent has a different or larger qty than the transport plan), the row is split by Truck Nº. All split rows add up to the Order qty.
4. Any qty without a truck is kept as a row with a blank Truck Nº ("keep for next"). Truck qty not used by any row is listed in **Truck Qty Left**.

## Output: `Draft Transfer Order Result.xlsx`

| Sheet | Content |
|---|---|
| Agent Order | Full report with Truck Nº filled in, split rows and an *Allocation Note* column |
| Summary | Per To Code: released rows, Order Qty, Allocated Qty, Not Allocated Qty, trucks used, % allocated, plus a TOTAL row |
| Truck Qty Left | Transport plan quantities not used by any row (only if any) |

## Required columns

| Report (Agent Order) | Transport Plan |
|---|---|
| Status, To Code, TQ No, TSO Nº, Item Nº, Order, Remaining | To Code, TQ No, TSO Nº, Item Nº, Order, Truck Nº |

Headers are matched flexibly (for example `TSO Nº` and `TSO No` both work). If a required column is missing, the log says which one.

## Notes

- Splitting is based on **Order**. **Remaining** is divided across the split rows.
- To publish the tool as a web page, rename the file to `index.html` and enable GitHub Pages (Settings → Pages → Deploy from branch → `main`).
