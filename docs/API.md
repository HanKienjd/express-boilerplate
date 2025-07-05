# API Documentation

Complete API documentation for the Express.js E-commerce API Boilerplate.

## 🔐 Authentication

All protected endpoints require a JWT token in the Authorization header:

```
Authorization: Bearer <your-jwt-token>
```

### Response Format

All API responses follow this standard format:

```javascript
{
  "success": true,
  "data": {}, // Response data
  "message": "Optional message"
}
```

Error responses:

```javascript
{
  "success": false,
  "error": "Error message",
  "details": {} // Optional error details
}
```

## 🔑 Authentication Endpoints

### 1. User Registration

**POST** `/api/auth/signup`

Register a new user account.

**Request Body:**
```javascript
{
  "firstName": "John",
  "lastName": "Doe",
  "email": "john@example.com",
  "password": "securePassword123",
  "phoneNumber": "+1234567890"
}
```

**Response:**
```javascript
{
  "success": true,
  "data": {
    "user": {
      "id": 1,
      "firstName": "John",
      "lastName": "Doe",
      "email": "john@example.com",
      "phoneNumber": "+1234567890",
      "role": "user",
      "createdAt": "2023-01-01T00:00:00.000Z"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

### 2. User Login

**POST** `/api/auth/signin`

Authenticate user and receive JWT token.

**Request Body:**
```javascript
{
  "email": "john@example.com",
  "password": "securePassword123"
}
```

**Response:**
```javascript
{
  "success": true,
  "data": {
    "user": {
      "id": 1,
      "firstName": "John",
      "lastName": "Doe",
      "email": "john@example.com",
      "role": "user"
    },
    "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..."
  }
}
```

### 3. Get Current User

**GET** `/api/auth/me`

Get current authenticated user information.

**Headers:**
```
Authorization: Bearer <token>
```

**Response:**
```javascript
{
  "success": true,
  "data": {
    "id": 1,
    "firstName": "John",
    "lastName": "Doe",
    "email": "john@example.com",
    "role": "user",
    "createdAt": "2023-01-01T00:00:00.000Z"
  }
}
```

### 4. Forgot Password

**POST** `/api/auth/forgot-password`

Send password reset code to user's email.

**Request Body:**
```javascript
{
  "email": "john@example.com"
}
```

**Response:**
```javascript
{
  "success": true,
  "message": "Password reset code sent to your email"
}
```

### 5. Verify Code

**POST** `/api/auth/verify-code`

Verify the code sent to user's email.

**Request Body:**
```javascript
{
  "email": "john@example.com",
  "code": "123456"
}
```

**Response:**
```javascript
{
  "success": true,
  "data": {
    "verified": true,
    "resetToken": "temporary-reset-token"
  }
}
```

## 📦 Product Endpoints

### 1. Get Products List

**GET** `/api/product`

Get paginated list of products with optional filters.

**Query Parameters:**
- `page` (optional): Page number (default: 1)
- `limit` (optional): Items per page (default: 10)
- `category` (optional): Filter by category ID
- `search` (optional): Search products by name
- `status` (optional): Filter by product status

**Response:**
```javascript
{
  "success": true,
  "data": {
    "products": [
      {
        "id": 1,
        "name": "Product Name",
        "description": "Product description",
        "price": 29.99,
        "stock": 100,
        "categoryId": 1,
        "images": ["image1.jpg", "image2.jpg"],
        "status": "active",
        "createdAt": "2023-01-01T00:00:00.000Z"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 50,
      "pages": 5
    }
  }
}
```

### 2. Get Product Details

**GET** `/api/product/:id`

Get detailed information about a specific product.

**Response:**
```javascript
{
  "success": true,
  "data": {
    "id": 1,
    "name": "Product Name",
    "description": "Detailed product description",
    "price": 29.99,
    "stock": 100,
    "categoryId": 1,
    "category": {
      "id": 1,
      "name": "Electronics"
    },
    "images": ["image1.jpg", "image2.jpg"],
    "specifications": {
      "weight": "1kg",
      "dimensions": "10x10x5cm"
    },
    "status": "active",
    "createdAt": "2023-01-01T00:00:00.000Z"
  }
}
```

### 3. Create Product (Admin Only)

**POST** `/api/product`

Create a new product.

**Headers:**
```
Authorization: Bearer <admin-token>
Content-Type: multipart/form-data
```

**Request Body (Form Data):**
```javascript
{
  "name": "New Product",
  "description": "Product description",
  "price": 29.99,
  "stock": 100,
  "categoryId": 1,
  "images": [File1, File2], // File uploads
  "specifications": JSON.stringify({
    "weight": "1kg",
    "dimensions": "10x10x5cm"
  })
}
```

**Response:**
```javascript
{
  "success": true,
  "data": {
    "id": 2,
    "name": "New Product",
    "description": "Product description",
    "price": 29.99,
    "stock": 100,
    "categoryId": 1,
    "images": ["uploaded-image1.jpg", "uploaded-image2.jpg"],
    "status": "active",
    "createdAt": "2023-01-01T00:00:00.000Z"
  }
}
```

### 4. Update Product (Admin Only)

**PUT** `/api/product/:id`

Update an existing product.

**Headers:**
```
Authorization: Bearer <admin-token>
```

**Request Body:**
```javascript
{
  "name": "Updated Product Name",
  "description": "Updated description",
  "price": 35.99,
  "stock": 75
}
```

**Response:**
```javascript
{
  "success": true,
  "data": {
    "id": 1,
    "name": "Updated Product Name",
    "description": "Updated description",
    "price": 35.99,
    "stock": 75,
    "updatedAt": "2023-01-01T00:00:00.000Z"
  }
}
```

### 5. Delete Product (Admin Only)

**DELETE** `/api/product/:id`

Delete a product.

**Headers:**
```
Authorization: Bearer <admin-token>
```

**Response:**
```javascript
{
  "success": true,
  "message": "Product deleted successfully"
}
```

## 🛒 Shopping Cart Endpoints

### 1. Get Active Cart

**GET** `/api/cart`

Get user's active shopping cart.

**Headers:**
```
Authorization: Bearer <token>
```

**Response:**
```javascript
{
  "success": true,
  "data": {
    "id": 1,
    "userId": 1,
    "items": [
      {
        "id": 1,
        "productId": 1,
        "product": {
          "id": 1,
          "name": "Product Name",
          "price": 29.99,
          "images": ["image1.jpg"]
        },
        "quantity": 2,
        "price": 29.99,
        "subtotal": 59.98
      }
    ],
    "total": 59.98,
    "createdAt": "2023-01-01T00:00:00.000Z"
  }
}
```

### 2. Add to Cart

**POST** `/api/cart`

Add item to shopping cart.

**Headers:**
```
Authorization: Bearer <token>
```

**Request Body:**
```javascript
{
  "productId": 1,
  "quantity": 2
}
```

**Response:**
```javascript
{
  "success": true,
  "data": {
    "id": 1,
    "productId": 1,
    "quantity": 2,
    "price": 29.99,
    "subtotal": 59.98
  }
}
```

### 3. Update Cart Item

**PUT** `/api/cart/:id`

Update quantity of cart item.

**Headers:**
```
Authorization: Bearer <token>
```

**Request Body:**
```javascript
{
  "quantity": 3
}
```

**Response:**
```javascript
{
  "success": true,
  "data": {
    "id": 1,
    "productId": 1,
    "quantity": 3,
    "price": 29.99,
    "subtotal": 89.97
  }
}
```

### 4. Remove from Cart

**DELETE** `/api/cart/:id`

Remove item from cart.

**Headers:**
```
Authorization: Bearer <token>
```

**Response:**
```javascript
{
  "success": true,
  "message": "Item removed from cart"
}
```

### 5. Empty Cart

**DELETE** `/api/cart/empty`

Remove all items from cart.

**Headers:**
```
Authorization: Bearer <token>
```

**Response:**
```javascript
{
  "success": true,
  "message": "Cart emptied successfully"
}
```

## 📋 Order Endpoints

### 1. Get User Orders

**GET** `/api/orders`

Get user's order history.

**Headers:**
```
Authorization: Bearer <token>
```

**Query Parameters:**
- `page` (optional): Page number
- `limit` (optional): Items per page
- `status` (optional): Filter by order status

**Response:**
```javascript
{
  "success": true,
  "data": {
    "orders": [
      {
        "id": 1,
        "userId": 1,
        "status": "pending",
        "total": 59.98,
        "shippingAddress": {
          "street": "123 Main St",
          "city": "New York",
          "state": "NY",
          "zipCode": "10001"
        },
        "createdAt": "2023-01-01T00:00:00.000Z"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 5,
      "pages": 1
    }
  }
}
```

### 2. Get Order Details

**GET** `/api/orders/:id`

Get detailed information about a specific order.

**Headers:**
```
Authorization: Bearer <token>
```

**Response:**
```javascript
{
  "success": true,
  "data": {
    "id": 1,
    "userId": 1,
    "status": "pending",
    "total": 59.98,
    "shippingAddress": {
      "street": "123 Main St",
      "city": "New York",
      "state": "NY",
      "zipCode": "10001"
    },
    "items": [
      {
        "id": 1,
        "productId": 1,
        "product": {
          "id": 1,
          "name": "Product Name",
          "images": ["image1.jpg"]
        },
        "quantity": 2,
        "price": 29.99,
        "subtotal": 59.98
      }
    ],
    "createdAt": "2023-01-01T00:00:00.000Z"
  }
}
```

### 3. Create Order

**POST** `/api/orders`

Create a new order from cart items.

**Headers:**
```
Authorization: Bearer <token>
```

**Request Body:**
```javascript
{
  "shippingAddress": {
    "street": "123 Main St",
    "city": "New York",
    "state": "NY",
    "zipCode": "10001"
  },
  "paymentMethod": "credit_card",
  "notes": "Please deliver between 9 AM - 5 PM"
}
```

**Response:**
```javascript
{
  "success": true,
  "data": {
    "id": 2,
    "userId": 1,
    "status": "pending",
    "total": 59.98,
    "shippingAddress": {
      "street": "123 Main St",
      "city": "New York",
      "state": "NY",
      "zipCode": "10001"
    },
    "createdAt": "2023-01-01T00:00:00.000Z"
  }
}
```

### 4. Update Order Status (Admin Only)

**PUT** `/api/orders/:id/status`

Update order status.

**Headers:**
```
Authorization: Bearer <admin-token>
```

**Request Body:**
```javascript
{
  "status": "shipped",
  "trackingNumber": "1234567890"
}
```

**Response:**
```javascript
{
  "success": true,
  "data": {
    "id": 1,
    "status": "shipped",
    "trackingNumber": "1234567890",
    "updatedAt": "2023-01-01T00:00:00.000Z"
  }
}
```

## 📂 Category Endpoints

### 1. Get Categories

**GET** `/api/category`

Get list of all categories.

**Response:**
```javascript
{
  "success": true,
  "data": [
    {
      "id": 1,
      "name": "Electronics",
      "description": "Electronic devices and gadgets",
      "image": "electronics.jpg",
      "productCount": 25,
      "createdAt": "2023-01-01T00:00:00.000Z"
    }
  ]
}
```

### 2. Create Category (Admin Only)

**POST** `/api/category`

Create a new category.

**Headers:**
```
Authorization: Bearer <admin-token>
```

**Request Body:**
```javascript
{
  "name": "New Category",
  "description": "Category description",
  "image": "category-image.jpg"
}
```

**Response:**
```javascript
{
  "success": true,
  "data": {
    "id": 2,
    "name": "New Category",
    "description": "Category description",
    "image": "category-image.jpg",
    "createdAt": "2023-01-01T00:00:00.000Z"
  }
}
```

### 3. Update Category (Admin Only)

**PUT** `/api/category/:id`

Update an existing category.

**Headers:**
```
Authorization: Bearer <admin-token>
```

**Request Body:**
```javascript
{
  "name": "Updated Category Name",
  "description": "Updated description"
}
```

**Response:**
```javascript
{
  "success": true,
  "data": {
    "id": 1,
    "name": "Updated Category Name",
    "description": "Updated description",
    "updatedAt": "2023-01-01T00:00:00.000Z"
  }
}
```

### 4. Delete Category (Admin Only)

**DELETE** `/api/category/:id`

Delete a category.

**Headers:**
```
Authorization: Bearer <admin-token>
```

**Response:**
```javascript
{
  "success": true,
  "message": "Category deleted successfully"
}
```

## ❤️ Favorites Endpoints

### 1. Add to Favorites

**POST** `/api/favorite`

Add product to user's favorites.

**Headers:**
```
Authorization: Bearer <token>
```

**Request Body:**
```javascript
{
  "productId": 1
}
```

**Response:**
```javascript
{
  "success": true,
  "data": {
    "id": 1,
    "userId": 1,
    "productId": 1,
    "createdAt": "2023-01-01T00:00:00.000Z"
  }
}
```

### 2. Remove from Favorites

**DELETE** `/api/favorite`

Remove product from user's favorites.

**Headers:**
```
Authorization: Bearer <token>
```

**Request Body:**
```javascript
{
  "productId": 1
}
```

**Response:**
```javascript
{
  "success": true,
  "message": "Product removed from favorites"
}
```

## 🔍 Search Endpoints

### 1. Search Products

**GET** `/api/search`

Search for products by name, description, or category.

**Query Parameters:**
- `q`: Search query (required)
- `category`: Filter by category ID
- `minPrice`: Minimum price filter
- `maxPrice`: Maximum price filter
- `page`: Page number
- `limit`: Items per page

**Response:**
```javascript
{
  "success": true,
  "data": {
    "products": [
      {
        "id": 1,
        "name": "Product Name",
        "description": "Product description",
        "price": 29.99,
        "images": ["image1.jpg"],
        "category": {
          "id": 1,
          "name": "Electronics"
        }
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 5,
      "pages": 1
    }
  }
}
```

## 🚚 Shipping Endpoints

### 1. Get User Shipping Addresses

**GET** `/api/shipping`

Get user's saved shipping addresses.

**Headers:**
```
Authorization: Bearer <token>
```

**Response:**
```javascript
{
  "success": true,
  "data": [
    {
      "id": 1,
      "userId": 1,
      "name": "Home Address",
      "street": "123 Main St",
      "city": "New York",
      "state": "NY",
      "zipCode": "10001",
      "country": "USA",
      "isDefault": true,
      "createdAt": "2023-01-01T00:00:00.000Z"
    }
  ]
}
```

### 2. Create Shipping Address

**POST** `/api/shipping`

Create a new shipping address.

**Headers:**
```
Authorization: Bearer <token>
```

**Request Body:**
```javascript
{
  "name": "Work Address",
  "street": "456 Business Ave",
  "city": "New York",
  "state": "NY",
  "zipCode": "10002",
  "country": "USA",
  "isDefault": false
}
```

**Response:**
```javascript
{
  "success": true,
  "data": {
    "id": 2,
    "userId": 1,
    "name": "Work Address",
    "street": "456 Business Ave",
    "city": "New York",
    "state": "NY",
    "zipCode": "10002",
    "country": "USA",
    "isDefault": false,
    "createdAt": "2023-01-01T00:00:00.000Z"
  }
}
```

### 3. Update Shipping Address

**PUT** `/api/shipping/:id`

Update an existing shipping address.

**Headers:**
```
Authorization: Bearer <token>
```

**Request Body:**
```javascript
{
  "name": "Updated Address Name",
  "street": "789 New Street",
  "isDefault": true
}
```

**Response:**
```javascript
{
  "success": true,
  "data": {
    "id": 1,
    "name": "Updated Address Name",
    "street": "789 New Street",
    "isDefault": true,
    "updatedAt": "2023-01-01T00:00:00.000Z"
  }
}
```

### 4. Delete Shipping Address

**DELETE** `/api/shipping/:id`

Delete a shipping address.

**Headers:**
```
Authorization: Bearer <token>
```

**Response:**
```javascript
{
  "success": true,
  "message": "Shipping address deleted successfully"
}
```

## 👥 User Management Endpoints (Admin Only)

### 1. Get All Users

**GET** `/api/user`

Get list of all users (admin only).

**Headers:**
```
Authorization: Bearer <admin-token>
```

**Query Parameters:**
- `page`: Page number
- `limit`: Items per page
- `role`: Filter by user role
- `search`: Search by name or email

**Response:**
```javascript
{
  "success": true,
  "data": {
    "users": [
      {
        "id": 1,
        "firstName": "John",
        "lastName": "Doe",
        "email": "john@example.com",
        "role": "user",
        "isActive": true,
        "createdAt": "2023-01-01T00:00:00.000Z"
      }
    ],
    "pagination": {
      "page": 1,
      "limit": 10,
      "total": 50,
      "pages": 5
    }
  }
}
```

### 2. Update User

**PUT** `/api/user/:id`

Update user information (admin only).

**Headers:**
```
Authorization: Bearer <admin-token>
```

**Request Body:**
```javascript
{
  "firstName": "Updated Name",
  "role": "admin",
  "isActive": false
}
```

**Response:**
```javascript
{
  "success": true,
  "data": {
    "id": 1,
    "firstName": "Updated Name",
    "role": "admin",
    "isActive": false,
    "updatedAt": "2023-01-01T00:00:00.000Z"
  }
}
```

### 3. Delete User

**DELETE** `/api/user/:id`

Delete a user account (admin only).

**Headers:**
```
Authorization: Bearer <admin-token>
```

**Response:**
```javascript
{
  "success": true,
  "message": "User deleted successfully"
}
```

## 📊 Report Endpoints (Admin Only)

### 1. Get Dashboard Info

**GET** `/api/report/info`

Get dashboard statistics and information.

**Headers:**
```
Authorization: Bearer <admin-token>
```

**Response:**
```javascript
{
  "success": true,
  "data": {
    "totalUsers": 1250,
    "totalProducts": 150,
    "totalOrders": 3500,
    "totalRevenue": 125000.50,
    "recentOrders": [
      {
        "id": 1,
        "userId": 1,
        "total": 59.98,
        "status": "pending",
        "createdAt": "2023-01-01T00:00:00.000Z"
      }
    ],
    "topProducts": [
      {
        "id": 1,
        "name": "Product Name",
        "salesCount": 100,
        "revenue": 2999.00
      }
    ]
  }
}
```

## 🎯 Error Codes

Common HTTP status codes used in the API:

- **200 OK**: Request successful
- **201 Created**: Resource created successfully
- **400 Bad Request**: Invalid request parameters
- **401 Unauthorized**: Authentication required
- **403 Forbidden**: Access denied
- **404 Not Found**: Resource not found
- **422 Unprocessable Entity**: Validation errors
- **500 Internal Server Error**: Server error

## 🔄 Rate Limiting

The API implements rate limiting to prevent abuse:

- **Default**: 100 requests per 15 minutes per IP
- **Authentication endpoints**: 5 requests per minute per IP
- **File upload endpoints**: 10 requests per minute per user

When rate limit is exceeded, the API returns:

```javascript
{
  "success": false,
  "error": "Too many requests",
  "retryAfter": 60
}
```

## 📝 Validation Rules

### User Registration
- Email: Must be valid email format
- Password: Minimum 8 characters
- Phone: Must be valid phone number format

### Product Creation
- Name: Required, minimum 3 characters
- Price: Must be positive number
- Stock: Must be non-negative integer

### Order Creation
- Shipping address: All fields required
- Cart must not be empty
- All products must be available in stock

---

For more information, visit the [Development Guide](DEVELOPMENT.md). 