import json
import os

# File to store contacts
CONTACT_FILE = "contacts.json"

# Load existing contacts
def load_contacts():
    if os.path.exists(CONTACT_FILE):
        with open(CONTACT_FILE, "r") as f:
            return json.load(f)
    return []

# Save contacts
def save_contacts(contacts):
    with open(CONTACT_FILE, "w") as f:
        json.dump(contacts, f, indent=4)

# Add new contact
def add_contact(contacts):
    print("\n=== Add New Contact ===")
    name = input("Name: ").strip()
    phone = input("Phone: ").strip()
    email = input("Email: ").strip()
    address = input("Address: ").strip()

    if name and phone:
        contacts.append({"name": name, "phone": phone, "email": email, "address": address})
        save_contacts(contacts)
        print("✅ Contact added successfully!\n")
    else:
        print("⚠️ Name and Phone are required!\n")

# View all contacts
def view_contacts(contacts):
    print("\n=== Contact List ===")
    if not contacts:
        print("No contacts available.\n")
    else:
        for i, contact in enumerate(contacts, start=1):
            print(f"{i}. {contact['name']} - {contact['phone']}")
    print()

# Search contacts
def search_contact(contacts):
    keyword = input("Enter name or phone to search: ").strip().lower()
    print("\n=== Search Results ===")
    found = False
    for contact in contacts:
        if keyword in contact["name"].lower() or keyword in contact["phone"]:
            print(f"Name: {contact['name']}")
            print(f"Phone: {contact['phone']}")
            print(f"Email: {contact['email']}")
            print(f"Address: {contact['address']}\n")
            found = True
    if not found:
        print("No contact found!\n")

# Update contact
def update_contact(contacts):
    view_contacts(contacts)
    if contacts:
        try:
            num = int(input("Enter contact number to update: "))
            if 1 <= num <= len(contacts):
                contact = contacts[num-1]
                print("\nLeave blank if you don't want to change.")

                new_name = input(f"Name ({contact['name']}): ").strip()
                new_phone = input(f"Phone ({contact['phone']}): ").strip()
                new_email = input(f"Email ({contact['email']}): ").strip()
                new_address = input(f"Address ({contact['address']}): ").strip()

                if new_name: contact['name'] = new_name
                if new_phone: contact['phone'] = new_phone
                if new_email: contact['email'] = new_email
                if new_address: contact['address'] = new_address

                save_contacts(contacts)
                print("✅ Contact updated successfully!\n")
            else:
                print("⚠️ Invalid contact number!\n")
        except ValueError:
            print("⚠️ Please enter a valid number!\n")

# Delete contact
def delete_contact(contacts):
    view_contacts(contacts)
    if contacts:
        try:
            num = int(input("Enter contact number to delete: "))
            if 1 <= num <= len(contacts):
                deleted = contacts.pop(num-1)
                save_contacts(contacts)
                print(f"🗑️ Deleted contact: {deleted['name']}\n")
            else:
                print("⚠️ Invalid contact number!\n")
        except ValueError:
            print("⚠️ Please enter a valid number!\n")

# Main menu
def main():
    contacts = load_contacts()

    while True:
        print("=== 📒 CONTACT MANAGEMENT SYSTEM ===")
        print("1. Add Contact")
        print("2. View Contacts")
        print("3. Search Contact")
        print("4. Update Contact")
        print("5. Delete Contact")
        print("6. Exit")

        choice = input("Enter choice (1-6): ")
        print()

        if choice == "1":
            add_contact(contacts)
        elif choice == "2":
            view_contacts(contacts)
        elif choice == "3":
            search_contact(contacts)
        elif choice == "4":
            update_contact(contacts)
        elif choice == "5":
            delete_contact(contacts)
        elif choice == "6":
            print("👋 Exiting... Contacts saved!")
            break
        else:
            print("⚠️ Invalid choice! Please enter 1–6.\n")

if __name__ == "__main__":
    main()
