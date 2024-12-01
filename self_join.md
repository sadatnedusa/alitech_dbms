### **What is a SELF JOIN?**

A **SELF JOIN** is a join operation where a table is joined with itself. It is particularly useful when dealing with hierarchical or relationship-based data within the same table. 

In a SELF JOIN, you create two aliases of the same table and join them based on a condition, typically comparing different rows of the same table.


### **Why Use SELF JOIN?**
1. To find relationships within the same table (e.g., manager-employee, parent-child).
2. To compare rows within the same table.
3. To identify patterns or find duplicate values.


### **Syntax**
```sql
SELECT A.column1, B.column2
FROM TableName A
JOIN TableName B
ON A.common_field = B.common_field;
```

Here:
- `A` and `B` are aliases for the same table.
- `common_field` is the column used to compare rows within the table.

### **Examples**

#### **1. Employee-Manager Relationship**
Consider an `Employees` table:

| **EmployeeID** | **EmployeeName** | **ManagerID** |
|----------------|------------------|---------------|
| 1              | John             | NULL          |
| 2              | Sarah            | 1             |
| 3              | Mike             | 1             |
| 4              | Lisa             | 2             |

To find the names of employees and their managers:
```sql
SELECT 
    E.EmployeeName AS Employee,
    M.EmployeeName AS Manager
FROM Employees E
LEFT JOIN Employees M
ON E.ManagerID = M.EmployeeID;
```

**Output**:
| **Employee** | **Manager** |
|--------------|-------------|
| John         | NULL        |
| Sarah        | John        |
| Mike         | John        |
| Lisa         | Sarah       |

---

#### **2. Finding Duplicate Rows**
Consider a `Products` table:

| **ProductID** | **ProductName** | **Price** |
|---------------|-----------------|-----------|
| 1             | Laptop          | 1000      |
| 2             | Mouse           | 20        |
| 3             | Laptop          | 1000      |
| 4             | Keyboard        | 50        |

To find duplicate products:
```sql
SELECT 
    P1.ProductName,
    P1.Price,
    P2.ProductID AS DuplicateID
FROM Products P1
JOIN Products P2
ON P1.ProductName = P2.ProductName
   AND P1.Price = P2.Price
   AND P1.ProductID < P2.ProductID;
```

**Output**:
| **ProductName** | **Price** | **DuplicateID** |
|-----------------|-----------|-----------------|
| Laptop          | 1000      | 3               |

---

#### **3. Comparing Salaries**
Consider an `Employees` table with salaries:

| **EmployeeID** | **EmployeeName** | **Salary** |
|----------------|------------------|------------|
| 1              | John             | 50000      |
| 2              | Sarah            | 60000      |
| 3              | Mike             | 70000      |

To find employees earning less than others:
```sql
SELECT 
    E1.EmployeeName AS Employee,
    E2.EmployeeName AS HigherEarner
FROM Employees E1
JOIN Employees E2
ON E1.Salary < E2.Salary;
```

**Output**:
| **Employee** | **HigherEarner** |
|--------------|------------------|
| John         | Sarah            |
| John         | Mike             |
| Sarah        | Mike             |

---

### **Key Notes**
1. Always use aliases (`A`, `B`, etc.) to avoid confusion.
2. Ensure proper conditions to avoid infinite loops or unintended results.
3. SELF JOIN is especially helpful in hierarchical or duplicate data analysis.

---

### **Practical Use Cases for SELF JOIN**

A **SELF JOIN** is useful when you need to find relationships or patterns within a single table. Below are some real-world scenarios where SELF JOIN can be applied, along with examples:

---

### **1. Employee-Manager Hierarchy**
**Scenario**: In an organization, each employee has a manager. The `Employees` table stores both employee and manager information. You need to list employees along with their managers.

**Table Structure**:
| **EmployeeID** | **EmployeeName** | **ManagerID** |
|----------------|------------------|---------------|
| 1              | John             | NULL          |
| 2              | Sarah            | 1             |
| 3              | Mike             | 1             |
| 4              | Lisa             | 2             |

**Query**:
```sql
SELECT 
    E.EmployeeName AS Employee,
    M.EmployeeName AS Manager
FROM Employees E
LEFT JOIN Employees M
ON E.ManagerID = M.EmployeeID;
```

**When to Use**: 
- To display hierarchical relationships, e.g., employees and their managers.
- To analyze organizational reporting structures.

---

### **2. Finding Duplicate Records**
**Scenario**: You need to identify duplicate entries in a `Products` table based on the same product name and price.

**Table Structure**:
| **ProductID** | **ProductName** | **Price** |
|---------------|-----------------|-----------|
| 1             | Laptop          | 1000      |
| 2             | Mouse           | 20        |
| 3             | Laptop          | 1000      |
| 4             | Keyboard        | 50        |

**Query**:
```sql
SELECT 
    P1.ProductID AS Original,
    P2.ProductID AS Duplicate
FROM Products P1
JOIN Products P2
ON P1.ProductName = P2.ProductName 
   AND P1.Price = P2.Price 
   AND P1.ProductID < P2.ProductID;
```

**When to Use**:
- To clean up duplicate records in the database.
- To validate data integrity.

---

### **3. Comparing Rows**
**Scenario**: A company wants to compare salaries between employees to identify those earning less than others.

**Table Structure**:
| **EmployeeID** | **EmployeeName** | **Salary** |
|----------------|------------------|------------|
| 1              | John             | 50000      |
| 2              | Sarah            | 60000      |
| 3              | Mike             | 70000      |

**Query**:
```sql
SELECT 
    E1.EmployeeName AS Employee,
    E2.EmployeeName AS HigherEarner
FROM Employees E1
JOIN Employees E2
ON E1.Salary < E2.Salary;
```

**When to Use**:
- To compare rows based on numerical values (e.g., salaries, sales, or grades).
- To rank or find relative metrics within the table.

---

### **4. Parent-Child Relationship**
**Scenario**: A family tree is stored in a `Persons` table where each person has a parent. You want to list all parent-child pairs.

**Table Structure**:
| **PersonID** | **Name** | **ParentID** |
|--------------|----------|--------------|
| 1            | Alice    | NULL         |
| 2            | Bob      | 1            |
| 3            | Charlie  | 1            |
| 4            | David    | 2            |

**Query**:
```sql
SELECT 
    C.Name AS Child,
    P.Name AS Parent
FROM Persons C
JOIN Persons P
ON C.ParentID = P.PersonID;
```

**When to Use**:
- To traverse parent-child relationships (e.g., family trees, organizational hierarchies).
- To explore recursive relationships within a single table.

---

### **5. Products Bought Together**
**Scenario**: A `Sales` table stores transactions. You want to find pairs of products bought together in the same transaction.

**Table Structure**:
| **TransactionID** | **ProductID** |
|-------------------|---------------|
| 1                 | 101           |
| 1                 | 102           |
| 2                 | 101           |
| 2                 | 103           |

**Query**:
```sql
SELECT 
    S1.ProductID AS ProductA,
    S2.ProductID AS ProductB
FROM Sales S1
JOIN Sales S2
ON S1.TransactionID = S2.TransactionID
   AND S1.ProductID < S2.ProductID;
```

**When to Use**:
- To identify product combinations frequently bought together.
- To improve recommendations or cross-sell strategies.

---

### **6. Identifying Missing Relationships**
**Scenario**: A `Friends` table stores friendships, and you want to find unreciprocated friendships (i.e., `A` is a friend of `B`, but `B` is not a friend of `A`).

**Table Structure**:
| **PersonA** | **PersonB** |
|-------------|-------------|
| Alice       | Bob         |
| Bob         | Alice       |
| Charlie     | Alice       |

**Query**:
```sql
SELECT 
    F1.PersonA,
    F1.PersonB
FROM Friends F1
LEFT JOIN Friends F2
ON F1.PersonA = F2.PersonB AND F1.PersonB = F2.PersonA
WHERE F2.PersonA IS NULL;
```

**When to Use**:
- To identify missing reciprocal relationships in social networks or datasets.

### **Advantages of SELF JOIN**
1. Helps analyze hierarchical and recursive relationships.
2. Simplifies comparisons within the same dataset.
3. Useful for cleaning and validating data.

