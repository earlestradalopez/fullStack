# 16-Module Full Stack Development Program
**Role:** Full Stack Developer Trainee
**Duration:** 8 weeks, 2 modules/week
**Modality:** Hybrid with asynchronous-first materials; optional live touchpoints

## Stack Overview
- **Languages:** JavaScript and TypeScript (JS first, TS introduced by week 3–4)
- **Frontend:** React (Vite primary + CRA exposure), Context + React Query, CSS Modules (+ optional Tailwind)
- **Backend:** NestJS with Node.js (TypeScript)
- **Databases:** PostgreSQL (raw SQL via node-postgres) and MongoDB (native driver)
- **Auth:** Local auth with JWT + HTTP-only cookies + bcrypt hashing
- **Testing:** Vitest (unit) and Playwright (E2E)
- **Tools:** GitHub, VS Code, Postman, Chrome DevTools, Docker
- **Deployment:** Local-only; offline-friendly

## Assessment Structure
- **Labs:** 60% (graded with rubric on correctness, code quality, tests, docs)
- **Quizzes:** 20% (5–8 questions per module; 70% passing threshold)
- **Code Reviews:** 20% (checkpoints after modules 5, 10, 16)

---

# Global Environment Setup (Week 1, before Module 1)

## Prerequisites
Cross-platform setup for Windows, macOS, and Linux

### Core Tools Installation
1. **Node.js LTS** (v20.x or latest LTS)
   ```bash
   # Verify installation
   node -v
   npm -v
   ```

2. **VS Code** with essential extensions:
   - ESLint
   - Prettier
   - Docker
   - GitLens
   - Thunder Client (Postman alternative)

3. **Git Configuration**
   ```bash
   git --version
   git config --global user.name "Your Name"
   git config --global user.email "your.email@example.com"

   # Generate SSH key for GitHub
   ssh-keygen -t ed25519 -C "your.email@example.com"
   ```

4. **Docker Desktop**
   ```bash
   docker --version
   docker-compose --version
   ```

5. **Database Clients**
   ```bash
   # PostgreSQL client
   psql --version

   # Optional: MongoDB Compass (GUI client)
   ```

6. **Postman** (for API testing)

### Repository Setup
Create monorepo structure:
```
fullstack-training/
├── frontend/                 # Vite React app
├── backend/                  # NestJS app
├── db/
│   ├── postgres/
│   │   ├── 001_init.sql
│   │   └── 002_seed.sql
│   └── mongo/
│       └── seed.js
├── scripts/                  # Validation scripts
├── docs/                     # Offline PDFs
├── docker-compose.yml
├── package.json             # Root package with workspaces
└── README.md
```

### Docker Compose Services
```yaml
# docker-compose.yml
version: '3.8'
services:
  postgres:
    image: postgres:15
    environment:
      POSTGRES_DB: training_db
      POSTGRES_USER: dev_user
      POSTGRES_PASSWORD: dev_pass
    ports:
      - "5432:5432"
    volumes:
      - ./db/postgres:/docker-entrypoint-initdb.d
      - postgres_data:/var/lib/postgresql/data

  mongo:
    image: mongo:7
    ports:
      - "27017:27017"
    volumes:
      - ./db/mongo:/docker-entrypoint-initdb.d
      - mongo_data:/data/db

volumes:
  postgres_data:
  mongo_data:
```

### Verification Script
Create `scripts/verify-env.js`:
```javascript
#!/usr/bin/env node
const { execSync } = require('child_process');
const fs = require('fs');

const checks = [
  { name: 'Node.js', cmd: 'node -v' },
  { name: 'npm', cmd: 'npm -v' },
  { name: 'Git', cmd: 'git --version' },
  { name: 'Docker', cmd: 'docker --version' },
  { name: 'PostgreSQL', cmd: 'psql --version' }
];

console.log('🔍 Environment Verification\n');

checks.forEach(({ name, cmd }) => {
  try {
    const result = execSync(cmd, { encoding: 'utf8' }).trim();
    console.log(`✅ ${name}: ${result}`);
  } catch (error) {
    console.log(`❌ ${name}: Not installed or not in PATH`);
  }
});

// Check VS Code extensions
try {
  const extensions = execSync('code --list-extensions', { encoding: 'utf8' });
  const required = ['ms-vscode.vscode-eslint', 'esbenp.prettier-vscode'];
  required.forEach(ext => {
    if (extensions.includes(ext)) {
      console.log(`✅ VS Code extension: ${ext}`);
    } else {
      console.log(`❌ VS Code extension missing: ${ext}`);
    }
  });
} catch (error) {
  console.log('⚠️  Could not check VS Code extensions');
}
```

---

# Module 1: Foundations
**Week 1 | Dev environment, CLI, Git, JavaScript essentials**

## Learning Objectives
- Set up professional development environment
- Master command line interface basics
- Understand Git version control fundamentals
- Write and debug JavaScript code
- Use VS Code effectively

## Prerequisites
- Completed environment setup
- Basic computer literacy

## Hands-On Labs

### Build Lab: "Hello DevEnv"
**Goal:** Set up complete development environment and verify all tools work correctly.

#### Steps:
1. **Clone starter repository**
   ```bash
   git clone <starter-repo-url>
   cd fullstack-training
   npm install
   ```

2. **Verify environment**
   ```bash
   npm run verify:env
   ```

3. **Create first JavaScript file**
   ```javascript
   // src/hello.js
   const greeting = 'Hello, Full Stack World!';

   function greetUser(name = 'Developer') {
     return `${greeting} Welcome, ${name}!`;
   }

   console.log(greetUser());
   console.log(greetUser('Alice'));

   module.exports = { greetUser };
   ```

4. **Run and test**
   ```bash
   node src/hello.js
   npm test -- hello.test.js
   ```

#### Validation Checks:
- All environment tools respond correctly
- ESLint and Prettier configs exist and work
- Git repository initialized with proper remotes
- JavaScript file runs without errors

### Debugging Lab: JavaScript Fundamentals
**Goal:** Fix 10 common JavaScript mistakes to build debugging skills.

#### Buggy Code Examples:
```javascript
// Bug 1: Undefined variable
console.log(userName); // Should declare first

// Bug 2: Wrong equality operator
if (age == '18') { } // Should use ===

// Bug 3: NaN comparison
if (result == NaN) { } // Should use isNaN()

// Bug 4: Scope issues
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100); // Should use let
}

// Bug 5: Missing return
function add(a, b) {
  a + b; // Missing return
}

// Continue with 5 more bugs...
```

#### Common Errors & Solutions:
- **PATH issues:** Restart terminal after installations
- **Git SSH:** Add public key to GitHub
- **npm permissions:** Use npm prefix or nvm
- **VS Code extensions:** Reload window after installation

## Assessment Quiz Topics
1. What's the difference between `let`, `const`, and `var`?
2. When should you use `===` instead of `==`?
3. How do you check if a value is `NaN`?
4. What's the purpose of `.gitignore`?
5. How do you undo the last git commit?
6. What does the VS Code debugger breakpoint do?
7. How do you run a specific npm script?

## Automated Checks
**Script:** `scripts/verify-env.js`
- Checks tool versions
- Verifies ESLint/Prettier configuration
- Tests basic JavaScript execution
- Validates Git setup

## Extensions for Advanced Learners
- **Command-line productivity:** Learn `npx`, explore Node REPL
- **Git aliases:** Set up useful Git shortcuts
- **VS Code customization:** Custom snippets and keybindings

---

# Module 2: React Fundamentals
**Week 1 | Vite + CSS Modules, accessibility-first**

## Learning Objectives
- Understand React component architecture
- Master JSX syntax and React patterns
- Implement accessibility best practices
- Use CSS Modules for styling
- Handle component state and effects

## Prerequisites
- JavaScript fundamentals (Module 1)
- Basic HTML/CSS knowledge

## Hands-On Labs

### Build Lab: Product List UI with Vite
**Goal:** Create a responsive, accessible product listing application.

#### Steps:
1. **Initialize Vite React app**
   ```bash
   cd frontend
   npm create vite@latest . -- --template react
   npm install
   ```

2. **Set up CSS Modules**
   ```css
   /* ProductList.module.css */
   .container {
     max-width: 1200px;
     margin: 0 auto;
     padding: 1rem;
   }

   .grid {
     display: grid;
     grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
     gap: 1.5rem;
   }

   .card {
     border: 1px solid #ddd;
     border-radius: 8px;
     padding: 1rem;
     background: white;
     box-shadow: 0 2px 4px rgba(0,0,0,0.1);
   }

   .title {
     font-size: 1.25rem;
     font-weight: 600;
     margin-bottom: 0.5rem;
     color: #1a1a1a;
   }

   .price {
     font-size: 1.125rem;
     font-weight: 700;
     color: #059669;
   }
   ```

3. **Create accessible components**
   ```jsx
   // components/ProductCard.jsx
   import styles from './ProductCard.module.css';

   export function ProductCard({ product }) {
     return (
       <article className={styles.card} role="article">
         <img
           src={product.image}
           alt={product.name}
           className={styles.image}
         />
         <h3 className={styles.title}>{product.name}</h3>
         <p className={styles.description}>{product.description}</p>
         <div className={styles.price} aria-label={`Price: ${product.price}`}>
           ${product.price}
         </div>
         <button
           className={styles.addButton}
           aria-label={`Add ${product.name} to cart`}
         >
           Add to Cart
         </button>
       </article>
     );
   }
   ```

   ```jsx
   // components/ProductList.jsx
   import { useState, useEffect } from 'react';
   import { ProductCard } from './ProductCard';
   import styles from './ProductList.module.css';

   export function ProductList() {
     const [products, setProducts] = useState([]);
     const [loading, setLoading] = useState(true);

     useEffect(() => {
       // Simulate API call with static data
       setTimeout(() => {
         setProducts([
           { id: 1, name: 'Laptop', price: 999, description: 'High-performance laptop' },
           { id: 2, name: 'Phone', price: 699, description: 'Latest smartphone' },
           { id: 3, name: 'Tablet', price: 399, description: 'Portable tablet' }
         ]);
         setLoading(false);
       }, 1000);
     }, []);

     if (loading) {
       return (
         <div className={styles.loading} role="status" aria-live="polite">
           Loading products...
         </div>
       );
     }

     return (
       <main className={styles.container}>
         <h1>Our Products</h1>
         <div className={styles.grid} role="list">
           {products.map(product => (
             <ProductCard key={product.id} product={product} />
           ))}
         </div>
       </main>
     );
   }
   ```

#### Validation Checks:
- Components render without errors
- CSS Modules classes apply correctly
- ARIA roles and labels present
- Responsive design works on mobile

### Debugging Lab: React State and Rendering Issues
**Goal:** Fix common React bugs related to state management and component lifecycle.

#### Bug Scenarios:
```jsx
// Bug 1: Missing key prop
{products.map(product =>
  <ProductCard product={product} /> // Missing key
)}

// Bug 2: Stale closure in useEffect
const [count, setCount] = useState(0);
useEffect(() => {
  const timer = setInterval(() => {
    setCount(count + 1); // Stale closure
  }, 1000);
  return () => clearInterval(timer);
}, []); // Missing dependency

// Bug 3: Direct state mutation
const addProduct = () => {
  products.push(newProduct); // Direct mutation
  setProducts(products);
};

// Bug 4: Missing ARIA labels
<button onClick={handleAdd}>Add</button> // No accessible name
```

#### Common Errors & Solutions:
- **useEffect dependency warnings:** Add all dependencies or use callback pattern
- **CSS not applying:** Check CSS Modules import syntax
- **ARIA violations:** Use axe-core DevTools extension
- **State not updating:** Avoid direct mutations, use functional updates

## Assessment Quiz Topics
1. What is the virtual DOM and why is it useful?
2. When would you use `useEffect` vs `useState`?
3. Why are keys important in React lists?
4. What's the difference between controlled and uncontrolled components?
5. How do CSS Modules prevent style conflicts?
6. What ARIA roles should you use for a product grid?
7. How do you handle loading states accessibly?

## Automated Checks
**Script:** `npm run verify:frontend:render`
- Playwright test checking DOM structure
- ARIA role verification
- CSS Modules class application
- Responsive breakpoint testing

## Extensions for Advanced Learners
- **Tailwind variant:** Rebuild ProductList using Tailwind CSS
- **Custom hooks:** Extract loading logic into reusable hook
- **Error boundaries:** Add error handling for failed renders

---

# Module 3: NestJS Fundamentals
**Week 2 | REST API basics with TypeScript**

## Learning Objectives
- Understand NestJS architecture and dependency injection
- Create RESTful API endpoints
- Implement proper error handling
- Use TypeScript effectively in backend development
- Apply validation and DTOs

## Prerequisites
- JavaScript/TypeScript basics
- Understanding of HTTP methods
- Command line proficiency

## Hands-On Labs

### Build Lab: Products API with NestJS
**Goal:** Build a complete CRUD API for product management with proper validation and error handling.

#### Steps:
1. **Initialize NestJS project**
   ```bash
   cd backend
   npm i -g @nestjs/cli
   nest new . --package-manager npm
   npm install class-validator class-transformer
   ```

2. **Create Product DTOs**
   ```typescript
   // src/products/dto/create-product.dto.ts
   import { IsString, IsNumber, IsOptional, Min, MaxLength } from 'class-validator';

   export class CreateProductDto {
     @IsString()
     @MaxLength(100)
     name: string;

     @IsString()
     @MaxLength(500)
     description: string;

     @IsNumber()
     @Min(0)
     price: number;

     @IsOptional()
     @IsString()
     category?: string;

     @IsOptional()
     @IsString()
     image?: string;
   }
   ```

   ```typescript
   // src/products/dto/update-product.dto.ts
   import { PartialType } from '@nestjs/mapped-types';
   import { CreateProductDto } from './create-product.dto';

   export class UpdateProductDto extends PartialType(CreateProductDto) {}
   ```

3. **Create Product Entity**
   ```typescript
   // src/products/entities/product.entity.ts
   export class Product {
     id: number;
     name: string;
     description: string;
     price: number;
     category?: string;
     image?: string;
     createdAt: Date;
     updatedAt: Date;
   }
   ```

4. **Implement Products Service**
   ```typescript
   // src/products/products.service.ts
   import { Injectable, NotFoundException } from '@nestjs/common';
   import { CreateProductDto } from './dto/create-product.dto';
   import { UpdateProductDto } from './dto/update-product.dto';
   import { Product } from './entities/product.entity';

   @Injectable()
   export class ProductsService {
     private products: Product[] = [];
     private nextId = 1;

     create(createProductDto: CreateProductDto): Product {
       const product: Product = {
         id: this.nextId++,
         ...createProductDto,
         createdAt: new Date(),
         updatedAt: new Date(),
       };
       this.products.push(product);
       return product;
     }

     findAll(): Product[] {
       return this.products;
     }

     findOne(id: number): Product {
       const product = this.products.find(p => p.id === id);
       if (!product) {
         throw new NotFoundException(`Product with ID ${id} not found`);
       }
       return product;
     }

     update(id: number, updateProductDto: UpdateProductDto): Product {
       const productIndex = this.products.findIndex(p => p.id === id);
       if (productIndex === -1) {
         throw new NotFoundException(`Product with ID ${id} not found`);
       }

       const updatedProduct = {
         ...this.products[productIndex],
         ...updateProductDto,
         updatedAt: new Date(),
       };
       this.products[productIndex] = updatedProduct;
       return updatedProduct;
     }

     remove(id: number): void {
       const productIndex = this.products.findIndex(p => p.id === id);
       if (productIndex === -1) {
         throw new NotFoundException(`Product with ID ${id} not found`);
       }
       this.products.splice(productIndex, 1);
     }
   }
   ```

5. **Create Products Controller**
   ```typescript
   // src/products/products.controller.ts
   import {
     Controller,
     Get,
     Post,
     Body,
     Patch,
     Param,
     Delete,
     ParseIntPipe,
     HttpCode,
     HttpStatus,
   } from '@nestjs/common';
   import { ProductsService } from './products.service';
   import { CreateProductDto } from './dto/create-product.dto';
   import { UpdateProductDto } from './dto/update-product.dto';

   @Controller('products')
   export class ProductsController {
     constructor(private readonly productsService: ProductsService) {}

     @Post()
     @HttpCode(HttpStatus.CREATED)
     create(@Body() createProductDto: CreateProductDto) {
       return this.productsService.create(createProductDto);
     }

     @Get()
     findAll() {
       return this.productsService.findAll();
     }

     @Get(':id')
     findOne(@Param('id', ParseIntPipe) id: number) {
       return this.productsService.findOne(id);
     }

     @Patch(':id')
     update(
       @Param('id', ParseIntPipe) id: number,
       @Body() updateProductDto: UpdateProductDto
     ) {
       return this.productsService.update(id, updateProductDto);
     }

     @Delete(':id')
     @HttpCode(HttpStatus.NO_CONTENT)
     remove(@Param('id', ParseIntPipe) id: number) {
       return this.productsService.remove(id);
     }
   }
   ```

6. **Set up validation globally**
   ```typescript
   // src/main.ts
   import { NestFactory } from '@nestjs/core';
   import { ValidationPipe } from '@nestjs/common';
   import { AppModule } from './app.module';

   async function bootstrap() {
     const app = await NestFactory.create(AppModule);

     app.useGlobalPipes(new ValidationPipe({
       whitelist: true,
       forbidNonWhitelisted: true,
       transform: true,
     }));

     app.enableCors({
       origin: 'http://localhost:5173', // Vite dev server
       credentials: true,
     });

     await app.listen(3000);
   }
   bootstrap();
   ```

#### Validation Checks:
- All CRUD endpoints respond correctly
- Validation errors return 400 status
- Not found errors return 404 status
- CORS configured for frontend

### Debugging Lab: Common NestJS Issues
**Goal:** Identify and fix typical problems in NestJS applications.

#### Bug Scenarios:
```typescript
// Bug 1: Missing provider
@Module({
  controllers: [ProductsController],
  // Missing ProductsService in providers
})

// Bug 2: Circular dependency
@Injectable()
export class ServiceA {
  constructor(private serviceB: ServiceB) {}
}

@Injectable()
export class ServiceB {
  constructor(private serviceA: ServiceA) {}
}

// Bug 3: Wrong HTTP status codes
@Post()
@HttpCode(HttpStatus.OK) // Should be CREATED
create() {}

// Bug 4: Missing validation
@Post()
create(@Body() data: any) { // Should use DTO
  return this.service.create(data);
}
```

#### Common Errors & Solutions:
- **Module not importing service:** Add to providers array
- **Environment variables not loading:** Install @nestjs/config
- **CORS errors:** Configure origins properly
- **Validation not working:** Enable global validation pipe

## Assessment Quiz Topics
1. What is dependency injection and how does NestJS implement it?
2. What's the difference between @Injectable() and @Controller()?
3. When should you use DTOs vs interfaces?
4. What HTTP status codes should you return for different operations?
5. How do validation pipes work in NestJS?
6. What's the purpose of exception filters?
7. How do you handle environment variables in NestJS?

## Automated Checks
**Script:** `npm run verify:api:routes`
- Tests all CRUD endpoints with supertest
- Validates response formats and status codes
- Checks error handling scenarios
- Verifies CORS configuration

## Extensions for Advanced Learners
- **OpenAPI documentation:** Add Swagger decorators
- **Custom decorators:** Create validation decorators
- **Interceptors:** Add logging and response transformation

---

# Module 4: PostgreSQL with Raw SQL
**Week 2 | Relational modeling, node-postgres integration**

## Learning Objectives
- Design normalized database schemas
- Write efficient SQL queries (DDL/DML)
- Understand transactions and ACID properties
- Implement database indexing strategies
- Integrate PostgreSQL with Node.js using node-postgres

## Prerequisites
- Basic SQL knowledge
- NestJS fundamentals (Module 3)
- Understanding of relational concepts

## Hands-On Labs

### Build Lab: Product Database Schema
**Goal:** Create a complete product management database with proper relationships, indexes, and seed data.

#### Steps:
1. **Design database schema**
   ```sql
   -- db/postgres/001_init.sql
   -- Create database schema for product management

   CREATE EXTENSION IF NOT EXISTS "uuid-ossp";

   -- Categories table
   CREATE TABLE categories (
     id SERIAL PRIMARY KEY,
     name VARCHAR(100) NOT NULL UNIQUE,
     description TEXT,
     created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
     updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );

   -- Products table
   CREATE TABLE products (
     id SERIAL PRIMARY KEY,
     name VARCHAR(100) NOT NULL,
     description TEXT,
     price DECIMAL(10,2) NOT NULL CHECK (price >= 0),
     category_id INTEGER REFERENCES categories(id) ON DELETE SET NULL,
     sku VARCHAR(50) UNIQUE,
     stock_quantity INTEGER DEFAULT 0 CHECK (stock_quantity >= 0),
     image_url TEXT,
     is_active BOOLEAN DEFAULT true,
     created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
     updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );

   -- Suppliers table
   CREATE TABLE suppliers (
     id SERIAL PRIMARY KEY,
     name VARCHAR(100) NOT NULL,
     email VARCHAR(255) UNIQUE,
     phone VARCHAR(20),
     address TEXT,
     created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );

   -- Product suppliers junction table (many-to-many)
   CREATE TABLE product_suppliers (
     product_id INTEGER REFERENCES products(id) ON DELETE CASCADE,
     supplier_id INTEGER REFERENCES suppliers(id) ON DELETE CASCADE,
     cost_price DECIMAL(10,2),
     PRIMARY KEY (product_id, supplier_id)
   );

   -- Create indexes for performance
   CREATE INDEX idx_products_category_id ON products(category_id);
   CREATE INDEX idx_products_sku ON products(sku);
   CREATE INDEX idx_products_name ON products(name);
   CREATE INDEX idx_products_active ON products(is_active);
   CREATE INDEX idx_products_price ON products(price);

   -- Create updated_at trigger function
   CREATE OR REPLACE FUNCTION update_updated_at_column()
   RETURNS TRIGGER AS $$
   BEGIN
     NEW.updated_at = CURRENT_TIMESTAMP;
     RETURN NEW;
   END;
   $$ language 'plpgsql';

   -- Apply trigger to products table
   CREATE TRIGGER update_products_updated_at
     BEFORE UPDATE ON products
     FOR EACH ROW
     EXECUTE FUNCTION update_updated_at_column();

   CREATE TRIGGER update_categories_updated_at
     BEFORE UPDATE ON categories
     FOR EACH ROW
     EXECUTE FUNCTION update_updated_at_column();
   ```

2. **Create seed data**
   ```sql
   -- db/postgres/002_seed.sql
   -- Insert sample data for development and testing

   -- Insert categories
   INSERT INTO categories (name, description) VALUES
   ('Electronics', 'Electronic devices and gadgets'),
   ('Clothing', 'Apparel and fashion items'),
   ('Books', 'Physical and digital books'),
   ('Home & Garden', 'Home improvement and gardening supplies');

   -- Insert suppliers
   INSERT INTO suppliers (name, email, phone, address) VALUES
   ('TechSupply Co', 'contact@techsupply.com', '+1-555-0101', '123 Tech St, Silicon Valley, CA'),
   ('Fashion Forward', 'orders@fashionforward.com', '+1-555-0102', '456 Style Ave, New York, NY'),
   ('Book Distributors Inc', 'sales@bookdist.com', '+1-555-0103', '789 Literature Ln, Boston, MA');

   -- Insert products
   INSERT INTO products (name, description, price, category_id, sku, stock_quantity, image_url) VALUES
   ('Wireless Headphones', 'High-quality bluetooth headphones with noise cancellation', 199.99, 1, 'WH-001', 50, 'https://example.com/headphones.jpg'),
   ('Smartphone', 'Latest generation smartphone with 5G capability', 899.99, 1, 'SP-001', 25, 'https://example.com/smartphone.jpg'),
   ('Cotton T-Shirt', 'Comfortable 100% cotton t-shirt in multiple colors', 24.99, 2, 'TS-001', 100, 'https://example.com/tshirt.jpg'),
   ('Programming Book', 'Learn modern web development', 45.99, 3, 'BK-001', 30, 'https://example.com/progbook.jpg'),
   ('Garden Tools Set', 'Complete set of essential gardening tools', 89.99, 4, 'GT-001', 15, 'https://example.com/tools.jpg');

   -- Insert product-supplier relationships
   INSERT INTO product_suppliers (product_id, supplier_id, cost_price) VALUES
   (1, 1, 120.00),
   (2, 1, 650.00),
   (3, 2, 15.00),
   (4, 3, 25.00),
   (5, 1, 55.00);
   ```

3. **Set up node-postgres connection**
   ```bash
   cd backend
   npm install pg @types/pg
   ```

   ```typescript
   // src/database/database.service.ts
   import { Injectable, OnModuleInit, OnModuleDestroy } from '@nestjs/common';
   import { Pool, PoolClient } from 'pg';

   @Injectable()
   export class DatabaseService implements OnModuleInit, OnModuleDestroy {
     private pool: Pool;

     async onModuleInit() {
       this.pool = new Pool({
         host: process.env.DB_HOST || 'localhost',
         port: parseInt(process.env.DB_PORT) || 5432,
         database: process.env.DB_NAME || 'training_db',
         user: process.env.DB_USER || 'dev_user',
         password: process.env.DB_PASSWORD || 'dev_pass',
         max: 20,
         idleTimeoutMillis: 30000,
         connectionTimeoutMillis: 2000,
       });
     }

     async onModuleDestroy() {
       await this.pool.end();
     }

     async getClient(): Promise<PoolClient> {
       return this.pool.connect();
     }

     async query(text: string, params?: any[]): Promise<any> {
       const client = await this.getClient();
       try {
         const result = await client.query(text, params);
         return result;
       } finally {
         client.release();
       }
     }

     async transaction<T>(callback: (client: PoolClient) => Promise<T>): Promise<T> {
       const client = await this.getClient();
       try {
         await client.query('BEGIN');
         const result = await callback(client);
         await client.query('COMMIT');
         return result;
       } catch (error) {
         await client.query('ROLLBACK');
         throw error;
       } finally {
         client.release();
       }
     }
   }
   ```

4. **Update Products Service with PostgreSQL**
   ```typescript
   // src/products/products.service.ts
   import { Injectable, NotFoundException } from '@nestjs/common';
   import { DatabaseService } from '../database/database.service';
   import { CreateProductDto } from './dto/create-product.dto';
   import { UpdateProductDto } from './dto/update-product.dto';
   import { Product } from './entities/product.entity';

   @Injectable()
   export class ProductsService {
     constructor(private readonly db: DatabaseService) {}

     async create(createProductDto: CreateProductDto): Promise<Product> {
       const query = `
         INSERT INTO products (name, description, price, category_id, sku, stock_quantity, image_url)
         VALUES ($1, $2, $3, $4, $5, $6, $7)
         RETURNING *
       `;

       const values = [
         createProductDto.name,
         createProductDto.description,
         createProductDto.price,
         createProductDto.categoryId,
         createProductDto.sku,
         createProductDto.stockQuantity || 0,
         createProductDto.imageUrl
       ];

       const result = await this.db.query(query, values);
       return result.rows[0];
     }

     async findAll(limit = 50, offset = 0): Promise<Product[]> {
       const query = `
         SELECT p.*, c.name as category_name
         FROM products p
         LEFT JOIN categories c ON p.category_id = c.id
         WHERE p.is_active = true
         ORDER BY p.created_at DESC
         LIMIT $1 OFFSET $2
       `;

       const result = await this.db.query(query, [limit, offset]);
       return result.rows;
     }

     async findOne(id: number): Promise<Product> {
       const query = `
         SELECT p.*, c.name as category_name
         FROM products p
         LEFT JOIN categories c ON p.category_id = c.id
         WHERE p.id = $1 AND p.is_active = true
       `;

       const result = await this.db.query(query, [id]);

       if (result.rows.length === 0) {
         throw new NotFoundException(`Product with ID ${id} not found`);
       }

       return result.rows[0];
     }

     async update(id: number, updateProductDto: UpdateProductDto): Promise<Product> {
       return this.db.transaction(async (client) => {
         // First check if product exists
         const checkQuery = 'SELECT id FROM products WHERE id = $1 AND is_active = true';
         const checkResult = await client.query(checkQuery, [id]);

         if (checkResult.rows.length === 0) {
           throw new NotFoundException(`Product with ID ${id} not found`);
         }

         // Build dynamic update query
         const fields = Object.keys(updateProductDto);
         const setClause = fields.map((field, index) => `${field} = $${index + 2}`).join(', ');
         const values = [id, ...Object.values(updateProductDto)];

         const updateQuery = `
           UPDATE products
           SET ${setClause}
           WHERE id = $1
           RETURNING *
         `;

         const result = await client.query(updateQuery, values);
         return result.rows[0];
       });
     }

     async remove(id: number): Promise<void> {
       const query = 'UPDATE products SET is_active = false WHERE id = $1';
       const result = await this.db.query(query, [id]);

       if (result.rowCount === 0) {
         throw new NotFoundException(`Product with ID ${id} not found`);
       }
     }

     async findByCategory(categoryId: number): Promise<Product[]> {
       const query = `
         SELECT p.*, c.name as category_name
         FROM products p
         LEFT JOIN categories c ON p.category_id = c.id
         WHERE p.category_id = $1 AND p.is_active = true
         ORDER BY p.name
       `;

       const result = await this.db.query(query, [categoryId]);
       return result.rows;
     }

     async searchProducts(searchTerm: string): Promise<Product[]> {
       const query = `
         SELECT p.*, c.name as category_name
         FROM products p
         LEFT JOIN categories c ON p.category_id = c.id
         WHERE p.is_active = true
         AND (p.name ILIKE $1 OR p.description ILIKE $1)
         ORDER BY p.name
       `;

       const result = await this.db.query(query, [`%${searchTerm}%`]);
       return result.rows;
     }
   }
   ```

#### Validation Checks:
- Database schema created successfully
- Seed data inserted properly
- Indexes created and functional
- Node.js can connect and query database

### Debugging Lab: SQL and Performance Issues
**Goal:** Identify and fix common database problems including performance bottlenecks.

#### Bug Scenarios:
```sql
-- Bug 1: Missing foreign key constraint
CREATE TABLE products (
  id SERIAL PRIMARY KEY,
  category_id INTEGER -- Missing REFERENCES
);

-- Bug 2: N+1 Query Problem
-- Instead of multiple queries:
SELECT * FROM products WHERE category_id = 1;
SELECT * FROM categories WHERE id = 1;
SELECT * FROM categories WHERE id = 2;
-- Use JOIN:
SELECT p.*, c.name as category_name
FROM products p
LEFT JOIN categories c ON p.category_id = c.id;

-- Bug 3: Missing index on frequently queried column
-- Add index: CREATE INDEX idx_products_name ON products(name);

-- Bug 4: Transaction not properly handled
-- Wrong:
await client.query('BEGIN');
await client.query('INSERT INTO products...');
// If error occurs here, no ROLLBACK
await client.query('INSERT INTO categories...');
await client.query('COMMIT');
```

#### Common Errors & Solutions:
- **Connection issues:** Check Docker container status
- **Schema not loading:** Verify volume mount in docker-compose
- **Slow queries:** Use EXPLAIN ANALYZE to identify missing indexes
- **Transaction deadlocks:** Keep transactions short and consistent

## Assessment Quiz Topics
1. What's the difference between INNER JOIN and LEFT JOIN?
2. When should you use indexes and what are the trade-offs?
3. What are the ACID properties of transactions?
4. How do you prevent SQL injection in parameterized queries?
5. What's the difference between SERIAL and UUID primary keys?
6. When should you use foreign key constraints?
7. How do you optimize a slow query?

## Automated Checks
**Script:** `npm run verify:pg:schema`
- Connects to PostgreSQL and verifies table structure
- Checks that indexes exist and are being used
- Validates foreign key relationships
- Tests basic CRUD operations

## Extensions for Advanced Learners
- **Query optimization:** Write complex queries with multiple JOINs
- **Database migrations:** Create a migration system for schema changes
- **Stored procedures:** Implement business logic in the database

---

*[Continuing with remaining 12 modules...]*

# Module 5: MongoDB with Native Driver
**Week 3 | Document modeling, CRUD operations**

## Learning Objectives
- Understand document-based data modeling
- Implement CRUD operations with MongoDB native driver
- Design effective document schemas and indexes
- Handle data validation and relationships in MongoDB
- Optimize queries for performance

## Prerequisites
- JavaScript/TypeScript proficiency
- Basic understanding of NoSQL concepts
- NestJS fundamentals

## Hands-On Labs

### Build Lab: Inventory Collection with MongoDB
**Goal:** Create a comprehensive inventory management system using MongoDB with proper indexing and validation.

#### Steps:
1. **Install MongoDB driver**
   ```bash
   cd backend
   npm install mongodb @types/mongodb
   ```

2. **Set up MongoDB connection service**
   ```typescript
   // src/database/mongodb.service.ts
   import { Injectable, OnModuleInit, OnModuleDestroy } from '@nestjs/common';
   import { MongoClient, Db, Collection } from 'mongodb';

   @Injectable()
   export class MongoDbService implements OnModuleInit, OnModuleDestroy {
     private client: MongoClient;
     private db: Db;

     async onModuleInit() {
       const uri = process.env.MONGO_URI || 'mongodb://localhost:27017';
       this.client = new MongoClient(uri);
       await this.client.connect();
       this.db = this.client.db(process.env.MONGO_DB_NAME || 'inventory_db');

       // Create indexes
       await this.createIndexes();
     }

     async onModuleDestroy() {
       await this.client.close();
     }

     getDb(): Db {
       return this.db;
     }

     getCollection<T = any>(name: string): Collection<T> {
       return this.db.collection<T>(name);
     }

     private async createIndexes() {
       const inventoryCollection = this.getCollection('inventory');

       // Create compound index for efficient queries
       await inventoryCollection.createIndex({ productId: 1, warehouseId: 1 });
       await inventoryCollection.createIndex({ productId: 1 });
       await inventoryCollection.createIndex({ warehouseId: 1 });
       await inventoryCollection.createIndex({ lastUpdated: -1 });

       // Text index for search functionality
       await inventoryCollection.createIndex({
         productName: 'text',
         description: 'text'
       });

       // Partial index for low stock items
       await inventoryCollection.createIndex(
         { quantity: 1 },
         { partialFilterExpression: { quantity: { $lt: 10 } } }
       );
     }
   }
   ```

3. **Create inventory document schemas**
   ```typescript
   // src/inventory/schemas/inventory.schema.ts
   import { ObjectId } from 'mongodb';

   export interface InventoryItem {
     _id?: ObjectId;
     productId: string;
     productName: string;
     description: string;
     sku: string;
     warehouseId: string;
     warehouseName: string;
     quantity: number;
     reservedQuantity: number;
     reorderLevel: number;
     costPrice: number;
     sellingPrice: number;
     lastRestockDate?: Date;
     lastUpdated: Date;
     status: 'active' | 'discontinued' | 'backordered';
     tags: string[];
     attributes: {
       color?: string;
       size?: string;
       weight?: number;
       dimensions?: {
         length: number;
         width: number;
         height: number;
       };
     };
     supplier: {
       id: string;
       name: string;
       contactEmail: string;
     };
     movements: Array<{
       type: 'in' | 'out' | 'adjustment';
       quantity: number;
       reason: string;
       timestamp: Date;
       userId: string;
     }>;
   }

   export interface Warehouse {
     _id?: ObjectId;
     name: string;
     code: string;
     address: {
       street: string;
       city: string;
       state: string;
       zipCode: string;
       country: string;
     };
     capacity: number;
     manager: {
       name: string;
       email: string;
     };
     isActive: boolean;
     createdAt: Date;
   }
   ```

4. **Implement inventory service with validation**
   ```typescript
   // src/inventory/inventory.service.ts
   import { Injectable, NotFoundException, BadRequestException } from '@nestjs/common';
   import { ObjectId } from 'mongodb';
   import { MongoDbService } from '../database/mongodb.service';
   import { InventoryItem, Warehouse } from './schemas/inventory.schema';
   import { CreateInventoryDto, UpdateInventoryDto } from './dto';

   @Injectable()
   export class InventoryService {
     constructor(private readonly mongoDb: MongoDbService) {}

     async createInventoryItem(createDto: CreateInventoryDto): Promise<InventoryItem> {
       const collection = this.mongoDb.getCollection<InventoryItem>('inventory');

       // Check if item already exists for this warehouse
       const existing = await collection.findOne({
         productId: createDto.productId,
         warehouseId: createDto.warehouseId
       });

       if (existing) {
         throw new BadRequestException('Inventory item already exists for this product and warehouse');
       }

       const inventoryItem: InventoryItem = {
         ...createDto,
         reservedQuantity: 0,
         lastUpdated: new Date(),
         status: 'active',
         movements: [{
           type: 'in',
           quantity: createDto.quantity,
           reason: 'Initial stock',
           timestamp: new Date(),
           userId: 'system'
         }]
       };

       const result = await collection.insertOne(inventoryItem);
       return { ...inventoryItem, _id: result.insertedId };
     }

     async findAll(options: {
       page?: number;
       limit?: number;
       warehouseId?: string;
       status?: string;
       lowStock?: boolean;
     } = {}): Promise<{ items: InventoryItem[]; total: number }> {
       const collection = this.mongoDb.getCollection<InventoryItem>('inventory');
       const { page = 1, limit = 50, warehouseId, status, lowStock } = options;

       // Build filter
       const filter: any = {};
       if (warehouseId) filter.warehouseId = warehouseId;
       if (status) filter.status = status;
       if (lowStock) filter.$expr = { $lt: ['$quantity', '$reorderLevel'] };

       const skip = (page - 1) * limit;

       const [items, total] = await Promise.all([
         collection.find(filter)
           .sort({ lastUpdated: -1 })
           .skip(skip)
           .limit(limit)
           .toArray(),
         collection.countDocuments(filter)
       ]);

       return { items, total };
     }

     async findById(id: string): Promise<InventoryItem> {
       const collection = this.mongoDb.getCollection<InventoryItem>('inventory');

       if (!ObjectId.isValid(id)) {
         throw new BadRequestException('Invalid inventory item ID');
       }

       const item = await collection.findOne({ _id: new ObjectId(id) });
       if (!item) {
         throw new NotFoundException(`Inventory item with ID ${id} not found`);
       }

       return item;
     }

     async updateQuantity(
       id: string,
       quantity: number,
       reason: string,
       userId: string
     ): Promise<InventoryItem> {
       const collection = this.mongoDb.getCollection<InventoryItem>('inventory');

       if (!ObjectId.isValid(id)) {
         throw new BadRequestException('Invalid inventory item ID');
       }

       const currentItem = await this.findById(id);
       const quantityDiff = quantity - currentItem.quantity;

       const movement = {
         type: quantityDiff > 0 ? 'in' as const : 'out' as const,
         quantity: Math.abs(quantityDiff),
         reason,
         timestamp: new Date(),
         userId
       };

       const result = await collection.findOneAndUpdate(
         { _id: new ObjectId(id) },
         {
           $set: {
             quantity,
             lastUpdated: new Date(),
             ...(quantity > currentItem.quantity && { lastRestockDate: new Date() })
           },
           $push: { movements: movement }
         },
         { returnDocument: 'after' }
       );

       if (!result.value) {
         throw new NotFoundException(`Inventory item with ID ${id} not found`);
       }

       return result.value;
     }

     async searchInventory(searchTerm: string): Promise<InventoryItem[]> {
       const collection = this.mongoDb.getCollection<InventoryItem>('inventory');

       return collection.find({
         $text: { $search: searchTerm }
       }).toArray();
     }

     async getLowStockItems(warehouseId?: string): Promise<InventoryItem[]> {
       const collection = this.mongoDb.getCollection<InventoryItem>('inventory');

       const filter: any = {
         $expr: { $lt: ['$quantity', '$reorderLevel'] }
       };

       if (warehouseId) {
         filter.warehouseId = warehouseId;
       }

       return collection.find(filter)
         .sort({ quantity: 1 })
         .toArray();
     }

     async getInventoryReport(warehouseId: string): Promise<any> {
       const collection = this.mongoDb.getCollection<InventoryItem>('inventory');

       const pipeline = [
         { $match: { warehouseId } },
         {
           $group: {
             _id: '$status',
             totalItems: { $sum: 1 },
             totalQuantity: { $sum: '$quantity' },
             totalValue: { $sum: { $multiply: ['$quantity', '$costPrice'] } },
             avgQuantity: { $avg: '$quantity' }
           }
         },
         {
           $project: {
             status: '$_id',
             totalItems: 1,
             totalQuantity: 1,
             totalValue: { $round: ['$totalValue', 2] },
             avgQuantity: { $round: ['$avgQuantity', 2] }
           }
         }
       ];

       return collection.aggregate(pipeline).toArray();
     }
   }
   ```

5. **Create seed data script**
   ```javascript
   // db/mongo/seed.js
   db = db.getSiblingDB('inventory_db');

   // Insert warehouses
   db.warehouses.insertMany([
     {
       name: 'Main Warehouse',
       code: 'WH001',
       address: {
         street: '123 Storage St',
         city: 'Warehouse City',
         state: 'CA',
         zipCode: '90210',
         country: 'USA'
       },
       capacity: 10000,
       manager: {
         name: 'John Manager',
         email: 'john@warehouse.com'
       },
       isActive: true,
       createdAt: new Date()
     },
     {
       name: 'Secondary Warehouse',
       code: 'WH002',
       address: {
         street: '456 Distribution Ave',
         city: 'Logistics Town',
         state: 'TX',
         zipCode: '75201',
         country: 'USA'
       },
       capacity: 5000,
       manager: {
         name: 'Jane Supervisor',
         email: 'jane@warehouse.com'
       },
       isActive: true,
       createdAt: new Date()
     }
   ]);

   // Insert sample inventory items
   db.inventory.insertMany([
     {
       productId: 'PROD001',
       productName: 'Wireless Headphones',
       description: 'High-quality bluetooth headphones with noise cancellation',
       sku: 'WH-001',
       warehouseId: 'WH001',
       warehouseName: 'Main Warehouse',
       quantity: 150,
       reservedQuantity: 10,
       reorderLevel: 25,
       costPrice: 120.00,
       sellingPrice: 199.99,
       lastRestockDate: new Date('2024-01-15'),
       lastUpdated: new Date(),
       status: 'active',
       tags: ['electronics', 'audio', 'wireless'],
       attributes: {
         color: 'black',
         weight: 0.3
       },
       supplier: {
         id: 'SUP001',
         name: 'AudioTech Supplies',
         contactEmail: 'orders@audiotech.com'
       },
       movements: [
         {
           type: 'in',
           quantity: 150,
           reason: 'Initial stock',
           timestamp: new Date('2024-01-15'),
           userId: 'system'
         }
       ]
     },
     {
       productId: 'PROD002',
       productName: 'Cotton T-Shirt',
       description: 'Comfortable 100% cotton t-shirt in multiple colors',
       sku: 'TS-001',
       warehouseId: 'WH001',
       warehouseName: 'Main Warehouse',
       quantity: 8,
       reservedQuantity: 0,
       reorderLevel: 50,
       costPrice: 15.00,
       sellingPrice: 24.99,
       lastUpdated: new Date(),
       status: 'active',
       tags: ['clothing', 'cotton', 'casual'],
       attributes: {
         color: 'blue',
         size: 'M'
       },
       supplier: {
         id: 'SUP002',
         name: 'Textile Wholesalers',
         contactEmail: 'sales@textileco.com'
       },
       movements: [
         {
           type: 'in',
           quantity: 100,
           reason: 'Initial stock',
           timestamp: new Date('2024-01-10'),
           userId: 'system'
         },
         {
           type: 'out',
           quantity: 92,
           reason: 'Sales',
           timestamp: new Date('2024-01-20'),
           userId: 'sales_system'
         }
       ]
     }
   ]);

   print('Seed data inserted successfully');
   ```

#### Validation Checks:
- MongoDB connection established
- Collections created with proper indexes
- Seed data inserted successfully
- Text search functionality working

### Debugging Lab: MongoDB Issues and Optimization
**Goal:** Identify and resolve common MongoDB problems including performance and data consistency issues.

#### Bug Scenarios:
```javascript
// Bug 1: Missing ObjectId validation
async findById(id: string) {
  // Missing: if (!ObjectId.isValid(id)) throw error
  return collection.findOne({ _id: new ObjectId(id) }); // Can throw
}

// Bug 2: Not using indexes effectively
// Slow query:
collection.find({ productName: /searchTerm/ }); // Full collection scan
// Better:
collection.find({ $text: { $search: searchTerm } }); // Uses text index

// Bug 3: Not handling concurrent updates
// Race condition:
const item = await collection.findOne({ _id: id });
item.quantity += 10;
await collection.updateOne({ _id: id }, { $set: { quantity: item.quantity } });
// Better:
await collection.updateOne({ _id: id }, { $inc: { quantity: 10 } });

// Bug 4: Inefficient aggregation
// N+1 problem in aggregation:
const items = await collection.find({}).toArray();
for (const item of items) {
  const warehouse = await warehouseCollection.findOne({ _id: item.warehouseId });
  // Process...
}
// Better: Use $lookup in aggregation pipeline
```

#### Common Errors & Solutions:
- **Connection string issues:** Check MongoDB URI format
- **Index not being used:** Use explain() to analyze query performance
- **ObjectId casting errors:** Always validate before casting
- **Memory issues with large results:** Use cursor iteration instead of toArray()

## Assessment Quiz Topics
1. What's the difference between embedding and referencing in MongoDB?
2. When should you use compound indexes?
3. How do text indexes work and when should you use them?
4. What's the purpose of the `$expr` operator?
5. How do you handle concurrent updates in MongoDB?
6. What are the benefits of partial indexes?
7. When should you use aggregation pipelines vs simple queries?

## Automated Checks
**Script:** `npm run verify:mongo:ops`
- Connects to MongoDB and verifies collections exist
- Checks that indexes are created and functional
- Validates document schema structure
- Tests CRUD operations and search functionality

## Extensions for Advanced Learners
- **Aggregation pipelines:** Create complex reports with $lookup and $group
- **GridFS:** Implement file storage for large documents
- **Change streams:** Add real-time notifications for inventory changes

---

# Checkpoint Code Review #1 (End of Module 5)

## Review Scope
Comprehensive review of Modules 1-5 implementations covering:

### Code Quality Rubric (100 points total)

#### 1. Functionality (25 points)
- **Excellent (23-25):** All features work correctly, edge cases handled
- **Good (20-22):** Core features work, minor edge cases missing
- **Satisfactory (15-19):** Basic functionality present, some bugs
- **Needs Improvement (0-14):** Major functionality issues

#### 2. Code Style & Organization (20 points)
- **Excellent (18-20):** Consistent style, well-organized, follows conventions
- **Good (16-17):** Mostly consistent, minor style issues
- **Satisfactory (12-15):** Some inconsistencies, adequate organization
- **Needs Improvement (0-11):** Poor style, disorganized code

#### 3. Git Hygiene (15 points)
- **Excellent (14-15):** Clear commit messages, logical commits, proper branching
- **Good (12-13):** Good commits, minor message issues
- **Satisfactory (9-11):** Adequate commits, some unclear messages
- **Needs Improvement (0-8):** Poor commit history, unclear changes

#### 4. Testing & Validation (20 points)
- **Excellent (18-20):** Comprehensive tests, automated checks passing
- **Good (16-17):** Good test coverage, most checks passing
- **Satisfactory (12-15):** Basic tests present, some issues
- **Needs Improvement (0-11):** Minimal or no testing

#### 5. Documentation (10 points)
- **Excellent (9-10):** Clear README, code comments, API documentation
- **Good (8):** Good documentation, minor gaps
- **Satisfactory (6-7):** Basic documentation present
- **Needs Improvement (0-5):** Poor or missing documentation

#### 6. Error Handling (10 points)
- **Excellent (9-10):** Comprehensive error handling, graceful failures
- **Good (8):** Good error handling, minor gaps
- **Satisfactory (6-7):** Basic error handling
- **Needs Improvement (0-5):** Poor error handling

### Review Process

#### 1. Self-Assessment Checklist
Before submitting for review, complete this checklist:

**Environment & Setup:**
- [ ] All tools installed and working (Node.js, Docker, Git, VS Code)
- [ ] Environment verification script passes
- [ ] Docker containers start successfully
- [ ] Database connections work

**Frontend (React with Vite):**
- [ ] Product list renders correctly
- [ ] CSS Modules styles apply properly
- [ ] Components are accessible (ARIA labels, keyboard navigation)
- [ ] Responsive design works on mobile
- [ ] No console errors in browser

**Backend (NestJS):**
- [ ] All CRUD endpoints respond correctly
- [ ] Validation works (400 errors for invalid data)
- [ ] Error handling returns proper status codes
- [ ] CORS configured for frontend integration
- [ ] TypeScript compiles without errors

**PostgreSQL Integration:**
- [ ] Schema created with all tables and indexes
- [ ] Seed data loads correctly
- [ ] CRUD operations work through API
- [ ] Transactions handle errors properly
- [ ] Query performance is acceptable

**MongoDB Integration:**
- [ ] Collections created with proper indexes
- [ ] Document validation works
- [ ] Text search functionality operational
- [ ] Aggregation queries return correct results
- [ ] Connection pooling configured

**Code Quality:**
- [ ] ESLint passes without errors
- [ ] Prettier formatting consistent
- [ ] TypeScript strict mode enabled
- [ ] No hardcoded secrets or passwords
- [ ] Error messages are user-friendly

#### 2. Automated Pre-Review Checks
Run all verification scripts before review:

```bash
# Environment verification
npm run verify:env

# Frontend checks
npm run verify:frontend:render

# Backend API checks
npm run verify:api:routes

# Database checks
npm run verify:pg:schema
npm run verify:mongo:ops

# Code quality
npm run lint
npm run type-check
npm test
```

#### 3. Peer Review Process
Work in pairs to review each other's code:

**Reviewer Checklist:**
- [ ] Clone and run the project locally
- [ ] Verify all automated checks pass
- [ ] Review code for clarity and maintainability
- [ ] Test edge cases and error scenarios
- [ ] Check for security issues (hardcoded secrets, SQL injection)
- [ ] Validate accessibility features work properly

**Review Deliverables:**
1. **Completed rubric** with scores and comments
2. **Code review comments** in GitHub PR
3. **Issues list** with priority levels (critical/major/minor)
4. **Recommendations** for improvements

#### 4. Common Issues to Look For

**Frontend Issues:**
- Missing keys in React lists
- Accessibility violations (missing ARIA labels)
- CSS specificity conflicts
- Unhandled promise rejections
- Missing loading states

**Backend Issues:**
- Missing input validation
- Improper error status codes
- Security vulnerabilities
- Missing or incorrect CORS configuration
- Inefficient database queries

**Database Issues:**
- Missing indexes on frequently queried columns
- N+1 query problems
- Improper transaction handling
- Data type mismatches
- Missing foreign key constraints

**General Issues:**
- Inconsistent naming conventions
- Missing error handling
- Hardcoded configuration values
- Poor commit messages
- Missing documentation

#### 5. Remediation Process
For issues identified in review:

1. **Critical Issues (Security, Data Loss):** Must fix before proceeding
2. **Major Issues (Functionality, Performance):** Fix within 48 hours
3. **Minor Issues (Style, Documentation):** Address by Module 10 review

# Module 6: API Design and Integration
**Week 3 | Postgres + NestJS with validation, pagination**

## Learning Objectives
- Design RESTful APIs with proper validation
- Implement pagination and filtering
- Connect NestJS services to PostgreSQL
- Handle CORS and HTTP headers correctly
- Create comprehensive error handling

## Prerequisites
- NestJS fundamentals (Module 3)
- PostgreSQL knowledge (Module 4)
- Understanding of HTTP protocols

## Hands-On Labs

### Build Lab: Complete Products API with PostgreSQL
**Goal:** Build a production-ready API with validation, pagination, filtering, and proper error handling.

#### Steps:
1. **Enhanced DTOs with validation**
   ```typescript
   // src/products/dto/create-product.dto.ts
   import {
     IsString, IsNumber, IsOptional, Min, Max, MaxLength,
     IsUUID, IsArray, IsEnum, IsUrl
   } from 'class-validator';
   import { Transform } from 'class-transformer';

   enum ProductStatus {
     ACTIVE = 'active',
     INACTIVE = 'inactive',
     DISCONTINUED = 'discontinued'
   }

   export class CreateProductDto {
     @IsString()
     @MaxLength(100, { message: 'Product name must not exceed 100 characters' })
     name: string;

     @IsString()
     @MaxLength(1000, { message: 'Description must not exceed 1000 characters' })
     description: string;

     @IsNumber({ maxDecimalPlaces: 2 }, { message: 'Price must have at most 2 decimal places' })
     @Min(0, { message: 'Price must be positive' })
     @Max(999999.99, { message: 'Price must not exceed $999,999.99' })
     price: number;

     @IsOptional()
     @IsNumber({}, { message: 'Category ID must be a number' })
     categoryId?: number;

     @IsString()
     @MaxLength(50, { message: 'SKU must not exceed 50 characters' })
     sku: string;

     @IsOptional()
     @IsNumber({}, { message: 'Stock quantity must be a number' })
     @Min(0, { message: 'Stock quantity cannot be negative' })
     stockQuantity?: number;

     @IsOptional()
     @IsUrl({}, { message: 'Image URL must be valid' })
     imageUrl?: string;

     @IsOptional()
     @IsEnum(ProductStatus, { message: 'Status must be active, inactive, or discontinued' })
     status?: ProductStatus;

     @IsOptional()
     @IsArray()
     @IsString({ each: true })
     tags?: string[];
   }

   // src/products/dto/query-products.dto.ts
   import { IsOptional, IsNumber, IsString, IsEnum, Min, Max } from 'class-validator';
   import { Transform } from 'class-transformer';

   export class QueryProductsDto {
     @IsOptional()
     @Transform(({ value }) => parseInt(value))
     @IsNumber({}, { message: 'Page must be a number' })
     @Min(1, { message: 'Page must be at least 1' })
     page?: number = 1;

     @IsOptional()
     @Transform(({ value }) => parseInt(value))
     @IsNumber({}, { message: 'Limit must be a number' })
     @Min(1, { message: 'Limit must be at least 1' })
     @Max(100, { message: 'Limit cannot exceed 100' })
     limit?: number = 20;

     @IsOptional()
     @IsString()
     search?: string;

     @IsOptional()
     @Transform(({ value }) => parseInt(value))
     @IsNumber({}, { message: 'Category ID must be a number' })
     categoryId?: number;

     @IsOptional()
     @IsEnum(['active', 'inactive', 'discontinued'])
     status?: string;

     @IsOptional()
     @IsEnum(['name', 'price', 'created_at'], { message: 'Invalid sort field' })
     sortBy?: string = 'created_at';

     @IsOptional()
     @IsEnum(['asc', 'desc'], { message: 'Sort order must be asc or desc' })
     sortOrder?: 'asc' | 'desc' = 'desc';

     @IsOptional()
     @Transform(({ value }) => parseFloat(value))
     @IsNumber({}, { message: 'Min price must be a number' })
     @Min(0)
     minPrice?: number;

     @IsOptional()
     @Transform(({ value }) => parseFloat(value))
     @IsNumber({}, { message: 'Max price must be a number' })
     @Min(0)
     maxPrice?: number;
   }
   ```

2. **Enhanced Products Service with complex queries**
   ```typescript
   // src/products/products.service.ts
   import { Injectable, NotFoundException, ConflictException } from '@nestjs/common';
   import { DatabaseService } from '../database/database.service';
   import { CreateProductDto, UpdateProductDto, QueryProductsDto } from './dto';

   export interface PaginatedResponse<T> {
     data: T[];
     pagination: {
       page: number;
       limit: number;
       total: number;
       totalPages: number;
       hasNext: boolean;
       hasPrev: boolean;
     };
   }

   @Injectable()
   export class ProductsService {
     constructor(private readonly db: DatabaseService) {}

     async create(createProductDto: CreateProductDto): Promise<Product> {
       return this.db.transaction(async (client) => {
         // Check for duplicate SKU
         const existingProduct = await client.query(
           'SELECT id FROM products WHERE sku = $1',
           [createProductDto.sku]
         );

         if (existingProduct.rows.length > 0) {
           throw new ConflictException(`Product with SKU '${createProductDto.sku}' already exists`);
         }

         // Insert product
         const query = `
           INSERT INTO products (
             name, description, price, category_id, sku,
             stock_quantity, image_url, status, tags
           ) VALUES ($1, $2, $3, $4, $5, $6, $7, $8, $9)
           RETURNING *
         `;

         const values = [
           createProductDto.name,
           createProductDto.description,
           createProductDto.price,
           createProductDto.categoryId || null,
           createProductDto.sku,
           createProductDto.stockQuantity || 0,
           createProductDto.imageUrl || null,
           createProductDto.status || 'active',
           createProductDto.tags || []
         ];

         const result = await client.query(query, values);
         return result.rows[0];
       });
     }

     async findAll(query: QueryProductsDto): Promise<PaginatedResponse<Product>> {
       const offset = (query.page - 1) * query.limit;

       // Build WHERE clause dynamically
       const conditions: string[] = ['p.is_active = true'];
       const params: any[] = [];
       let paramCount = 0;

       if (query.search) {
         paramCount++;
         conditions.push(`(p.name ILIKE $${paramCount} OR p.description ILIKE $${paramCount})`);
         params.push(`%${query.search}%`);
       }

       if (query.categoryId) {
         paramCount++;
         conditions.push(`p.category_id = $${paramCount}`);
         params.push(query.categoryId);
       }

       if (query.status) {
         paramCount++;
         conditions.push(`p.status = $${paramCount}`);
         params.push(query.status);
       }

       if (query.minPrice !== undefined) {
         paramCount++;
         conditions.push(`p.price >= $${paramCount}`);
         params.push(query.minPrice);
       }

       if (query.maxPrice !== undefined) {
         paramCount++;
         conditions.push(`p.price <= $${paramCount}`);
         params.push(query.maxPrice);
       }

       const whereClause = conditions.join(' AND ');
       const orderClause = `ORDER BY p.${query.sortBy} ${query.sortOrder.toUpperCase()}`;

       // Get total count
       const countQuery = `
         SELECT COUNT(*)
         FROM products p
         LEFT JOIN categories c ON p.category_id = c.id
         WHERE ${whereClause}
       `;
       const countResult = await this.db.query(countQuery, params);
       const total = parseInt(countResult.rows[0].count);

       // Get paginated data
       const dataQuery = `
         SELECT
           p.*,
           c.name as category_name,
           c.description as category_description
         FROM products p
         LEFT JOIN categories c ON p.category_id = c.id
         WHERE ${whereClause}
         ${orderClause}
         LIMIT $${paramCount + 1} OFFSET $${paramCount + 2}
       `;

       const dataResult = await this.db.query(dataQuery, [...params, query.limit, offset]);

       const totalPages = Math.ceil(total / query.limit);

       return {
         data: dataResult.rows,
         pagination: {
           page: query.page,
           limit: query.limit,
           total,
           totalPages,
           hasNext: query.page < totalPages,
           hasPrev: query.page > 1
         }
       };
     }

     async findOne(id: number): Promise<Product> {
       const query = `
         SELECT
           p.*,
           c.name as category_name,
           json_agg(
             json_build_object(
               'supplier_id', ps.supplier_id,
               'supplier_name', s.name,
               'cost_price', ps.cost_price
             )
           ) FILTER (WHERE ps.supplier_id IS NOT NULL) as suppliers
         FROM products p
         LEFT JOIN categories c ON p.category_id = c.id
         LEFT JOIN product_suppliers ps ON p.id = ps.product_id
         LEFT JOIN suppliers s ON ps.supplier_id = s.id
         WHERE p.id = $1 AND p.is_active = true
         GROUP BY p.id, c.name
       `;

       const result = await this.db.query(query, [id]);

       if (result.rows.length === 0) {
         throw new NotFoundException(`Product with ID ${id} not found`);
       }

       return result.rows[0];
     }

     async update(id: number, updateProductDto: UpdateProductDto): Promise<Product> {
       return this.db.transaction(async (client) => {
         // Check if product exists
         const existsQuery = 'SELECT id FROM products WHERE id = $1 AND is_active = true';
         const existsResult = await client.query(existsQuery, [id]);

         if (existsResult.rows.length === 0) {
           throw new NotFoundException(`Product with ID ${id} not found`);
         }

         // Check for SKU conflict if updating SKU
         if (updateProductDto.sku) {
           const skuQuery = 'SELECT id FROM products WHERE sku = $1 AND id != $2';
           const skuResult = await client.query(skuQuery, [updateProductDto.sku, id]);

           if (skuResult.rows.length > 0) {
             throw new ConflictException(`Product with SKU '${updateProductDto.sku}' already exists`);
           }
         }

         // Build dynamic update query
         const fields = Object.keys(updateProductDto);
         const setClause = fields.map((field, index) => `${field} = $${index + 2}`).join(', ');
         const values = [id, ...Object.values(updateProductDto)];

         const updateQuery = `
           UPDATE products
           SET ${setClause}, updated_at = CURRENT_TIMESTAMP
           WHERE id = $1
           RETURNING *
         `;

         const result = await client.query(updateQuery, values);
         return result.rows[0];
       });
     }

     async remove(id: number): Promise<void> {
       const query = 'UPDATE products SET is_active = false WHERE id = $1 AND is_active = true';
       const result = await this.db.query(query, [id]);

       if (result.rowCount === 0) {
         throw new NotFoundException(`Product with ID ${id} not found`);
       }
     }

     async getProductStats(): Promise<any> {
       const query = `
         SELECT
           COUNT(*) as total_products,
           COUNT(*) FILTER (WHERE status = 'active') as active_products,
           COUNT(*) FILTER (WHERE stock_quantity < 10) as low_stock_products,
           AVG(price) as average_price,
           MAX(price) as max_price,
           MIN(price) as min_price
         FROM products
         WHERE is_active = true
       `;

       const result = await this.db.query(query);
       return result.rows[0];
     }
   }
   ```

3. **Enhanced Controller with proper error handling**
   ```typescript
   // src/products/products.controller.ts
   import {
     Controller, Get, Post, Body, Patch, Param, Delete, Query,
     ParseIntPipe, HttpCode, HttpStatus, UseInterceptors,
     ValidationPipe, UsePipes
   } from '@nestjs/common';
   import { ProductsService } from './products.service';
   import { CreateProductDto, UpdateProductDto, QueryProductsDto } from './dto';

   @Controller('api/products')
   export class ProductsController {
     constructor(private readonly productsService: ProductsService) {}

     @Post()
     @HttpCode(HttpStatus.CREATED)
     @UsePipes(new ValidationPipe({ transform: true }))
     async create(@Body() createProductDto: CreateProductDto) {
       return this.productsService.create(createProductDto);
     }

     @Get()
     @UsePipes(new ValidationPipe({ transform: true }))
     async findAll(@Query() query: QueryProductsDto) {
       return this.productsService.findAll(query);
     }

     @Get('stats')
     async getStats() {
       return this.productsService.getProductStats();
     }

     @Get(':id')
     async findOne(@Param('id', ParseIntPipe) id: number) {
       return this.productsService.findOne(id);
     }

     @Patch(':id')
     @UsePipes(new ValidationPipe({ transform: true }))
     async update(
       @Param('id', ParseIntPipe) id: number,
       @Body() updateProductDto: UpdateProductDto
     ) {
       return this.productsService.update(id, updateProductDto);
     }

     @Delete(':id')
     @HttpCode(HttpStatus.NO_CONTENT)
     async remove(@Param('id', ParseIntPipe) id: number) {
       await this.productsService.remove(id);
     }
   }
   ```

#### Validation Checks:
- All endpoints return correct HTTP status codes
- Validation errors provide helpful messages
- Pagination works correctly with proper metadata
- Search and filtering functions properly
- CORS configured for frontend integration

### Debugging Lab: API Integration Issues
**Goal:** Identify and fix common problems in API design and integration.

#### Bug Scenarios:
```typescript
// Bug 1: Missing CORS configuration
// Error: CORS policy blocks frontend requests
// Fix: Configure CORS in main.ts

// Bug 2: Incorrect status codes
@Post()
@HttpCode(HttpStatus.OK) // Should be CREATED (201)
create() {}

// Bug 3: Missing validation
@Post()
create(@Body() data: any) { // Should use DTO with validation
  return this.service.create(data);
}

// Bug 4: Poor error messages
throw new Error('Bad input'); // Not helpful
// Better:
throw new BadRequestException('Product name must not exceed 100 characters');

// Bug 5: No pagination limits
@Get()
findAll(@Query('limit') limit: string) {
  const limitNum = parseInt(limit) || 1000000; // Could crash server
  // Should validate and cap limit
}
```

#### Common Errors & Solutions:
- **CORS preflight failures:** Configure proper origins and headers
- **Validation pipe not working:** Enable globally in main.ts
- **JSON parse errors:** Add proper Content-Type headers
- **Timeout errors:** Implement request timeout middleware

## Assessment Quiz Topics
1. What HTTP status codes should you return for different CRUD operations?
2. How do DTOs differ from interfaces in NestJS?
3. What's the purpose of validation pipes and how do they work?
4. How should you implement pagination in a REST API?
5. What CORS headers are needed for credentials?
6. How do you handle database constraint violations?
7. What's the difference between @Query() and @Param() decorators?

## Automated Checks
**Script:** `npm run verify:api:pg`
- Tests all CRUD endpoints with various payloads
- Validates response formats and error messages
- Checks pagination metadata accuracy
- Verifies database integration works correctly

## Extensions for Advanced Learners
- **Rate limiting:** Implement API rate limiting middleware
- **Caching:** Add Redis caching for frequently accessed data
- **API versioning:** Implement versioning strategy for endpoints

---

# Module 7: Authentication & Security
**Week 4 | JWT + HTTP-only cookies + bcrypt**

## Learning Objectives
- Implement secure user authentication system
- Use JWT tokens with HTTP-only cookies
- Hash passwords securely with bcrypt
- Protect routes with authentication middleware
- Implement refresh token flow

## Prerequisites
- NestJS and PostgreSQL knowledge
- Understanding of HTTP cookies and headers
- Basic security concepts

## Hands-On Labs

### Build Lab: Complete Authentication System
**Goal:** Build a production-ready authentication system with registration, login, logout, and protected routes.

#### Steps:
1. **Set up authentication dependencies**
   ```bash
   cd backend
   npm install @nestjs/jwt @nestjs/passport passport passport-jwt bcrypt
   npm install --save-dev @types/bcrypt @types/passport-jwt
   ```

2. **Create User entity and database schema**
   ```sql
   -- Add to db/postgres/001_init.sql
   CREATE TABLE users (
     id SERIAL PRIMARY KEY,
     email VARCHAR(255) NOT NULL UNIQUE,
     password_hash VARCHAR(255) NOT NULL,
     first_name VARCHAR(100) NOT NULL,
     last_name VARCHAR(100) NOT NULL,
     role VARCHAR(50) DEFAULT 'user',
     email_verified BOOLEAN DEFAULT false,
     last_login TIMESTAMP,
     created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
     updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );

   CREATE TABLE refresh_tokens (
     id SERIAL PRIMARY KEY,
     user_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
     token_hash VARCHAR(255) NOT NULL,
     expires_at TIMESTAMP NOT NULL,
     created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
   );

   CREATE INDEX idx_users_email ON users(email);
   CREATE INDEX idx_refresh_tokens_user_id ON refresh_tokens(user_id);
   CREATE INDEX idx_refresh_tokens_expires ON refresh_tokens(expires_at);

   -- Apply trigger to users table
   CREATE TRIGGER update_users_updated_at
     BEFORE UPDATE ON users
     FOR EACH ROW
     EXECUTE FUNCTION update_updated_at_column();
   ```

3. **Create authentication DTOs**
   ```typescript
   // src/auth/dto/register.dto.ts
   import { IsEmail, IsString, MinLength, MaxLength, Matches } from 'class-validator';

   export class RegisterDto {
     @IsEmail({}, { message: 'Please provide a valid email address' })
     email: string;

     @IsString()
     @MinLength(8, { message: 'Password must be at least 8 characters long' })
     @MaxLength(128, { message: 'Password must not exceed 128 characters' })
     @Matches(
       /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]/,
       { message: 'Password must contain uppercase, lowercase, number, and special character' }
     )
     password: string;

     @IsString()
     @MaxLength(100, { message: 'First name must not exceed 100 characters' })
     firstName: string;

     @IsString()
     @MaxLength(100, { message: 'Last name must not exceed 100 characters' })
     lastName: string;
   }

   // src/auth/dto/login.dto.ts
   import { IsEmail, IsString } from 'class-validator';

   export class LoginDto {
     @IsEmail({}, { message: 'Please provide a valid email address' })
     email: string;

     @IsString()
     password: string;
   }
   ```

4. **Implement User Service**
   ```typescript
   // src/users/users.service.ts
   import { Injectable, ConflictException, NotFoundException } from '@nestjs/common';
   import { DatabaseService } from '../database/database.service';
   import * as bcrypt from 'bcrypt';

   export interface User {
     id: number;
     email: string;
     firstName: string;
     lastName: string;
     role: string;
     emailVerified: boolean;
     lastLogin?: Date;
     createdAt: Date;
     updatedAt: Date;
   }

   export interface CreateUserData {
     email: string;
     password: string;
     firstName: string;
     lastName: string;
   }

   @Injectable()
   export class UsersService {
     constructor(private readonly db: DatabaseService) {}

     async create(userData: CreateUserData): Promise<User> {
       return this.db.transaction(async (client) => {
         // Check if user already exists
         const existingUser = await client.query(
           'SELECT id FROM users WHERE email = $1',
           [userData.email]
         );

         if (existingUser.rows.length > 0) {
           throw new ConflictException('User with this email already exists');
         }

         // Hash password
         const saltRounds = 12;
         const passwordHash = await bcrypt.hash(userData.password, saltRounds);

         // Create user
         const query = `
           INSERT INTO users (email, password_hash, first_name, last_name)
           VALUES ($1, $2, $3, $4)
           RETURNING id, email, first_name, last_name, role, email_verified, created_at, updated_at
         `;

         const result = await client.query(query, [
           userData.email,
           passwordHash,
           userData.firstName,
           userData.lastName
         ]);

         return this.formatUser(result.rows[0]);
       });
     }

     async findByEmail(email: string): Promise<User & { passwordHash: string } | null> {
       const query = 'SELECT * FROM users WHERE email = $1';
       const result = await this.db.query(query, [email]);

       if (result.rows.length === 0) {
         return null;
       }

       return {
         ...this.formatUser(result.rows[0]),
         passwordHash: result.rows[0].password_hash
       };
     }

     async findById(id: number): Promise<User | null> {
       const query = `
         SELECT id, email, first_name, last_name, role, email_verified, last_login, created_at, updated_at
         FROM users WHERE id = $1
       `;
       const result = await this.db.query(query, [id]);

       if (result.rows.length === 0) {
         return null;
       }

       return this.formatUser(result.rows[0]);
     }

     async updateLastLogin(id: number): Promise<void> {
       const query = 'UPDATE users SET last_login = CURRENT_TIMESTAMP WHERE id = $1';
       await this.db.query(query, [id]);
     }

     async validatePassword(plainPassword: string, hashedPassword: string): Promise<boolean> {
       return bcrypt.compare(plainPassword, hashedPassword);
     }

     private formatUser(dbUser: any): User {
       return {
         id: dbUser.id,
         email: dbUser.email,
         firstName: dbUser.first_name,
         lastName: dbUser.last_name,
         role: dbUser.role,
         emailVerified: dbUser.email_verified,
         lastLogin: dbUser.last_login,
         createdAt: dbUser.created_at,
         updatedAt: dbUser.updated_at
       };
     }
   }
   ```

5. **Implement Auth Service with JWT and refresh tokens**
   ```typescript
   // src/auth/auth.service.ts
   import { Injectable, UnauthorizedException, BadRequestException } from '@nestjs/common';
   import { JwtService } from '@nestjs/jwt';
   import { UsersService } from '../users/users.service';
   import { DatabaseService } from '../database/database.service';
   import { RegisterDto, LoginDto } from './dto';
   import * as crypto from 'crypto';

   export interface AuthTokens {
     accessToken: string;
     refreshToken: string;
   }

   export interface JwtPayload {
     sub: number;
     email: string;
     role: string;
   }

   @Injectable()
   export class AuthService {
     constructor(
       private readonly usersService: UsersService,
       private readonly jwtService: JwtService,
       private readonly db: DatabaseService
     ) {}

     async register(registerDto: RegisterDto): Promise<{ user: any; tokens: AuthTokens }> {
       const user = await this.usersService.create({
         email: registerDto.email,
         password: registerDto.password,
         firstName: registerDto.firstName,
         lastName: registerDto.lastName
       });

       const tokens = await this.generateTokens(user);
       await this.saveRefreshToken(user.id, tokens.refreshToken);

       return { user, tokens };
     }

     async login(loginDto: LoginDto): Promise<{ user: any; tokens: AuthTokens }> {
       const user = await this.usersService.findByEmail(loginDto.email);

       if (!user) {
         throw new UnauthorizedException('Invalid credentials');
       }

       const isPasswordValid = await this.usersService.validatePassword(
         loginDto.password,
         user.passwordHash
       );

       if (!isPasswordValid) {
         throw new UnauthorizedException('Invalid credentials');
       }

       await this.usersService.updateLastLogin(user.id);
       const tokens = await this.generateTokens(user);
       await this.saveRefreshToken(user.id, tokens.refreshToken);

       // Remove password hash from response
       const { passwordHash, ...userWithoutPassword } = user;
       return { user: userWithoutPassword, tokens };
     }

     async refreshTokens(refreshToken: string): Promise<AuthTokens> {
       const tokenHash = crypto.createHash('sha256').update(refreshToken).digest('hex');

       const query = `
         SELECT rt.user_id, u.email, u.role
         FROM refresh_tokens rt
         JOIN users u ON rt.user_id = u.id
         WHERE rt.token_hash = $1 AND rt.expires_at > NOW()
       `;

       const result = await this.db.query(query, [tokenHash]);

       if (result.rows.length === 0) {
         throw new UnauthorizedException('Invalid or expired refresh token');
       }

       const { user_id, email, role } = result.rows[0];

       // Generate new tokens
       const newTokens = await this.generateTokens({ id: user_id, email, role });

       // Save new refresh token and remove old one
       await this.db.transaction(async (client) => {
         await client.query('DELETE FROM refresh_tokens WHERE token_hash = $1', [tokenHash]);
         await this.saveRefreshToken(user_id, newTokens.refreshToken);
       });

       return newTokens;
     }

     async logout(refreshToken: string): Promise<void> {
       const tokenHash = crypto.createHash('sha256').update(refreshToken).digest('hex');
       await this.db.query('DELETE FROM refresh_tokens WHERE token_hash = $1', [tokenHash]);
     }

     async logoutAll(userId: number): Promise<void> {
       await this.db.query('DELETE FROM refresh_tokens WHERE user_id = $1', [userId]);
     }

     private async generateTokens(user: { id: number; email: string; role: string }): Promise<AuthTokens> {
       const payload: JwtPayload = {
         sub: user.id,
         email: user.email,
         role: user.role
       };

       const [accessToken, refreshToken] = await Promise.all([
         this.jwtService.signAsync(payload, { expiresIn: '15m' }),
         this.jwtService.signAsync(payload, { expiresIn: '7d' })
       ]);

       return { accessToken, refreshToken };
     }

     private async saveRefreshToken(userId: number, refreshToken: string): Promise<void> {
       const tokenHash = crypto.createHash('sha256').update(refreshToken).digest('hex');
       const expiresAt = new Date();
       expiresAt.setDate(expiresAt.getDate() + 7); // 7 days

       const query = `
         INSERT INTO refresh_tokens (user_id, token_hash, expires_at)
         VALUES ($1, $2, $3)
       `;

       await this.db.query(query, [userId, tokenHash, expiresAt]);
     }

     async validateUser(payload: JwtPayload): Promise<any> {
       const user = await this.usersService.findById(payload.sub);
       if (!user) {
         throw new UnauthorizedException('User not found');
       }
       return user;
     }
   }
   ```

6. **Create JWT strategy and guards**
   ```typescript
   // src/auth/strategies/jwt.strategy.ts
   import { Injectable, UnauthorizedException } from '@nestjs/common';
   import { PassportStrategy } from '@nestjs/passport';
   import { ExtractJwt, Strategy } from 'passport-jwt';
   import { AuthService } from '../auth.service';
   import { JwtPayload } from '../auth.service';
   import { Request } from 'express';

   @Injectable()
   export class JwtStrategy extends PassportStrategy(Strategy) {
     constructor(private readonly authService: AuthService) {
       super({
         jwtFromRequest: ExtractJwt.fromExtractors([
           (request: Request) => {
             return request?.cookies?.['access_token'];
           }
         ]),
         ignoreExpiration: false,
         secretOrKey: process.env.JWT_SECRET || 'your-secret-key',
       });
     }

     async validate(payload: JwtPayload) {
       const user = await this.authService.validateUser(payload);
       if (!user) {
         throw new UnauthorizedException();
       }
       return user;
     }
   }

   // src/auth/guards/jwt-auth.guard.ts
   import { Injectable, ExecutionContext } from '@nestjs/common';
   import { AuthGuard } from '@nestjs/passport';
   import { Reflector } from '@nestjs/core';

   @Injectable()
   export class JwtAuthGuard extends AuthGuard('jwt') {
     constructor(private reflector: Reflector) {
       super();
     }

     canActivate(context: ExecutionContext) {
       const isPublic = this.reflector.getAllAndOverride<boolean>('isPublic', [
         context.getHandler(),
         context.getClass(),
       ]);

       if (isPublic) {
         return true;
       }

       return super.canActivate(context);
     }
   }
   ```

7. **Implement Auth Controller with secure cookies**
   ```typescript
   // src/auth/auth.controller.ts
   import {
     Controller, Post, Body, Res, Req, HttpCode, HttpStatus,
     UseGuards, Get, UnauthorizedException
   } from '@nestjs/common';
   import { Response, Request } from 'express';
   import { AuthService } from './auth.service';
   import { RegisterDto, LoginDto } from './dto';
   import { JwtAuthGuard } from './guards/jwt-auth.guard';
   import { User } from './decorators/user.decorator';

   @Controller('api/auth')
   export class AuthController {
     constructor(private readonly authService: AuthService) {}

     @Post('register')
     @HttpCode(HttpStatus.CREATED)
     async register(
       @Body() registerDto: RegisterDto,
       @Res({ passthrough: true }) response: Response
     ) {
       const { user, tokens } = await this.authService.register(registerDto);

       this.setTokenCookies(response, tokens);

       return {
         message: 'Registration successful',
         user
       };
     }

     @Post('login')
     @HttpCode(HttpStatus.OK)
     async login(
       @Body() loginDto: LoginDto,
       @Res({ passthrough: true }) response: Response
     ) {
       const { user, tokens } = await this.authService.login(loginDto);

       this.setTokenCookies(response, tokens);

       return {
         message: 'Login successful',
         user
       };
     }

     @Post('refresh')
     @HttpCode(HttpStatus.OK)
     async refresh(
       @Req() request: Request,
       @Res({ passthrough: true }) response: Response
     ) {
       const refreshToken = request.cookies['refresh_token'];

       if (!refreshToken) {
         throw new UnauthorizedException('Refresh token not found');
       }

       const tokens = await this.authService.refreshTokens(refreshToken);
       this.setTokenCookies(response, tokens);

       return { message: 'Tokens refreshed successfully' };
     }

     @Post('logout')
     @HttpCode(HttpStatus.OK)
     @UseGuards(JwtAuthGuard)
     async logout(
       @Req() request: Request,
       @Res({ passthrough: true }) response: Response
     ) {
       const refreshToken = request.cookies['refresh_token'];

       if (refreshToken) {
         await this.authService.logout(refreshToken);
       }

       this.clearTokenCookies(response);

       return { message: 'Logout successful' };
     }

     @Post('logout-all')
     @HttpCode(HttpStatus.OK)
     @UseGuards(JwtAuthGuard)
     async logoutAll(
       @User() user: any,
       @Res({ passthrough: true }) response: Response
     ) {
       await this.authService.logoutAll(user.id);
       this.clearTokenCookies(response);

       return { message: 'Logged out from all devices' };
     }

     @Get('me')
     @UseGuards(JwtAuthGuard)
     async getProfile(@User() user: any) {
       return { user };
     }

     private setTokenCookies(response: Response, tokens: { accessToken: string; refreshToken: string }) {
       const cookieOptions = {
         httpOnly: true,
         secure: process.env.NODE_ENV === 'production',
         sameSite: 'lax' as const,
         path: '/'
       };

       response.cookie('access_token', tokens.accessToken, {
         ...cookieOptions,
         maxAge: 15 * 60 * 1000 // 15 minutes
       });

       response.cookie('refresh_token', tokens.refreshToken, {
         ...cookieOptions,
         maxAge: 7 * 24 * 60 * 60 * 1000 // 7 days
       });
     }

     private clearTokenCookies(response: Response) {
       const cookieOptions = {
         httpOnly: true,
         secure: process.env.NODE_ENV === 'production',
         sameSite: 'lax' as const,
         path: '/'
       };

       response.clearCookie('access_token', cookieOptions);
       response.clearCookie('refresh_token', cookieOptions);
     }
   }
   ```

#### Validation Checks:
- User registration creates secure password hashes
- Login validates credentials and returns secure cookies
- JWT tokens are properly signed and validated
- Refresh token flow works correctly
- Logout clears all authentication state

### Debugging Lab: Authentication Flow Issues
**Goal:** Identify and fix common authentication problems including token handling and security vulnerabilities.

#### Bug Scenarios:
```typescript
// Bug 1: Storing JWT in localStorage (XSS vulnerable)
// Wrong:
localStorage.setItem('token', accessToken);
// Right: Use HTTP-only cookies

// Bug 2: Weak password requirements
@MinLength(6) // Too weak
password: string;
// Better: Enforce strong password policy

// Bug 3: Not handling token expiry
// Wrong:
const user = jwt.verify(token, secret); // Throws on expiry
// Better: Handle expiry gracefully and refresh

// Bug 4: Insecure cookie settings
response.cookie('token', jwt, {
  httpOnly: false, // XSS vulnerable
  secure: false    // Not HTTPS only
});

// Bug 5: Clock skew issues
const payload = { exp: Date.now() / 1000 + 3600 }; // Wrong precision
// Better: Use proper JWT library methods
```

#### Common Errors & Solutions:
- **Token not found in cookies:** Check cookie domain and path settings
- **Clock skew errors:** Synchronize server time or add leeway
- **CSRF attacks:** Implement CSRF protection for state-changing operations
- **Password validation bypass:** Ensure validation runs before hashing

## Assessment Quiz Topics
1. What are the security benefits of HTTP-only cookies vs localStorage?
2. How does bcrypt protect against rainbow table attacks?
3. What's the purpose of refresh tokens in JWT authentication?
4. How do you prevent timing attacks in password validation?
5. What JWT claims should you validate and why?
6. How do CSRF attacks work and how can you prevent them?
7. What are the security implications of different cookie SameSite values?

## Automated Checks
**Script:** `npm run verify:auth`
- Tests registration with various password combinations
- Validates login/logout flow with cookie handling
- Checks refresh token functionality
- Verifies protected route access control
- Tests for common security vulnerabilities

## Extensions for Advanced Learners
- **Password reset:** Implement secure password reset via signed URLs
- **Multi-factor authentication:** Add TOTP-based 2FA
- **Rate limiting:** Implement login attempt rate limiting

---

*[Continue with remaining modules 8-16...]*
