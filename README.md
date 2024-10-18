class Item:
    def __init__(self, item_id, name, category, quantity, price, **specific_attributes):
        self.item_id = item_id
        self.name = name
        self.category = category
        self.quantity = quantity
        self.price = price
        self.specific_attributes = specific_attributes

    def update_quantity(self, new_quantity):
        self.quantity = new_quantity

    def get_details(self):
        details = {
            'ID': self.item_id,
            'Name': self.name,
            'Category': self.category,
            'Quantity': self.quantity,
            'Price': self.price,
            **self.specific_attributes
        }
        return details
        
import json

class Inventory:
    def __init__(self):
        self.items = {}

    def add_item(self, item):
        self.items[item.item_id] = item

    def remove_item(self, item_id):
        if item_id in self.items:
            del self.items[item_id]

    def get_item(self, item_id):
        return self.items.get(item_id)

    def list_items(self):
        return [item.get_details() for item in self.items.values()]

    def update_inventory(self, item_id, new_quantity):
        item = self.get_item(item_id)
        if item:
            item.update_quantity(new_quantity)

    def check_stock(self, item_id):
        item = self.get_item(item_id)
        if item:
            return item.quantity
        return None

    def save_to_file(self, filename):
        with open(filename, 'w') as file:
            json.dump([item.get_details() for item in self.items.values()], file)

    def load_from_file(self, filename):
        with open(filename, 'r') as file:
            items_data = json.load(file)
            for item_data in items_data:
                item = Item(**item_data)
                self.add_item(item)
                
class Report:
    def __init__(self, inventory):
        self.inventory = inventory

    def low_stock_report(self, threshold):
        return [item.get_details() for item in self.inventory.items.values() if item.quantity < threshold]

    def expiry_report(self):
        return [item.get_details() for item in self.inventory.items.values()
                if 'expiry_date' in item.specific_attributes and item.specific_attributes['expiry_date'] <= '2024-12-31']

    def sales_report(self):
        # Placeholder for future sales integration
        return "Sales report not yet implemented."

    def generate_report(self):
        return {
            'Total Items': len(self.inventory.items),
            'Low Stock Items': self.low_stock_report(5),
        }

class User:
    def __init__(self, username, role):
        self.username = username
        self.role = role

    def login(self, username, password):
        # Placeholder for actual login logic
        return username == self.username

    def get_permissions(self):
        permissions = {
            'admin': ['add_item', 'remove_item', 'update_inventory', 'generate_reports'],
            'manager': ['list_items', 'update_inventory', 'generate_reports'],
            'staff': ['list_items', 'check_stock']
        }
        return permissions.get(self.role, [])

    def perform_inventory_actions(self, action, inventory, **kwargs):
        if action in self.get_permissions():
            method = getattr(inventory, action)
            return method(**kwargs)
        else:
            return f"Permission denied for {self.role}"

# Create items
item1 = Item(1, "Gold Ring", "Ring", 10, 500, size=7, color='Gold')
item2 = Item(2, "Silver Necklace", "Necklace", 5, 300, length='18 inches', material='Silver')

# Create inventory and add items
inventory = Inventory()
inventory.add_item(item1)
inventory.add_item(item2)

# Create and generate reports
report = Report(inventory)
print(report.low_stock_report(threshold=6))

# Save inventory to file
inventory.save_to_file('inventory.json')

# Load inventory from file
inventory.load_from_file('inventory.json')

# User management
admin = User('admin_user', 'admin')
admin.perform_inventory_actions('add_item', inventory, item=item1)
