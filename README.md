# EasyShop Backend Project

## Overview

This project is an online shop with a finished frontend application. 
The backend of the project needed more methods implemented or altered in order to:
- register a new user
- add a new user
- login as admin
- add a category as admin
- get newly added categories by ID
- delete categories as admin
- and search products

The project was divided into phases. 
## Phase 1

In phase 1 the class MySqlCategoryDao needed its methods implemented.
The methods are:
- getAllCategories()
- getById()
- create()
- update()
- delete()

In the class CategoriesController methods needed to be implemented. 
The methods are:
- getAll()
- getById()
- getProductsById()
- addCategory()
- updateCategory()
- deleteCategory()

The logic in these methods uses the methods of the MySqlCategoryDao.

Only admins are able to add, update and delete categories. 

## Phase 2

### Bug 1

The search functionality did not return the expected results. 
This bug needed to be fixed in the MySqlProductDao class, in the search() method. 

The SQL query inside the method was missing a clause for the maximum price:

"SELECT * FROM products " +
"WHERE (category_id = ? OR ? = -1) " +
"   AND (price <= ? OR ? = -1) " +
**"   AND (price >= ? OR ? = -1) " // added to look for products between min and max price** + "   AND (color = ? OR ? = '') ";

The second thing that needed to be added in the search method in order for the search functionality to work properly was:
**statement.setBigDecimal(3, maxPrice);**
**statement.setBigDecimal(4, maxPrice);**

This was needed in order for this SQL clause to have the proper parameter:

**"   AND (price <= ? OR ? = -1) "**

### Bug 2

The second bug in the code caused a new product to be created when the admin tried to update a product. 
This was solved by changing the code inside the ProductController class, inside the updateProduct() method. 

This code needed to be changed from:
**productDao.create(product); // creates a new product**

To:
**productDao.update(id, product); // updates an existing product**

## Acknowledgements

Many thanks to Paul Kimball, A'sha Shepard, Javier Chavez and Mohammed Ahmed for their help with this project. 

