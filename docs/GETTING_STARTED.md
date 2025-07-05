# Getting Started

Welcome to the Express.js E-commerce API Boilerplate! This guide will help you get up and running quickly.

## 🚀 Quick Setup (5 minutes)

### 1. Clone and Install
```bash
git clone <repository-url>
cd express-boilerplate
npm install
```

### 2. Environment Setup
Create a `.env` file in the root directory:
```bash
# Copy the environment variables from DEVELOPMENT.md
# Minimum required variables:
DB_CLIENT=mysql
DB_HOST=localhost
DB_DATABASE=app
DB_USERNAME=app
DB_PASSWORD=123456
JWT_SECRET=your-super-secret-jwt-key-here
NODE_ENV=development
PORT=8080
```

### 3. Database Setup
```bash
# Option 1: Use Docker (Recommended)
npm run docker

# Option 2: Local MySQL
# Install MySQL and create database manually
# Then run:
npm run migrate:run
npm run seed:run
```

### 4. Start Development Server
```bash
npm run dev
```

🎉 **Your API is now running on `http://localhost:8080`**

## 📚 Documentation Files

| File | Description |
|------|-------------|
| `README.md` | Main project documentation |
| `DEVELOPMENT.md` | Detailed development guide |
| `API.md` | Complete API documentation |
| `GETTING_STARTED.md` | This quick start guide |
| `postman_collection.json` | Postman collection for API testing |

## 🧪 Test Your Installation

### 1. Health Check
```bash
curl http://localhost:8080/api/auth/me
# Should return: {"success": false, "error": "Authorization header required"}
```

### 2. Create a Test User
```bash
curl -X POST http://localhost:8080/api/auth/signup \
  -H "Content-Type: application/json" \
  -d '{
    "firstName": "Test",
    "lastName": "User",
    "email": "test@example.com",
    "password": "password123"
  }'
```

### 3. Login
```bash
curl -X POST http://localhost:8080/api/auth/signin \
  -H "Content-Type: application/json" \
  -d '{
    "email": "test@example.com",
    "password": "password123"
  }'
```

## 🛠️ Development Tools

### Postman Collection
1. Import `postman_collection.json` into Postman
2. Set the `baseUrl` variable to `http://localhost:8080/api`
3. After login, copy the JWT token to the `token` variable

### Database Tools
- **phpMyAdmin**: `http://localhost:81` (if using Docker)
- **MySQL Workbench**: Connect to `localhost:3306`

## 🔧 Common Issues & Solutions

### Database Connection Issues
```bash
# Check if MySQL is running
mysql -u app -p

# If using Docker, check container status
docker ps

# Reset database
npm run migrate:rollback
npm run migrate:run
npm run seed:run
```

### Port Already in Use
```bash
# Kill process on port 8080
lsof -ti:8080 | xargs kill -9

# Or change port in .env file
PORT=3000
```

### JWT Token Issues
```bash
# Generate a new JWT secret
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

## 🚀 Next Steps

1. **Read the Documentation**
   - Check `API.md` for detailed endpoint documentation
   - Review `DEVELOPMENT.md` for development best practices

2. **Configure Services**
   - Set up Cloudinary for image uploads
   - Configure email service (Gmail/SendGrid)
   - Set up AWS S3 for file storage

3. **Customize for Your Project**
   - Modify database schema as needed
   - Add new API endpoints
   - Implement business logic

4. **Testing**
   - Run tests: `npm test`
   - Use Postman collection for API testing
   - Write additional tests for your features

## 💡 Tips

- **Development**: Use `npm run dev` for auto-reload
- **Production**: Use `npm start` with PM2 for production
- **Database**: Always backup before running migrations
- **Security**: Never commit `.env` files to version control

## 📞 Support

If you need help:
1. Check the documentation files
2. Review the code comments
3. Create an issue in the repository
4. Contact the development team

Happy coding! 🎉 