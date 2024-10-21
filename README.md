class Item:
    def __init__(self, item_id, name, category, quantity, price, attributes=None):
        self.item_id = item_id  
        self.name = name
        self.category = category
        self.quantity = quantity
        self.price = price
        self.attributes = attributes or {}  
    
    def update_quantity(self, new_quantity):
        self.quantity = new_quantity
    
    def get_details(self):
        details = {
            "ID": self.item_id,
            "Name": self.name,
            "Category": self.category,
            "Quantity": self.quantity,
            "Price": self.price,
            "Attributes": self.attributes
        }
        return details
        
class Inventory:
    def __init__(self):
        self.items = {}  
    
    def add_item(self, item):
        if item.item_id in self.items:
            print(f"Item with ID {item.item_id} already exists.")
        else:
            self.items[item.item_id] = item
            print(f"Added {item.name} to inventory.")
    
    def remove_item(self, item_id):
        if item_id in self.items:
            removed_item = self.items.pop(item_id)
            print(f"Removed {removed_item.name} from inventory.")
        else:
            print(f"Item with ID {item_id} does not exist.")
    
    def get_item(self, item_id):
        return self.items.get(item_id, None)
    
    def list_items(self):
        for item in self.items.values():
            print(item.get_details())
    
    def update_inventory(self, item_id, new_quantity):
        item = self.get_item(item_id)
        if item:
            item.update_quantity(new_quantity)
            print(f"Updated {item.name}'s quantity to {new_quantity}.")
        else:
            print(f"Item with ID {item_id} not found.")
    
    def check_stock(self, item_id):
        item = self.get_item(item_id)
        if item:
            if item.quantity < 10:
                print(f"Low stock alert: {item.name} (Quantity: {item.quantity})")
            else:
                print(f"Stock level of {item.name} is sufficient (Quantity: {item.quantity}).")
        else:
            print(f"Item with ID {item_id} not found.")

class Report:
    def __init__(self, inventory):
        self.inventory = inventory
    
    def low_stock_report(self, threshold=10):
        print("Low Stock Report:")
        for item in self.inventory.items.values():
            if item.quantity < threshold:
                print(f"{item.name} (ID: {item.item_id}) - Quantity: {item.quantity}")
    
    def generate_report(self, condition=None):
        if condition:
            print(f"Report based on condition: {condition}")
        else:
            print("General Inventory Report:")
            self.inventory.list_items()
            
class User:
    def __init__(self, username, role):
        self.username = username
        self.role = role
    
    def get_permissions(self):
        if self.role == "admin":
            return ["add", "remove", "update", "view"]
        elif self.role == "manager":
            return ["view", "update"]
        elif self.role == "staff":
            return ["view"]
        else:
            return []
    
    def perform_inventory_actions(self, action, inventory, *args):
        permissions = self.get_permissions()
        
        if action in permissions:
            if action == "add" and len(args) == 1:
                inventory.add_item(args[0])
            elif action == "remove" and len(args) == 1:
                inventory.remove_item(args[0])
            elif action == "update" and len(args) == 2:
                inventory.update_inventory(args[0], args[1])
            elif action == "view":
                inventory.list_items()
            else:
                print(f"Invalid arguments for action {action}.")
        else:
            print(f"User {self.username} does not have permission to {action}.")

item1 = Item(1, "Gold Ring", "Ring", 6, 150, {"Size": "6", "Weight": "3g"})
item2 = Item(2, "Gold Necklace", "Necklace", 27, 300, {"Length": "16 inches"})

inventory = Inventory()
inventory.add_item(item1)
inventory.add_item(item2)

inventory.list_items()

report = Report(inventory)
report.low_stock_report(threshold=10)

admin_user = User("admin_user", "admin")
admin_user.perform_inventory_actions("add", inventory, Item(5, "Diamond Earrings", "Earrings", 20, 500))

manager_user = User("manager1", "manager")
manager_user.perform_inventory_actions("update", inventory, 1, 10) 
