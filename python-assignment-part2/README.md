# DATA COPIED EXACTLY SAME AS PROVIDED IN THE QUESTION

menu = {
    "Paneer Tikka":   {"category": "Starters",  "price": 180.0, "available": True},
    "Chicken Wings":  {"category": "Starters",  "price": 220.0, "available": False},
    "Veg Soup":       {"category": "Starters",  "price": 120.0, "available": True},
    "Butter Chicken": {"category": "Mains",     "price": 320.0, "available": True},
    "Dal Tadka":      {"category": "Mains",     "price": 180.0, "available": True},
    "Veg Biryani":    {"category": "Mains",     "price": 250.0, "available": True},
    "Garlic Naan":    {"category": "Mains",     "price":  40.0, "available": True},
    "Gulab Jamun":    {"category": "Desserts",  "price":  90.0, "available": True},
    "Rasgulla":       {"category": "Desserts",  "price":  80.0, "available": True},
    "Ice Cream":      {"category": "Desserts",  "price": 110.0, "available": False},
}

# -------------------------------
# TASK 1: EXPLORE THE MENU
# -------------------------------

# Step 1: find all Menu categories
categories = set()

for item in menu:
    categories.add(menu[item]["category"])

# Step 2: print items grouped by category
for cat in categories:
    print("===== " + cat + " =====")
    
    for item in menu:
        if menu[item]["category"] == cat:
            price = menu[item]["price"]
            available = menu[item]["available"]
            
            if available:
                status = "[Available]"
            else:
                status = "[Unavailable]"
            
            print(f"{item:15} ₹{price:.2f}   {status}")
    
    print()  # empty line

# -----------------------------------
# 2.1 TOTAL NUMBER OF ITEMS IN THE MENU
# -----------------------------------
total_items = len(menu)
print("Total items on menu:", total_items)


# -----------------------------------
# TOTAL NUMBER OF AVAILABLE ITEMS
# -----------------------------------
available_count = 0

for item in menu:
    if menu[item]["available"]:
        available_count += 1

print("Available items:", available_count)


# -----------------------------------
# THE MOST EXPENSIVE ITEM (NAME AND PRICE)
# -----------------------------------
max_price = 0
max_item = ""

for item in menu:
    if menu[item]["price"] > max_price:
        max_price = menu[item]["price"]
        max_item = item

print("Most expensive item:", max_item, "₹", max_price)


# -----------------------------------
# ALL ITEMS PRICED UNDER ₹150 (NAME AND PRICE)
# -----------------------------------
print("Items under ₹150:")

for item in menu:
    if menu[item]["price"] < 150:
        print(item, "₹", menu[item]["price"])

print("======END OF TASK 1========")

# -------------------------------
# TASK 2: EXPLORE THE MENU
# -------------------------------
# STARTING WITH EMPTY CART
cart = []

# Requirement 1 - ADD ITEM FUNCTION
def add_item(name, qty):
    if name not in menu:
        print("Item not in menu!")
        return
    
    if not menu[name]["available"]:
        print("Item is not available!")
        return
    
    # check if the item exits in menu
    for item in cart:
        if item["item"] == name:
            item["quantity"] += qty
            print(f"Updated {name} quantity to {item['quantity']}")
            return
    
    # else add new item
    cart.append({
        "item": name,
        "quantity": qty,
        "price": menu[name]["price"]
    })
    print(f"Added {name} x{qty}")

# Requirement 2 - LOGIC TO REMOVE ITEM
def remove_item(name):
    for item in cart:
        if item["item"] == name:
            cart.remove(item)
            print(f"🗑 Removed {name}")
            return
    print("Item not in cart!")

# Requirement 3 - LOGIC TO UPDATE THE QUANTITY
def update_item(name, qty):
    for item in cart:
        if item["item"] == name:
            item["quantity"] = qty
            print(f"Updated {name} to {qty}")
            return
    print("Item not in cart!")

# PRINT CART
def print_cart():
    print("\n Current Cart:")
    if not cart:
        print("Cart is empty!")
    for item in cart:
        print(item)

# =========================
# Requirement 4 - SIMULATION STEPS
# =========================

add_item("Paneer Tikka", 2)
print_cart()

add_item("Gulab Jamun", 1)
print_cart()

add_item("Paneer Tikka", 1)  # should update the quantity and not new entry 
print_cart()

add_item("Mystery Burger", 1)  # this item does not exist in menu
print_cart()

add_item("Chicken Wings", 1)  # item in menu but unavailable
print_cart()

remove_item("Gulab Jamun")
print_cart()

# =========================
# Requirement - 5 FINAL ORDER SUMMARY
# =========================

print("\n========== Order Summary ==========")

subtotal = 0

for item in cart:
    total_price = item["quantity"] * item["price"]
    subtotal += total_price
    print(f"{item['item']:15} x{item['quantity']}    ₹{total_price:.2f}")

print("------------------------------------")

gst = subtotal * 0.05
total = subtotal + gst

print(f"Subtotal:                ₹{subtotal:.2f}")
print(f"GST (5%):               ₹{gst:.2f}")
print(f"Total Payable:          ₹{total:.2f}")
print("======END OF TASK 2========")

# -----------------------------------------
# TASK 3: INVENTORY TRACKER WITH DEEP COPY
# -----------------------------------------
import copy

# INVENTORY DATA AS PROVIDED IN THE QUESTION DESCRIPTION
inventory = {
    "Paneer Tikka":   {"stock": 10, "reorder_level": 3},
    "Chicken Wings":  {"stock":  8, "reorder_level": 2},
    "Veg Soup":       {"stock": 15, "reorder_level": 5},
    "Butter Chicken": {"stock": 12, "reorder_level": 4},
    "Dal Tadka":      {"stock": 20, "reorder_level": 5},
    "Veg Biryani":    {"stock":  6, "reorder_level": 3},
    "Garlic Naan":    {"stock": 30, "reorder_level": 10},
    "Gulab Jamun":    {"stock":  5, "reorder_level": 2},
    "Rasgulla":       {"stock":  4, "reorder_level": 3},
    "Ice Cream":      {"stock":  7, "reorder_level": 4},
}

# FINAL CART FROM TASK 2
cart = [
    {"item": "Paneer Tikka", "quantity": 3, "price": 180.0}
]

# =============================================================
# STEP 1: DEEP COPY into a variable called inventory_backup
# =============================================================
inventory_backup = copy.deepcopy(inventory)

# Changing one stock value to check if the copy works
inventory["Paneer Tikka"]["stock"] = 99

print("Changed Inventory:", inventory["Paneer Tikka"])
print("Backup Inventory:", inventory_backup["Paneer Tikka"])

#Restoring the original inventory from backup
inventory = copy.deepcopy(inventory_backup)

print("\nRestored Inventory:", inventory["Paneer Tikka"])

# ==================================
# STEP 2: STIMULATE ORDER FULFILMENT
# ==================================

for cart_item in cart:
    name = cart_item["item"]
    qty_needed = cart_item["quantity"]
    
    if name in inventory:
        stock_available = inventory[name]["stock"]
        
        if stock_available >= qty_needed:
            inventory[name]["stock"] -= qty_needed
        else:
            print(f"Only {stock_available} available for {name}, adjusting order!")
            inventory[name]["stock"] = 0

# =====================================================================================================================================
# STEP 3: After deduction, loop through inventory and print a Reorder Alert for every item whose stock is at or below its reorder_level
# ======================================================================================================================================

print("\n Reorder Alerts:")

for item, details in inventory.items():
    if details["stock"] <= details["reorder_level"]:
        print(f"Reorder Alert: {item} — Only {details['stock']} unit(s) left (reorder level: {details['reorder_level']})")

# ===================================================================================
# STEP 4: Print both inventory and inventory_backup at the end to confirm they differ
# ===================================================================================

print("\n Final Inventory (After Orders):")
for item, details in inventory.items():
    print(item, details)

print("\n Backup Inventory (Original Safe Copy):")
for item, details in inventory_backup.items():
    print(item, details)
print("======END OF TASK 3========")

# -------------------------------
# TASK 4: DAILY SALES LOG ANALYSIS
# -------------------------------

# SALES LOG AS PROVIDED IN THE QUESTION DESCRIPTION
sales_log = {
    "2025-01-01": [
        {"order_id": 1,  "items": ["Paneer Tikka", "Garlic Naan"],          "total": 220.0},
        {"order_id": 2,  "items": ["Gulab Jamun", "Veg Soup"],              "total": 210.0},
        {"order_id": 3,  "items": ["Butter Chicken", "Garlic Naan"],        "total": 360.0},
    ],
    "2025-01-02": [
        {"order_id": 4,  "items": ["Dal Tadka", "Garlic Naan"],             "total": 220.0},
        {"order_id": 5,  "items": ["Veg Biryani", "Gulab Jamun"],           "total": 340.0},
    ],
    "2025-01-03": [
        {"order_id": 6,  "items": ["Paneer Tikka", "Rasgulla"],             "total": 260.0},
        {"order_id": 7,  "items": ["Butter Chicken", "Veg Biryani"],        "total": 570.0},
        {"order_id": 8,  "items": ["Garlic Naan", "Gulab Jamun"],           "total": 130.0},
    ],
    "2025-01-04": [
        {"order_id": 9,  "items": ["Dal Tadka", "Garlic Naan", "Rasgulla"], "total": 300.0},
        {"order_id": 10, "items": ["Paneer Tikka", "Gulab Jamun"],          "total": 270.0},
    ],
}

# ==============================
# 1: Print total revenue per day
# ==============================

print("Revenue Per Day:")

daily_revenue = {}

for date, orders in sales_log.items():
    total = 0
    for order in orders:
        total += order["total"]
    
    daily_revenue[date] = total
    print(date, "→ ₹", total)

# ===================================================================
# 2: Print the best-selling day (date with the highest total revenue)
# ===================================================================

best_day = max(daily_revenue, key=daily_revenue.get)
print("\n Best Selling Day:", best_day, "₹", daily_revenue[best_day])

# =============================
# 3: Find the most ordered item
# =============================

item_count = {}

for orders in sales_log.values():
    for order in orders:
        for item in order["items"]:
            item_count[item] = item_count.get(item, 0) + 1

most_ordered = max(item_count, key=item_count.get)
print("\n Most Ordered Item:", most_ordered, "(", item_count[most_ordered], "times )")

# ===========================================================================================
# 4: Add new day to sales_log and then reprint the revenue-per-day table and best-selling day 
# ===========================================================================================

sales_log["2025-01-05"] = [
    {"order_id": 11, "items": ["Butter Chicken", "Gulab Jamun", "Garlic Naan"], "total": 490.0},
    {"order_id": 12, "items": ["Paneer Tikka", "Rasgulla"],                     "total": 260.0},
]

print("\n Updated Revenue Per Day:")

daily_revenue = {}

for date, orders in sales_log.items():
    total = sum(order["total"] for order in orders)
    daily_revenue[date] = total
    print(date, "→ ₹", total)

best_day = max(daily_revenue, key=daily_revenue.get)
print("\n New Best Selling Day:", best_day, "₹", daily_revenue[best_day])

# ========================================================================
# 5. Using enumerate, print a numbered list of all orders across all dates
# ========================================================================

print("\n All Orders:")

count = 1

for date, orders in sales_log.items():
    for order in orders:
        items_str = ", ".join(order["items"])
        print(f"{count}. [{date}] Order #{order['order_id']} — ₹{order['total']:.2f} — Items: {items_str}")
        count += 1

print("======END OF TASK 4========")

