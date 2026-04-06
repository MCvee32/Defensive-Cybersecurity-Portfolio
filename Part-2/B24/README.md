# Part 2 - B24: Design and implement access control of your choice

### Access Control Implementation: Role-Based Access Control
**How it works:**  
Restricts system access to authorised users based on their defined roles within an organization, rather than individual user permissions.  

**Implementation**  
To demonstrate this vulnerability I created the following program.

**1. Define roles and permissions**  
```python
roles = {
    "admin": ["read", "write", "delete"],
    "user": ["read", "write"],
    "guest": ["read"]
}
```
**2. Define system users and their roles**  
```python
users = {
    "toto": "admin",
    "kimi": "user",
    "george": "guest"
}
```
**3. Get users permissions from database**  
```python
def get_permissions(username):
    role = users.get(username)
    if not role:
        return None
    return roles[role]
```
**4. Main: User enters username, grants level of access based on role, or denies access if not in database**  
```python
def main():
    username = input("Welcome! Please enter your username: ").strip().lower()

    permissions = get_permissions(username)
    if permissions:
        print(f"\nHello {username}! Your role is '{users[username]}'.")
        print(f"Permissions: {', '.join(permissions)}")
    else:
        print(f"\nUser '{username}' not found. Access denied.")
```
**Output**  
<img width="794" height="403" alt="Screenshot 2026-04-06 at 8 25 06 pm" src="https://github.com/user-attachments/assets/542785f0-da10-41b2-a353-117236d141c3" />
