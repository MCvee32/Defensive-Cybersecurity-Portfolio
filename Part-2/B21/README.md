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

**Activty Tasks:**  
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
