# mdma-subd

## Project Topic
Online Store

## Student
- Full Name: Vladislav Stepanov
- Group Number: 153503

## Functional Requirements
1. **User Authentication.**
2. **User Management (CRUD).**
3. **Role-Based System.**
4. **User Activity Logging.**
5. **Product Management (CRUD).**
6. **Shopping Cart.**
7. **Payment and Order Processing.**
8. **Product Reviews and Ratings.**
9. **Product Search.**
10. **Product Categories Filtering.**
11. **Discount system for specific products.**
12. **Products store different attibutes.**
13. **SKU.** *Each product can have multiple SKUs representing different variants of the same product.*
14. **Store management(for Admin)**

## Database Model
![Logical Database Model](myStore.svg)

## Entity Descriptions


**Table: users**

This table stores information about users of the online marketplace platform.

**Fields:**
- `id` (uuid, primary key): Unique identifier for the user.
- `first_name` (varchar): The user's first name.
- `last_name` (varchar): The user's last name.
- `roles` (enum): An array of roles assigned to the user (e.g., 'customer', 'seller', 'admin').
- `email` (varchar): The user's email address.
- `password` (varchar): The hashed password of the user.

**Constraints:**
- The `id` field is a primary key.
- The `email` field is unique to ensure that each email address corresponds to a unique user account.
- Passwords should be securely hashed before storage.

**Relationships:**
- None
  
---

**Table: shopping_carts**

This table stores information about shopping carts associated with users of the online marketplace.

**Fields:**
- `id` (uuid, primary key): Unique identifier for the shopping cart.
- `user_id` (uuid, foreign key): References the `users` table, indicating the user who owns the shopping cart.
- `created_at` (timestamp with time zone): The timestamp when the shopping cart was created.

**Constraints:**
- The `id` field is a primary key.
- The `user_id` field references the `users` table to establish a many-to-one relationship between users and their shopping carts.

**Relationships:**
- Many-to-one relationship with the `users` table via the `user_id` field, indicating that each shopping cart belongs to a single user.

---

**Table: cart_items**

This table stores information about items added to a user's shopping cart in the online marketplace.

**Fields:**
- `id` (uuid, primary key): Unique identifier for the cart item.
- `product_id` (uuid, foreign key): References the `products` table, indicating the product added to the cart.
- `quantity` (integer): The quantity of the product added to the cart.

**Constraints:**
- The `id` field is a primary key.
- The `product_id` field references the `products` table to establish a many-to-one relationship between cart items and products.
- The `quantity` field ensures that the quantity is a non-negative integer.

**Relationships:**
- Many-to-one relationship with the `products` table via the `product_id` field, indicating which product is added to the cart.

---

**Table: reviews**

This table stores information about product reviews submitted by users in the online marketplace.

**Fields:**
- `id` (uuid, primary key): Unique identifier for the review.
- `product_id` (uuid, foreign key): References the `products` table, indicating the product being reviewed.
- `user_id` (uuid, foreign key): References the `users` table, identifying the user who submitted the review.
- `created_at` (timestamp with time zone): The timestamp when the review was created.
- `rating` (integer): The rating given by the user (e.g., 1 to 5 stars).
- `review_text` (text): The text content of the review.

**Constraints:**
- The `id` field is a primary key.
- The `product_id` field references the `products` table to establish a many-to-one relationship between reviews and products.
- The `user_id` field references the `users` table to establish a many-to-one relationship between reviews and users.
- The `rating` field ensures that the rating is within a predefined range (e.g., 1 to 5).

**Relationships:**
- Many-to-one relationship with the `products` table via the `product_id` field, indicating which product is being reviewed.
- Many-to-one relationship with the `users` table via the `user_id` field, identifying the user who submitted the review.

---


**Table: products**

This table stores information about products available in the online marketplace.

**Fields:**
- `id` (uuid, primary key): Unique identifier for the product.
- `name` (varchar): The name or title of the product.
- `description` (text): A detailed description of the product.
- `price` (decimal): The price of the product.
- `category_id` (uuid, foreign key): References the `product_categories` table, indicating the category to which the product belongs.
- `store_id` (uuid, foreign key): References the `stores` table, indicating the store that sells the product.
- `photos` (array of varchar): URLs or file paths to product images.

**Constraints:**
- The `id` field is a primary key.
- The `category_id` field references the `product_categories` table to establish a many-to-one relationship between products and categories.
- The `store_id` field references the `stores` table to establish a many-to-one relationship between products and stores.

**Relationships:**
- Many-to-one relationship with the `product_categories` table via the `category_id` field, indicating the category of the product.
- Many-to-one relationship with the `stores` table via the `store_id` field, indicating the store that sells the product.
---

**Table: product_options**

This table stores information about product options or attributes associated with products in the online marketplace.

**Fields:**
- `product_id` (uuid, foreign key): References the `products` table, indicating the product to which the option belongs.
- `attribute_name` (varchar): The name or type of the product attribute (e.g., "Color," "Size").
- `attribute_value` (varchar): The value of the product attribute (e.g., "Red," "Large").

**Constraints:**
- The combination of `product_id`, `attribute_name`, and `attribute_value` is unique to ensure that each attribute is associated with a specific product and has a unique name and value.

**Relationships:**
- Many-to-one relationship with the `products` table via the `product_id` field, indicating the product to which the option or attribute belongs.

---

**Table: product_sku**

This table stores information about product stock keeping units (SKUs) for products in the online marketplace.

**Fields:**
- `id` (uuid, primary key): Unique identifier for the product SKU.
- `product_id` (uuid, foreign key): References the `products` table, indicating the product to which the SKU belongs.
- `sku` (varchar): The stock keeping unit (SKU) identifier for the product.
- `available_quantity` (integer): The quantity of the product available in stock.
- `is_available` (boolean): Indicates whether the product SKU is currently available for purchase.

**Constraints:**
- The `id` field is a primary key.
- The `product_id` field references the `products` table to establish a many-to-one relationship between SKUs and products.
- The `sku` field should be unique to ensure that each SKU is distinct within the context of a product.
- The `is_available` field helps track the availability status of the SKU.

**Relationships:**
- Many-to-one relationship with the `products` table via the `product_id` field, indicating the product to which the SKU belongs.

---


**Table: stores**

This table stores information about stores or shops in the online marketplace.

**Fields:**
- `id` (uuid, primary key): Unique identifier for the store.
- `owner_id` (uuid, foreign key): References the `users` table, indicating the user who owns the store.
- `name` (varchar): The name or title of the store.
- `logo` (varchar): URL or file path to the store's logo.
- `address` (varchar): The physical address of the store.
- `is_enabled` (boolean): Indicates whether the store is currently enabled for operation.
- `contacts` (jsonb): JSON object storing store contact information. For example:
  ```json
  {
      "phone": "123-456-7890",
      "email": "store@example.com"
  }
  ```
- `social_networks` (jsonb): JSON object storing links to the store's social network profiles. For example:
  ```json
  {
      "facebook": "https://www.facebook.com/storepage",
      "twitter": "https://twitter.com/storepage"
  }
  ```

**Constraints:**
- The `id` field is a primary key.
- The `owner_id` field references the `users` table, indicating the user who owns the store.

**Relationships:**
- Many-to-one relationship with the `users` table via the `owner_id` field, indicating the owner of the store.

---

**Table: user_tokens**

This table stores information about user authentication tokens for user sessions in the online marketplace.

**Fields:**
- `id` (uuid, primary key): Unique identifier for the user token.
- `user_id` (uuid, foreign key): References the `users` table, identifying the user associated with the token.
- `token` (varchar): The authentication token associated with the user's session.

**Constraints:**
- The `id` field is a primary key.
- The `user_id` field references the `users` table to establish a many-to-one relationship between user tokens and users.
- The `token` field stores the authentication token generated for the user's session.

**Relationships:**
- Many-to-one relationship with the `users` table via the `user_id` field, indicating the user associated with the token.

---

**Table: order_product**

This table represents the many-to-many relationship between orders and products in the online marketplace, allowing multiple products to be associated with each order, and vice versa.

**Fields:**
- `id` (uuid, primary key): Unique identifier for the order-product relationship (optional, you can add it if needed).
- `product_id` (uuid, foreign key): References the `products` table, indicating the product included in the order.
- `order_id` (uuid, foreign key): References the `orders` table, indicating the order to which the product belongs.
- `quantity` (integer): The quantity of the specific product included in the order.
- `discount_id` (uuid, foreign key): References the `discounts` table, indicating any applied discount for this product in the order.

**Constraints:**
- The combination of `product_id` and `order_id` should be unique to prevent duplicate products in the same order.

**Relationships:**
- Many-to-many relationship between the `products` table and the `orders` table through the `product_id` and `order_id` fields, indicating the products included in each order and the orders that include specific products.
- Many-to-one relationship with the `discounts` table via the `discount_id` field, indicating any applied discount for this product in the order.

---

**Table: product_categories**

This table stores information about product categories in the online marketplace.

**Fields:**
- `id` (uuid, primary key): Unique identifier for the product category.
- `name` (varchar): The name or title of the product category.
- `description` (text): A detailed description of the product category.
- `keywords` (varchar[]): Keywords associated with the category to aid in search and categorization.
- `parent_category` (uuid, foreign key): References the `product_categories` table, indicating the parent category to which this category belongs, if applicable.

**Constraints:**
- The `id` field is a primary key.
- The `parent_category` field references the `product_categories` table, allowing for hierarchical categorization.

**Relationships:**
- Many-to-one relationship with the `product_categories` table via the `parent_category` field, allowing categories to be organized hierarchically.

---

**Table: discounts**

This table stores information about discounts that can be applied to products in the online marketplace.

**Fields:**
- `id` (uuid, primary key): Unique identifier for the discount.
- `name` (varchar): The name or title of the discount.
- `description` (text): A detailed description of the discount.
- `percentage` (decimal): The discount percentage (e.g., 10%).
- `start_date` (date): The start date when the discount becomes valid.
- `end_date` (date): The end date when the discount expires.
- `is_active` (boolean): Indicates whether the discount is currently active.
- `applicability` (uuid, foreign key): References the `products` table, indicating which products the discount is applicable to.

**Constraints:**
- The `id` field is a primary key.
- The `applicability` field references the `products` table, specifying the products to which the discount can be applied.

**Relationships:**
- Many-to-one relationship with the `products` table via the `applicability` field, indicating the products to which the discount is applicable.

---
