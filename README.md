# RiwiSupply S.A.S. - Proyecto de Desempeño: Bases de Datos Relacionales

**Nombre:** luis  
**Clase:** RiwiSupply  
**Fecha:** Julio 2026

---

## README.md

### Project Description
This is the complete delivery for the Relational Databases Performance Test (Módulo 4). The business context is RiwiSupply S.A.S., a company that sells and distributes industrial supplies nationwide.

Task: Analyze the Excel file, normalize the data, design a relational model (up to 3NF), implement in PostgreSQL via pgAdmin, load the data, do CRUD, execute the 6 analytical queries, and document everything.

### Technologies Used
- PostgreSQL 16 (or latest) via pgAdmin 4
- Git for version control

### Database Engine Used
PostgreSQL (via pgAdmin)

### Explanation of Normalization Process
The original Excel had duplicates (supplier names like "Aceros del Norte S.A.S" vs "Aceros del Norte" vs "ACEROS NORTE"), inconsistent product/category names, repeated warehouses/cities, and redundant fields.

We applied:
- **1FN**: Removed repeating groups (no multi-values in a single cell).
- **2FN**: Eliminated partial dependencies (supplier and product are independent).
- **3FN**: Removed transitive dependencies (no redundant city info in multiple tables).

All decisions and justifications are in the sections below. The final model is fully normalized and ready for production.

### Database Structure
The relational model uses these tables (all start with `riwi_` as required):

1 'riwi_city'

2 'riwi_category'

3 'riwi_supplier'

4 'riwi_warehouse'

5 'riwi_product'

6 'riwi_inventory_movement'

### Entity Relationship Model (MER)
The MER was designed in MySQL Workbench and exported as PDF (attached). It uses:
- Entities with attributes and primary keys.
- Relationships (all 1:N).
- Cardinalities clearly defined.
- Foreign keys with ON DELETE CASCADE where logical (e.g., delete a product only if it has no movements).

**MER PDF:** The full diagram is in the submission folder.

### Instructions to Create the Database in pgAdmin
1. Open pgAdmin 4.
2. Connect to your PostgreSQL server.
3. Right-click **Databases** > Create > Database.
4. Name it: `bd_luis_pina_magdalena`
5. Click OK.
6. Right-click the new database > **Query Tool**.
7. Copy and execute the DDL script below.
8. Then run the loading scripts.

### Instructions to Load the Data
- Use the SQL scripts below to load into the normalized tables.
- All data will be consistent after normalization.

### Explanation of Each SQL Query
Each query is explained with the exact business requirement it meets.

### Developer Information
- **Full name:** luis123
- **Clan:** Magdalena

**GitHub URL:** https://github.com/luisP-max/RiwiSupply

---

## Database Design & Implementation

### ERD Design (Textual Representation)

#### Entities & Attributes
- **riwi_city** (City)
  - `city_id` (PK, SERIAL)
  - `city_name` (VARCHAR(100) NOT NULL)

- **riwi_category** (Category)
  - `category_id` (PK, SERIAL)
  - `category_name` (VARCHAR(100) NOT NULL)

- **riwi_supplier** (Supplier)
  - `supplier_id` (PK, SERIAL)
  - `supplier_name` (VARCHAR(150) NOT NULL)
  - `city_id` (FK, INT NOT NULL)

- **riwi_warehouse** (Warehouse)
  - `warehouse_id` (PK, SERIAL)
  - `warehouse_name` (VARCHAR(100) NOT NULL)
  - `city_id` (FK, INT NOT NULL)

- **riwi_product** (Product)
  - `product_id` (PK, SERIAL)
  - `product_name` (VARCHAR(150) NOT NULL)
  - `category_id` (FK, INT NOT NULL)

- **riwi_inventory_movement** (Inventory Movement)
  - `movement_id` (PK, SERIAL)
  - `product_id` (FK, INT NOT NULL)
  - `warehouse_id` (FK, INT NOT NULL)
  - `movement_date` (DATE NOT NULL)
  - `quantity` (DECIMAL(10,2) NOT NULL)
  - `movement_type` (ENUM('IN', 'OUT') NOT NULL)
  - `unit_price` (DECIMAL(10,2) NOT NULL)
  - `purchase_order` (VARCHAR(20))

**Relationships (all 1:N):**
supplier → city_id

warehouse → city_id

product → category_id

inventory_movement → product_id

inventory_movement → warehouse_id

**Constraints:**
- NOT NULL on essential fields.
- UNIQUE on supplier names and product names.
- FOREIGN KEY with CASCADE on delete for movements.
