
# Table of contents

1. [Setting up roles](#roles)
2. [Setting up users](#users)
3. [Setting up applications](#applications)
4. [Setting up hierarchies](#hierarchy)
5. [Setting up "Setup"](#setup)
6. [Importing and Exporting](#export)

## Setting up roles<a name="roles"></a>
![](https://i.imgur.com/Ai29iqg.png)
List of functions:
1. Add
2. Delete
3. Edit
### Adding new role
![](https://i.imgur.com/DFnkbVI.png)

Add new role by entering the name of the role.
### Delete role
Delete role by clicking on delete button next to the role.
### Edit
Clicking on the edit button will bring up a new window.
![](https://i.imgur.com/cGgCaLl.png)
Permissions can be set for each roles. Access to application can be turned on and off on the list on the left hand side. Finer permissions can be set on the right hand side.

### Application Permissions details
### 1. Column with string data
Column with string data has following conditions:
**1. All**
If set, all data will be return without filter.
	
**2. Match**
If set, data will be filtered so that it matched with the users' detail.
For example, if "department" is set to match, when a user is trying to access the application, user's "department" would be checked, the data will be filtered so that only rows with the same department would be returned.
	
**3. Value**
If set, data will be filtered based on the selected value.
For example, if "sales" and "management" were chosen, only rows with "sales" and "management" would be returned.
	
**4. Match and ...**
If set, data will be filtered based on users' detail as well as the selected value.
For example, if "sales" was chosen, and the user's department is under "finance", rows with both "sales" and "finance" would be returned.

### 2. Column with date data
![](https://i.imgur.com/yE1im95.png)

Column with date data has following conditions:
**1. All**
Same as above.

**2. Match**
Same as above.

**3. Before**
A date can be selected. Data that are **before** the date will be returned. The **day** input can be used to add or minus days starting from the selected day. 

For example, if date chosen is "1 January 2022", and:
1. Day is set to 5, data returned will be filtered as **before 6 January 2022**.
2. Day is set to -2, data returned will be filtered as **before 30 December 2021**. 

If "Today" is checked, the data will be filtered based on "Today" date, day calculation can still be applied.

**4. After**
Same concept with **before**, but data that are **after** will be filtered.

**5. Between**
Combination of **before** and **after**. Data will be filtered within the set timespan.

### 3. Column with number data
**1. All**
Same as above.

**2. Match**
Same as above.

**3. Less than**
Data that are lesser than the input value will be returned.

**4. More than**
Data that are more than the input value will be returned.

**5. Between**
Data that are in between the input value will be returned.

### Need Request
If **Need Request** is turned on, user will not have direct access to the application. A request need to be made before user can access it. The request will be approved by system admin.
### Allow Upper
If **Allow Upper** is turned on, user's upper will be able to view whatever data this user can even if the uppers does not have direct access to the application themselves. Relationship of hierarchies will be explained further in later section.


## Setting up users<a name="users"></a>
List of functions:
1. Add
2. Delete
3. Edit
4. More Settings

### Add Role
![](https://i.imgur.com/XuJqXq6.png)
New user can be added by "Assign" an existing employee that are setup on the client database.
### Edit
![](https://i.imgur.com/gyzMjV0.png)
On the sidebar, "Edit" mode can be turned on to change user's username or role.
### More Settings
1. Access Control
2. Access Permission
### Access Control
![](https://i.imgur.com/gK9XdVo.png)
Under this tab, "Exclusive Abilities" can be turned on. If turned on, the user's role permissions will be overwritten. Permissions can be setup exclusively for this user in this section. Users with exclusive abilities do not have "Allow Upper" as they are excluded from the hierarchy order.
Permissions settings are exactly the same as setting up for roles.

### Access Permission
![](https://i.imgur.com/nWTg58O.png)
User made requests are under "Access Permission" tab. These are the requests made by users that are trying to access application with "Need Request" clause turned on. Admin can "Reject" or "Approve" the request. If "Approve", an expired date would be set. Users that try to access the application after the expired date will need to make the request again.

## Setting up applications<a name="applications"></a>
List of functions:
1. Add
2. Delete

### Adding applications
![](https://i.imgur.com/VXLilxQ.png)
Applications are added by inserting a name and a query that return the application data.

## Setting up hierarchies<a name="hierarchy"></a>
Hierarchy groups will contain of roles. Relationship of uppers and lowers will be set here. Roles in groups that are in the upper can view data from the roles in groups that are lower in hierarchy, IF the "Allow Upper" clause is turned on. 

### Create Hierarchy Group
Create a new group by entering a name for the group

![](https://i.imgur.com/nVcUgzl.png)
### Adding and removing roles to and from the group
By clicking "Add" under the "Contain of" section, a list of created roles can be added to the hierarchy group. To remove, click on the delete icon inside the role.

### Set the group as the direct upper of other group
![](https://i.imgur.com/5XrbUv5.png)
By clicking "Edit" under the "Direct upper of" section, hierarchy groups can be added as "lower" groups.

## Setting up Setup<a name="setup"></a>
![](https://i.imgur.com/s06uVMS.png)
"Setup" page is a page to view at client-side of the database. It is also used to link both ABAC Framework and client-side of the database. Setup table will be used to configure the relationship.

### Explanation of "Setup" table
Sometimes the database will not have the value needed in the same table (eg. a table with only department ID as foreign key but not the name of department itself). "Setup" table will take several parameters, and "join" the related table together so that when the value is needed, the "readable" value will be shown instead.

### Parameters for "Setup" table
1. Name
Identifier. No impact.
2. Table
The table the foreign key at.
3. Field
The name of the column the foreign key at the "Table".
4. Foreign Table
The table the value at.
5. Foreign Key
The name of the column of the foreign key at the "Foreign Table".
6. Foreign Field
The name of the column of the value.

### Case 1:
Table 1: profile
| id | name | department_id |
|----|------|---------------|
| 1  | alan | 1             |

Table 2: department
| id | department |
|----|------------|
| 1  | finance    |

Parameters setting:
1. **Name**: department
2. **Table**: profile
3. **Field**: department_id
4. **Foreign Table**: department
5. **Foreign Key**: id
6. **Foreign Field**: department

Everytime "department" is referenced, the value instead of the ID will be shown.

### Case 2:
Table 1: profile
| id | name  | supervisor |
|----|-------|------------|
| 1  | alan  | 2          |
| 2  | brian |            |

Parameters setting:
1. **Name**: supervisor
2. **Table**: profile
3. **Field**: supervisor
4. **Foreign Table**: profile
5. **Foreign Key**: supervisor
6. **Foreign Field**: name

Value can also point to the same table. In the scenario above, "brian" is the supervisor of "alan", but when queried, the supervisor of "alan" would be shown as "2" instead. The settings above will link the value together so that "brian" would be shown instead.

## Importing and Exporting<a name="export"></a>
Configurations of roles and users can be exported and be imported on the Role and User management page.
> _**Important Note**_
> Roles should be imported before users. Role name should not be duplicated as during the import of the users, the role ID is assigned by checking the name of the roles, not the ID.
