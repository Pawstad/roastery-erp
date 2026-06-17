# Blueprint: Roastery & Coffee Shop SME ERP System

## 1. Project Objective
To build an *Enterprise Resource Planning* (ERP) application prototype specifically designed for small-to-medium enterprise (SME) roasteries and coffee shops. This system aims to digitize and automate cash flow tracking, goods movement, and inventory management at the managerial level without burdening users with complex data entry bureaucracy.

## 2. Functional Scope
This application focuses on three core business-driving modules:
* **Order to Cash (O2C):** Manages the sales cycle, from incoming orders and fulfillment to receiving payments.
* **Procure to Pay (P2P):** Controls the raw material purchasing cycle, from ordering from vendors and receiving physical goods, to executing accounts payable.
* **Item Management:** Controls inventory, including item tracking, inter-location transfers, and stock adjustments due to shrinkage (*yield loss*) post-roasting.

## 3. Role Structure (Application Users)
The system uses a functional access approach simplified into 4 main roles to accommodate the SME structure:

1.  **Admin (Owner)**
    * *Superuser* with full access to all modules, features, and screens.
    * Can view the overarching business dashboard and intervene in any transaction.
2.  **Sales**
    * Combines the functions of a *Sales Representative* and *Sales Manager*.
    * Responsible for receiving customer orders and creating *Sales Orders*.
3.  **Inventory**
    * Combines the functions of an *Inventory Manager* and *Purchasing Manager*.
    * Responsible for the physical flow of goods, stock adjustments, and creating vendor order drafts (*Purchase Orders*).
4.  **Monetary**
    * Combines the functions of an *A/R Analyst* and *A/P Analyst*.
    * Fully responsible for cash flow, issuing *Invoices* to customers, and executing *Pay Bills* to vendors.

## 4. Standard Operating Procedures (SOP)

### A. Order to Cash (O2C)
1.  **Create Order:** The **Sales** role receives an order and creates a *Sales Order* in the system. The order is automatically approved.
2.  **Fulfillment:** The **Inventory** role processes the order, prepares the goods (*Pick & Pack*), and dispatches them (*Ship*).
3.  **Invoicing:** Once the goods are shipped, the **Monetary** role issues an *Invoice* and sends it to the customer.
4.  **Payment:** The **Monetary** role records the incoming funds (*Accept Payment*) once the customer clears the bill.

### B. Procure to Pay (P2P)
*Note: The system implements an automatic Three-Way Matching principle to prevent cash flow leakage.*
1.  **Create PO:** The **Inventory** role creates a *Purchase Order* (PO) to restock raw materials from a vendor.
    * *Background Process:* The system automatically generates a *Bill* from that PO, but its status remains *Locked*. **Monetary** cannot process the payment yet.
2.  **Receive Item:** Physical goods arrive at the roastery. The **Inventory** role verifies the goods and executes an *Item Receipt*. Warehouse stock increases.
    * *Trigger:* This receipt action unlocks the *Bill*. The *Bill* status changes to *Open / Ready to Pay*.
3.  **Pay Bill:** The **Monetary** role reviews the list of *Open Bills* and executes payments to the vendors.

### C. Item Management
Fully managed by the **Inventory** role with the following operational capabilities:
* **Item Creation:** Master data creation based on item types:
    * *Inventory Item* (e.g., Green Beans, Roasted Beans).
    * *Non-Inventory Item* (e.g., Cleaning supplies, packaging).
    * *Service Item* (e.g., Toll roasting services).
* **Adjustment:** Manual stock adjustments. Highly crucial for recording the weight shrinkage (*yield loss*) of raw coffee beans after the roasting process.
* **Transfer:** Moving stock between locations (e.g., from the Storage Warehouse to the Bar/Roasting Area).

## 5. Tech Stack & Architecture
This project prioritizes robust and scalable *rapid prototyping*, utilizing a *polyrepo architecture* to separate the frontend and backend.

**Tooling & Environment**
* **Package Manager & Runtime:** Bun (For blazing-fast installation and script execution).
* **Code Quality:** ESLint + Prettier (To maintain code consistency across both repositories).

**Backend Repository**
* **Framework:** Express.js (Lightweight and straight to the core of routing).
* **Database:** PostgreSQL.
* **ORM:** Prisma (Accelerates writing data relational schemas such as *Purchase Orders*, *Bills*, and *Items*).

**Frontend Repository**
* **Build Tool & Framework:** React + Vite (Provides a lightning-fast development server and optimized build process).
* **Styling:** Tailwind CSS (Utility-first framework to rapidly build custom and responsive UI directly within the React components).