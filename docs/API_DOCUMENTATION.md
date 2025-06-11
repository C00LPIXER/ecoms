# E-commerce API Documentation

## Table of Contents
1. [Introduction](#introduction)
2. [Authentication](#authentication)
3. [Core Modules](#core-modules)
4. [Feature Modules](#feature-modules)
5. [Error Handling](#error-handling)
6. [Rate Limiting](#rate-limiting)
7. [Versioning](#versioning)

## Introduction

### Base URL
```
https://api.ecommerce.com/v1
```

### Common Headers
```json
{
  "Content-Type": "application/json",
  "Accept": "application/json",
  "Authorization": "Bearer <token>"
}
```

### Response Format
```json
{
  "success": true,
  "data": {},
  "error": null,
  "message": "Success message",
  "meta": {
    "pagination": {
      "total": 100,
      "page": 1,
      "limit": 10
    }
  }
}
```

## Authentication

### Register User
```http
POST /auth/register
```

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "securepassword",
  "firstName": "John",
  "lastName": "Doe",
  "phone": "+1234567890"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "user": {
      "id": "user_id",
      "email": "user@example.com",
      "firstName": "John",
      "lastName": "Doe"
    },
    "token": "jwt_token"
  }
}
```

### Login
```http
POST /auth/login
```

**Request Body:**
```json
{
  "email": "user@example.com",
  "password": "securepassword"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "user": {
      "id": "user_id",
      "email": "user@example.com"
    },
    "token": "jwt_token"
  }
}
```

## Core Modules

### Products Module

#### Get Products
```http
GET /products
```

**Query Parameters:**
- `page` (number, optional): Page number
- `limit` (number, optional): Items per page
- `sort` (string, optional): Sort field
- `order` (string, optional): Sort order (asc/desc)
- `category` (string, optional): Category ID
- `search` (string, optional): Search term

**Response:**
```json
{
  "success": true,
  "data": {
    "products": [
      {
        "id": "product_id",
        "name": "Product Name",
        "description": "Product Description",
        "price": 99.99,
        "images": ["url1", "url2"],
        "category": "category_id",
        "stock": 100,
        "attributes": {
          "color": "red",
          "size": "M"
        }
      }
    ]
  },
  "meta": {
    "pagination": {
      "total": 100,
      "page": 1,
      "limit": 10
    }
  }
}
```

#### Get Product Details
```http
GET /products/:id
```

**Response:**
```json
{
  "success": true,
  "data": {
    "product": {
      "id": "product_id",
      "name": "Product Name",
      "description": "Product Description",
      "price": 99.99,
      "images": ["url1", "url2"],
      "category": "category_id",
      "stock": 100,
      "attributes": {
        "color": "red",
        "size": "M"
      },
      "reviews": [
        {
          "id": "review_id",
          "rating": 5,
          "comment": "Great product!",
          "user": "user_id"
        }
      ]
    }
  }
}
```

### Categories Module

#### Get Categories
```http
GET /categories
```

**Query Parameters:**
- `parent` (string, optional): Parent category ID
- `level` (number, optional): Category level

**Response:**
```json
{
  "success": true,
  "data": {
    "categories": [
      {
        "id": "category_id",
        "name": "Category Name",
        "description": "Category Description",
        "parent": "parent_category_id",
        "image": "image_url",
        "level": 1
      }
    ]
  }
}
```

## Feature Modules

### Cart Module

#### Get Cart
```http
GET /cart
```

**Response:**
```json
{
  "success": true,
  "data": {
    "cart": {
      "id": "cart_id",
      "items": [
        {
          "product": {
            "id": "product_id",
            "name": "Product Name",
            "price": 99.99,
            "image": "image_url"
          },
          "quantity": 2,
          "price": 199.98
        }
      ],
      "total": 199.98
    }
  }
}
```

#### Add to Cart
```http
POST /cart/items
```

**Request Body:**
```json
{
  "productId": "product_id",
  "quantity": 2
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "cart": {
      "id": "cart_id",
      "items": [
        {
          "product": {
            "id": "product_id",
            "name": "Product Name",
            "price": 99.99
          },
          "quantity": 2,
          "price": 199.98
        }
      ],
      "total": 199.98
    }
  }
}
```

### Orders Module

#### Create Order
```http
POST /orders
```

**Request Body:**
```json
{
  "items": [
    {
      "productId": "product_id",
      "quantity": 2
    }
  ],
  "shippingAddress": {
    "street": "123 Main St",
    "city": "City",
    "state": "State",
    "country": "Country",
    "zipCode": "12345"
  },
  "paymentMethod": "credit_card",
  "paymentDetails": {
    "cardNumber": "4111111111111111",
    "expiryMonth": "12",
    "expiryYear": "2025",
    "cvv": "123"
  }
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "order": {
      "id": "order_id",
      "status": "pending",
      "items": [
        {
          "product": {
            "id": "product_id",
            "name": "Product Name",
            "price": 99.99
          },
          "quantity": 2,
          "price": 199.98
        }
      ],
      "total": 199.98,
      "shippingAddress": {
        "street": "123 Main St",
        "city": "City",
        "state": "State",
        "country": "Country",
        "zipCode": "12345"
      },
      "paymentMethod": "credit_card",
      "createdAt": "2024-03-20T10:00:00Z"
    }
  }
}
```

### User Module

#### Get User Profile
```http
GET /users/profile
```

**Response:**
```json
{
  "success": true,
  "data": {
    "user": {
      "id": "user_id",
      "email": "user@example.com",
      "firstName": "John",
      "lastName": "Doe",
      "phone": "+1234567890",
      "addresses": [
        {
          "id": "address_id",
          "street": "123 Main St",
          "city": "City",
          "state": "State",
          "country": "Country",
          "zipCode": "12345",
          "isDefault": true
        }
      ]
    }
  }
}
```

#### Update User Profile
```http
PUT /users/profile
```

**Request Body:**
```json
{
  "firstName": "John",
  "lastName": "Doe",
  "phone": "+1234567890"
}
```

**Response:**
```json
{
  "success": true,
  "data": {
    "user": {
      "id": "user_id",
      "email": "user@example.com",
      "firstName": "John",
      "lastName": "Doe",
      "phone": "+1234567890"
    }
  }
}
```

## Error Handling

### Error Response Format
```json
{
  "success": false,
  "error": {
    "code": "ERROR_CODE",
    "message": "Error message",
    "details": {}
  }
}
```

### Common Error Codes
- `AUTH_REQUIRED`: Authentication required
- `INVALID_CREDENTIALS`: Invalid login credentials
- `INVALID_TOKEN`: Invalid or expired token
- `RESOURCE_NOT_FOUND`: Requested resource not found
- `VALIDATION_ERROR`: Invalid request data
- `RATE_LIMIT_EXCEEDED`: Too many requests
- `SERVER_ERROR`: Internal server error

## Rate Limiting

- 100 requests per minute for authenticated users
- 20 requests per minute for unauthenticated users
- Rate limit headers included in response:
  - `X-RateLimit-Limit`
  - `X-RateLimit-Remaining`
  - `X-RateLimit-Reset`

## Versioning

- API version included in URL path
- Current version: v1
- Version deprecation announced 6 months in advance
- Multiple versions supported simultaneously 