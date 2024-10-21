Report 

This project is designed to create a custom inventory management system for a jewelry store using object-oriented design. The system allows users to manage jewelry inventory through operations such as adding, removing, and updating items, generating reports, and ensuring multi-user support with different access levels. 

The project covers the essential operations of an inventory management system including:
Inventory operations: Adding/removing items, checking stock 
Reporting: Generating reports on stock levels.
User management: Supporting different user roles (e.g., admin, manager).
Data persistence: Storing inventory data for later retrieval.
 	
The system manages various types of jewelry items (e.g., rings, necklaces, earrings), each having specific attributes like size, color, or weight. The Item class supports these variations by including a flexible attributes dictionary to store type-specific details. 

Class Summaries
Item: Stores information about individual jewelry pieces.
Inventory: Manages the collection of items, allowing operations like add, remove, and update.
Report: Provides methods for generating reports on low stock, general item listings, etc.
User: Manages user roles and access permissions, allowing admins to perform inventory actions.

How to Use the System
Add Items: Use the Inventory.add_item() method to add an Item.
Remove Items: Use the Inventory.remove_item(item_id) method.
Update Quantity: Use the Inventory.update_inventory(item_id, new_quantity) method.
Generate Reports: Use the Report.low_stock_report() method to generate stock-related reports.

All major functionalities were tested to ensure the system behaved correctly. Key validations include:
Item addition and removal: Adding items results in them being stored correctly; removing items works as expected.
Report generation: Reports reflect accurate inventory conditions based on item stock levels.
User actions: Role-based access ensures only authorized actions are performed.

Sample Scenarios
Scenario 1: Adding a Jewelry Item:
Admin logs in and adds a new gold ring to the inventory.
Scenario 2: Checking Stock:
A manager checks stock levels and receives a notification that stock for "Gold Ring" is low.
Scenario 3: Generating a Report:
A report is generated to show all items with stock below a specific threshold.

Findings and Challenges
Challenges:
 Managing data persistence efficiently for large inventories.
Implementing role-based permissions without over-complicating the system.
Improvements:
Adding a user-friendly interface.
Enhancing the report generation to include additional criteria.
Adding integration with an external database (e.g., SQLite) for better data persistence.
This project successfully implements a basic jewelry inventory management system with object-oriented design principles. It covers the core operations, but future improvements could include more advanced reporting, better error handling, and user interface development.

Output of the code:
Added Gold Ring to inventory.
Added Gold Necklace to inventory.
{'ID': 1, 'Name': 'Gold Ring', 'Category': 'Ring', 'Quantity': 6, 'Price': 150, 'Attributes': {'Size': '6', 'Weight': '3g'}}
{'ID': 2, 'Name': 'Gold Necklace', 'Category': 'Necklace', 'Quantity': 27, 'Price': 300, 'Attributes': {'Length': '16 inches'}}
Low Stock Report:
Gold Ring (ID: 1) - Quantity: 6
Added Diamond Earrings to inventory.
Updated Gold Ring's quantity to 10.

