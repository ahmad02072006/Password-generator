# Password-generator
# Generate random passwords
# Customize length and character types
# Save passwords with labels
# Search saved passwords
# Password strength checker

import random
import string
import json
import os

# Step 1: Generate password
def generate_password(length=12,use_uppercase=True,use_numbers=True,
                      use_special=True):
    """Generate a random password"""
    characters=string.ascii_lowercase
    
    if use_uppercase:
        characters+=string.ascii_uppercase
    if use_numbers:
        characters+=string.digits
    if use_special:
        characters+="!@#$%^&*"

    #Generate random password
    password=''.join(random.choice(characters) for _ in range(length))
    return password

# Step 2: Check password strength
def check_strength(password):
    """Check the strength of a password"""
    score=0
    
    if len(password)>=8:
        score+=1
    if len(password)>=12:
        score+=1
    if any(c.isupper() for c in password):
        score+=1
    if any(c.isdigit() for c in password):
        score+=1
    if any(c in "!@#$%^&*" for c in password):
        score+=1
        
    strenghts=['Very Weak',"Weak","Medium",'Strong','Very Strong']
    return strenghts[min(score,4)]

# Step 3: Save password
def save_password(label, password):
    """Save password to file"""
    passwords = load_passwords()
    
    passwords[label] = password
    
    with open('passwords.json', 'w') as f:
        json.dump(passwords, f, indent=2)
    
    print(f"✅ Password for '{label}' saved!")

# Step 4: Load passwords
def load_passwords():
    """Load saved passwords"""
    if os.path.exists('passwords.json'):
        with open('passwords.json', 'r') as f:
            return json.load(f)
    return {}

# Step 5: View saved passwords
def view_passwords():
    """Display all saved passwords"""
    passwords = load_passwords()
    
    if not passwords:
        print("No saved passwords!")
        return
    
    print("\n" + "="*50)
    print("SAVED PASSWORDS")
    print("="*50)
    
    for label, pwd in passwords.items():
        strength = check_strength(pwd)
        print(f"\n{label}:")
        print(f"  Password: {pwd}")
        print(f"  Strength: {strength}")

# Step 6: Main program
def main():
    """Main program"""
    while True:
        print("\n" + "="*50)
        print("PASSWORD GENERATOR & MANAGER")
        print("="*50)
        print("1. Generate Password")
        print("2. Save Password")
        print("3. View Saved Passwords")
        print("4. Check Password Strength")
        print("5. Exit")
        
        choice = input("\nChoose (1-5): ")
        
        if choice == '1':
            length = int(input("Password length (8-50): "))
            password = generate_password(length)
            strength = check_strength(password)
            print(f"\n🔐 Generated Password: {password}")
            print(f"Strength: {strength}")
            
        elif choice == '2':
            label = input("Enter label (e.g., 'Gmail', 'Facebook'): ")
            password = input("Enter password to save: ")
            save_password(label, password)
            
        elif choice == '3':
            view_passwords()
            
        elif choice == '4':
            password = input("Enter password to check: ")
            strength = check_strength(password)
            print(f"Password Strength: {strength}")
            
        elif choice == '5':
            print("Goodbye! 👋")
            break
if __name__ == "__main__":
    main()


main()
