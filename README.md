# SAP ABAP Classical Report - VBAK Sales Order Display

This ABAP report (`ZREPORT_SINGLE_TABLE`) fetches and displays Sales Order Header data from the `VBAK` table in a classical report format.

---

##  Description

The report reads records from the `VBAK` table based on selection screen inputs and displays the following fields:

- **Sales Document (VBELN)**
- **Created On (ERDAT)**
- **Created By (ERNAM)**
- **Order Date (AUDAT)**
- **Sales Document Type (AUART)**
- **Currency (WAERK)**

---

##  Selection Screen Features

- Select Options for `VBELN` (Sales Document Number)
- Select Options for `WAERK` (Currency) with conditional enable/disable logic via a checkbox
- `p_chk` checkbox dynamically enables/disables the WAERK field group


---

##  Events Used

- `AT SELECTION-SCREEN OUTPUT` – For dynamic screen modification
- `INITIALIZATION` – Default values set for report ID, user, and date
- `START-OF-SELECTION` – Main data fetching logic
- `TOP-OF-PAGE` – Custom header on report
- `END-OF-SELECTION` – Footer text
- `END-OF-PAGE` – Page-wise footer (optional)

---

##  Sample Output

| VBELN      | ERDAT      | WAERK | ERNAM    | AUDAT      | AUART |
| ---------- | ---------- | ----- | -------- | ---------- | ----- |
| 4000001234 | 2025-05-15 | INR   | SAPUSER1 | 2025-05-10 | OR    |
| ...        |            |       |          |            |       |


-----------------------------------

##  Technical Details

- Table Used: `VBAK`
- Custom Types: `TY_VBAK`
- Internal Table: `IT_VBAK`
- Work Area: `WA_VBAK`

---

##  Notes

- Ensure authorization for accessing `VBAK` table in production.
- Modify `SELECT` logic to add more fields or apply row limitations (`UP TO 20 ROWS`).
- Modularization possible using includes: `TOP`, `SCR`, `F01`

---

##  File Info

- `ZREPORT_SINGLE_TABLE.abap` – Main ABAP classical report source code

---

##  Author

**[VIKAS KUMAR ]** – SAP ABAP Technical Consultant  





