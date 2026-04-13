# Part 2 - B19: Find and fix a vulnerability from a GitHub project.

### GitHub Project Chosen: github.com/sohailmahmud/labM
Project is a Laboratory Management System. It contains an admin side where a user can manage all the inventory records (manage transactions, add faculty, room, and inventory, view records, and user management). In this project, the user has to perform all the main functions from the admin side.

### Vulnerability Found: SQL Injection
Unsanitised POST params injected directly into SQL query  
**File Affected:** class/display/display.php  
**Vulnerable Code:**  
```
// display.php line: 579 $sql = $conn->prepare( "... WHERE MONTH(borrow.date_borrow) = " . $_POST['month'] . // ← unsanitised user input " AND YEAR(borrow.date_borrow) = " . $_POST['year'] // ← unsanitised user input );
```
**Attack Payload:** month=1 OR 1=1-- -  
**Effect:** bypasses the WHERE filter, dumps all borrow records regardless of date  
### Fixed Code: Parameterised prepared statement
- Bind month/year as types parameters
```
$month = filter_input(INPUT_POST, 'month', FILTER_VALIDATE_INT);
$year = filter_input(INPUT_POST, 'year', FILTER_VALIDATE_INT);
$sql = $conn->prepare( "... WHERE MONTH(borrow.date_borrow) = ? AND YEAR(borrow.date_borrow) = ? GROUP BY borrow.borrowcode" );
$sql->execute([$month, $year]);
```
