# Express.js E-commerce API Boilerplate

A comprehensive Express.js boilerplate for building e-commerce applications with authentication, product management, shopping cart, orders, and more.

## 🚀 Features

- **Authentication System**
  - User registration and login
  - JWT token-based authentication
  - Password reset functionality
  - Email verification with codes

- **Product Management**
  - Product CRUD operations
  - Category and subcategory management
  - Product search functionality
  - 3D file upload support
  - Product favorites system

- **Shopping Cart & Orders**
  - Shopping cart management
  - Order processing system
  - Order status tracking
  - Order history

- **User Management**
  - User profiles
  - Role-based access control
  - User management for admins

- **Additional Features**
  - File uploads (Cloudinary integration)
  - Email notifications
  - Shipping management
  - Banner/poster management
  - Reporting system
  - Province/division lookup

## 🛠️ Tech Stack

- **Backend Framework**: Express.js
- **Database**: MySQL with Knex.js query builder
- **ORM**: Objection.js
- **Authentication**: JWT
- **File Storage**: Cloudinary, AWS S3
- **Email Service**: Nodemailer
- **Validation**: Joi
- **Testing**: Mocha, Chai, Sinon
- **Documentation**: Built-in API documentation

## 📦 Installation

### Prerequisites

- Node.js (v14 or higher)
- MySQL database
- Git

### Local Development Setup

1. **Clone the repository**
```bash
git clone <repository-url>
cd express-boilerplate
```

2. **Install dependencies**
```bash
npm install
```

3. **Environment Configuration**
Create a `.env` file in the root directory:
```bash
cp .env.example .env
```

4. **Configure environment variables** (see Environment Variables section)

5. **Database Setup**
```bash
# Run database migrations
npm run migrate:run

# Seed initial data
npm run seed:run
```

6. **Start the development server**
```bash
npm run dev
```

The server will start on `http://localhost:8080`

## 🐳 Docker Setup

### Using Docker Compose

1. **Start services with Docker**
```bash
npm run docker
```

This will start:
- MySQL database on port 3306
- phpMyAdmin on port 81 (optional)
- MinIO object storage on port 9000 (optional)

2. **Run the application**
```bash
npm run dev
```

### Full Docker Setup

To run the entire application in Docker:

1. **Build and start all services**
```bash
docker-compose up -d
```

## 🔧 Environment Variables

Create a `.env` file with the following variables:

```env
# Database Configuration
DB_CLIENT=mysql
DB_HOST=localhost
DB_DATABASE=app
DB_USERNAME=app
DB_PASSWORD=123456

# JWT Configuration
JWT_SECRET=your-jwt-secret-key
JWT_EXPIRES_IN=7d

# Email Configuration
MAIL_HOST=smtp.gmail.com
MAIL_PORT=587
MAIL_USERNAME=your-email@gmail.com
MAIL_PASSWORD=your-app-password
MAIL_FROM=your-email@gmail.com

# Cloudinary Configuration
CLOUDINARY_CLOUD_NAME=your-cloud-name
CLOUDINARY_API_KEY=your-api-key
CLOUDINARY_API_SECRET=your-api-secret

# AWS S3 Configuration
AWS_ACCESS_KEY_ID=your-access-key
AWS_SECRET_ACCESS_KEY=your-secret-key
AWS_REGION=us-east-1
AWS_BUCKET=your-bucket-name

# Application Configuration
NODE_ENV=development
PORT=8080
```

## 📊 Database Schema

The application uses the following main tables:

- `users` - User accounts and profiles
- `categories` - Product categories
- `products` - Product information
- `cart` - Shopping cart items
- `orders` - Order information
- `order_details` - Order item details
- `favorites` - User favorite products
- `shipping_details` - Shipping information
- `banners` - Marketing banners

## 🛣️ API Routes

### Authentication Routes
- `POST /api/auth/signup` - User registration
- `POST /api/auth/signin` - User login
- `POST /api/auth/forgot-password` - Password reset request
- `POST /api/auth/verify-code` - Verify email code
- `GET /api/auth/me` - Get current user profile

### Product Routes
- `GET /api/product` - Get products list
- `GET /api/product/:id` - Get product details
- `POST /api/product` - Create new product (Admin)
- `PUT /api/product/:id` - Update product (Admin)
- `DELETE /api/product/:id` - Delete product (Admin)

### Cart Routes
- `GET /api/cart` - Get user's active cart
- `POST /api/cart` - Add item to cart
- `PUT /api/cart/:id` - Update cart item
- `DELETE /api/cart/:id` - Remove item from cart
- `DELETE /api/cart/empty` - Empty entire cart

### Order Routes
- `GET /api/orders` - Get user's orders
- `GET /api/orders/:id` - Get order details
- `POST /api/orders` - Create new order
- `PUT /api/orders/:id/status` - Update order status (Admin)

### Category Routes
- `GET /api/category` - Get categories list
- `POST /api/category` - Create category (Admin)
- `PUT /api/category/:id` - Update category (Admin)
- `DELETE /api/category/:id` - Delete category (Admin)

### User Management Routes
- `GET /api/user` - Get users list (Admin)
- `PUT /api/user/:id` - Update user (Admin)
- `DELETE /api/user/:id` - Delete user (Admin)

## 📜 Available Scripts

```bash
# Development
npm run dev          # Start development server with nodemon
npm start           # Start production server

# Database
npm run migrate:run     # Run all migrations
npm run migrate:up      # Run next migration
npm run migrate:down    # Rollback last migration
npm run migrate:rollback # Rollback all migrations
npm run seed:run        # Run database seeds

# Testing & Quality
npm test            # Run test suite
npm run eslint      # Run ESLint code linting

# Docker
npm run docker      # Start Docker services
```

## 🏗️ Project Structure

```
express-boilerplate/
├── app/
│   ├── config/          # Configuration files
│   ├── enums/           # Enum definitions
│   ├── helpers/         # Helper functions
│   ├── http/
│   │   ├── controllers/ # Route controllers
│   │   ├── middlewares/ # Custom middlewares
│   │   └── services/    # Business logic services
│   ├── models/          # Database models
│   ├── routes/          # API routes
│   ├── utils/           # Utility functions
│   └── views/           # View templates
├── database/
│   ├── migrations/      # Database migrations
│   └── seeds/           # Database seeds
├── tests/               # Test files
├── uploads/             # File uploads directory
├── app.js              # Main application file
├── knexfile.js         # Database configuration
├── package.json        # Dependencies and scripts
└── docker-compose.yml  # Docker services configuration
```

## 🧪 Testing

Run the test suite:

```bash
npm test
```

The project uses:
- **Mocha** - Test framework
- **Chai** - Assertion library
- **Sinon** - Test spies, stubs, and mocks

## 🔒 Security Features

- **JWT Authentication** - Secure token-based authentication
- **Password Hashing** - bcrypt for password encryption
- **Input Validation** - Joi schema validation
- **CORS** - Cross-origin resource sharing configuration
- **Rate Limiting** - (To be implemented)
- **Helmet** - (To be implemented)

## 📝 Code Style

The project follows Standard.js coding conventions:
- 2 space indentation
- Single quotes for strings
- No semicolons
- Proper error handling
- Descriptive variable names

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📄 License

This project is licensed under the MIT License.

## 🆘 Support

For support and questions:
- Create an issue in the repository
- Contact the development team

## 🔄 Changelog

### v1.0.0
- Initial release
- Complete authentication system
- Product management
- Shopping cart functionality
- Order processing
- User management
- File upload integration

---

**Built with ❤️ by [Your Name]** 