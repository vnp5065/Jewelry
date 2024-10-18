class Item:
    def __init__(self, item_id, name, category, quantity, price, **attributes):
        self.item_id = item_id
        self.name = name
        self.category = category
        self.quantity = quantity
        self.price = price
        self.attributes = attributes  # Can store size, color, etc.

    def update_quantity(self, new_quantity):
        self.quantity = new_quantity

    def get_details(self):
        return {
            'ID': self.item_id,
            'Name': self.name,
            'Category': self.category,
            'Quantity': self.quantity,
            'Price': self.price,
            **self.attributes
        }
class Inventory:
    def __init__(self):
        self.items = {}

    def add_item(self, item):
        self.items[item.item_id] = item

    def remove_item(self, item_id):
        if item_id in self.items:
            del self.items[item_id]

    def get_item(self, item_id):
        return self.items.get(item_id, None)

    def list_items(self):
        return [item.get_details() for item in self.items.values()]

    def update_inventory(self, item_id, quantity):
        item = self.get_item(item_id)
        if item:
            item.update_quantity(quantity)

    def check_stock(self, item_id):
        item = self.get_item(item_id)
        if item:
            return item.quantity
        return None
class Report:
    def __init__(self, inventory):
        self.inventory = inventory

    def low_stock_report(self, threshold=5):
        low_stock_items = [item.get_details() for item in self.inventory.items.values() if item.quantity < threshold]
        return low_stock_items

    def expiry_report(self):
        expiry_items = [item.get_details() for item in self.inventory.items.values() if 'expiry_date' in item.attributes and item.attributes['expiry_date'] <= '2024-12-31']
        return expiry_items

    def sales_report(self):
        # Example: This could be implemented to track sales and generate reports on sold items
        pass

    def generate_report(self, report_type, **kwargs):
        if report_type == 'low_stock':
            return self.low_stock_report(**kwargs)
        elif report_type == 'expiry':
            return self.expiry_report()
        # Add more report types as needed
class User:
    def __init__(self, username, role):
        self.username = username
        self.role = role

    def get_permissions(self):
        if self.role == 'admin':
            return ['add', 'remove', 'update', 'view', 'report']
        elif self.role == 'manager':
            return ['update', 'view', 'report']
        else:
            return ['view']

    def perform_inventory_actions(self, action, inventory, *args):
        permissions = self.get_permissions()
        if action in permissions:
            if action == 'add':
                inventory.add_item(*args)
            elif action == 'remove':
                inventory.remove_item(*args)
            elif action == 'update':
                inventory.update_inventory(*args)
            elif action == 'view':
                return inventory.list_items()
            elif action == 'report':
                return args[0].generate_report(*args[1:])
        else:
            return "Permission Denied"
import json

class Inventory:
    def __init__(self):
        self.items = {}

    def save_inventory(self, filename='inventory.json'):
        with open(filename, 'w') as file:
            data = {item_id: item.get_details() for item_id, item in self.items.items()}
            json.dump(data, file)

    def load_inventory(self, filename='inventory.json'):
        with open(filename, 'r') as file:
            data = json.load(file)
            self.items = {item_id: Item(**item_data) for item_id, item_data in data.items()}


# Create items
item1 = Item(1, "Gold Ring", "Ring", 10, 500, size=7, color='Gold')
item2 = Item(2, "Silver Necklace", "Necklace", 5, 300, length='18 inches', material='Silver')

# Manage inventory
inventory = Inventory()
inventory.add_item(item1)
inventory.add_item(item2)

# Generate reports
report = Report(inventory)
print(report.low_stock_report(threshold=6))

# Save and Load from file
inventory.save_to_file('inventory.json')
inventory.load_from_file('inventory.json')

# User Management
admin = User('admin_user', 'admin')
print(admin.perform_inventory_actions('add_item', inventory, item=item1))
