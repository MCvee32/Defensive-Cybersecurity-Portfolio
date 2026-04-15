# Part 2 - B21: Design and implement a cybersecurity learning activity.

**Activity Focus:** Access Control  

**Program Overview:**  
Participants/Students will:
1. Enter a username
2. Be assigned a role (admin, manager, employee, guest)
3. See what permissions they have
4. Try to access restricted resources
5. Observe what happens when access is denied

```python
# RBAC Cybersecurity Learning Activity

# Define roles and permissions
roles = {
    "admin": ["read", "write", "delete", "modify_users"],
    "manager": ["read", "write"],
    "employee": ["read"],
    "guest": []
}

# Users
users = {
    "alice": "admin",
    "bob": "manager",
    "charlie": "employee",
    "david": "guest"
}

def login():
    username = input("Enter your username: ").lower()
    
    if username in users:
        role = users[username]
        print(f"\nWelcome {username}! Your role is: {role}")
        return role
    else:
        print("User not found. Assigned role: guest")
        return "guest"

def show_permissions(role):
    print("\nYour permissions:")
    for perm in roles[role]:
        print(f"- {perm}")

def access_resource(role):
    print("\nTry to access a resource:")
    action = input("Enter action (read/write/delete/modify_users): ").lower()
    
    if action in roles[role]:
        print("Access granted!")
    else:
        print("Access denied!")

def main():
    print("RBAC Simulation Activity")
    
    role = login()
    show_permissions(role)
    
    while True:
        access_resource(role)
        again = input("\nTry again? (yes/no): ").lower()
        if again != "yes":
            break

    print("\nEnd. Complete Reflection Questions.")

if __name__ == "__main__":
    main()
```

### Activity

**Learning Objectives:**  
- Understand what access control is
- Learn the concept of roles and permissions
- See how Role-Based Access Control is implemented in real systems
- Identify risks of misconfigured permissions  

**Overview:**  
This activity demonstrates how Role-Based Access Control (RBAC) works through an interactive Python simulation. Participants will log in as different users, each assigned a specific role, and explore the permissions associated with that role.

By attempting to perform various actions (such as reading, writing, or deleting data), participants will observe how access is either granted or denied based on their role. This helps illustrate key cybersecurity principles such as authorization, least privilege, and the importance of properly configured access controls in preventing unauthorized actions.

**Activity Tasks:**  
Complete each task and answer questions  
**Task 1:** Login  
- Try logging in as different users:
  - alice
  - bob
  - charlie
  - random user (choose a username and attempt login)  

**Question:**
What happens when a user not in the system attempts to access the system?  

**Task 2:** Explore Permissions
 - For each user outline what permissions they have:
   - alice:
   - bob:
   - charlie:  

**Question:**
Which role has the most and least access?

**Task 3:** Test Security
- As guest, attempt to delete
- As employee, attempt to write
- As manager, attempt to modify_users  

**Question:**
What happens for each?

**Reflection**  
- How does RBAC reduce security risks?
- How could permission assignment misconfiguration lead to a security breach in RBAC systems? (e.g. a guest is incorrectly given "delete" permissions)
- What improvements could be made to improve this system?
