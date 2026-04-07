# Part 2 - B17: Implement one of the current state-of-the-art solutions and evaluate it

### Solution: IAM system for Access Control
```python
class IAMSystem:
    def __init__(self):
        self.users = {}        # username -> roles
        self.roles = {}        # role -> permissions

    # -----------------------------
    # ROLE MANAGEMENT
    # -----------------------------
    def create_role(self, role_name):
        if role_name not in self.roles:
            self.roles[role_name] = set()

    def add_permission_to_role(self, role_name, permission):
        if role_name in self.roles:
            self.roles[role_name].add(permission)

    # -----------------------------
    # USER MANAGEMENT
    # -----------------------------
    def create_user(self, username):
        if username not in self.users:
            self.users[username] = set()

    def assign_role_to_user(self, username, role_name):
        if username in self.users and role_name in self.roles:
            self.users[username].add(role_name)

    # -----------------------------
    # ACCESS CHECK (CORE IAM LOGIC)
    # -----------------------------
    def check_access(self, username, permission):
        if username not in self.users:
            return False

        user_roles = self.users[username]

        for role in user_roles:
            if permission in self.roles.get(role, []):
                return True

        return False

    # -----------------------------
    # DISPLAY USER PERMISSIONS
    # -----------------------------
    def get_user_permissions(self, username):
        permissions = set()
        if username in self.users:
            for role in self.users[username]:
                permissions.update(self.roles.get(role, []))
        return permissions


# -----------------------------
# DEMO / TEST
# -----------------------------
if __name__ == "__main__":
    iam = IAMSystem()

    # Create roles
    iam.create_role("admin")
    iam.create_role("editor")
    iam.create_role("viewer")

    # Assign permissions
    iam.add_permission_to_role("admin", "read")
    iam.add_permission_to_role("admin", "write")
    iam.add_permission_to_role("admin", "delete")

    iam.add_permission_to_role("editor", "read")
    iam.add_permission_to_role("editor", "write")

    iam.add_permission_to_role("viewer", "read")

    # Create users
    iam.create_user("alice")
    iam.create_user("bob")

    # Assign roles
    iam.assign_role_to_user("alice", "admin")
    iam.assign_role_to_user("bob", "viewer")

    # Test access
    print("Alice permissions:", iam.get_user_permissions("alice"))
    print("Bob permissions:", iam.get_user_permissions("bob"))

    print("Can Alice delete?", iam.check_access("alice", "delete"))
    print("Can Bob write?", iam.check_access("bob", "write"))
```
### Evaluation
This IAM system uses RBAC to assign permissions to users based on their role. It follows the security principles of least privilege and seperation of concerns (roles manage permissions). 
- Strengths:
  - Scalability. Users are assigned roles instead of individual permissions, making adding new users easy.
  - Maintainable. Permissions change once per role
  - Secure baseline. reduces human error vs manual permissions
  - Principle of Least Privilege. Users receive permissions when user identity is created and role is assigned.
  - Central Permission Management. Permissions are managed at role level, not per user. Access for all users are updated when role permsissins are updated.  
    
- Limitations:
  - Role explosion problme. As the system grow, more roles might be needed for different combinations of permissions (in this system it is easy becuase there is limited resources, users, and permissions)
  - Lack of context awareness. e.g. time, location, device, etc
  - Static Permissions. Permissions are fixed within roles, cannot dynamically adapt to changing conditions.
  - No authentication
  - No audit or logging    
