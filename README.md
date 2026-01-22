# Creating Express.js with TypeScript

## Initialize Project

Run `npm init -y` - this initializes a new Node.js project

---

## Install Dependencies

### Production Dependencies
```bash
npm i express mongoose
```

### Dev Dependencies
```bash
npm install -D typescript @types/node @types/express ts-node nodemon
```

---

## Initialize TypeScript Configuration

### Create tsconfig.json
```bash
npx tsc --init
```

Get `tsconfig.json` on `https://tsconfig.guide/`

**NOTE:** use `nodenext` on `module: ""`

**NOTE:** add `"allowImportingTsExtensions": true` on `tsconfig.json`

---

## Update package.json Scripts

Add in `package.json` (scripts section):

```json
"dev": "nodemon --exec ts-node --esm src/index.ts",
"build": "tsc",
"start": "node dist/index.js",
"type-check": "tsc --noEmit"
```

---

## Create Project Structure

### Create src folder and entry point

```bash
mkdir src
cd src
touch index.ts
```

### Create Express Server in index.ts

```typescript
import express from 'express';
import type { Request, Response, Express } from 'express';
import dotenv from 'dotenv'

dotenv.config()

const app: Express = express();
const PORT = process.env.PORT || 3000;

// Middleware
app.use(express.json());
app.use(express.urlencoded({ extended: true }));

// Routes
app.get('/', (req: Request, res: Response) => {
  res.json({ message: 'Welcome to Express with TypeScript!' });
});

// Start server
app.listen(PORT, () => {
  console.log(`Server is running at http://localhost:${PORT}`);
});

export default app;
```

---

## Create Directories

```bash
mkdir controllers, services, routes, dbConfig
```

---

## Database Configuration

Create db connection in `db.config.ts` or `db.service.ts`

## You can refer on mongoose quickstart docs for this one

```typescript
import dotenv from 'dotenv'
import mongoose from 'mongoose'

dotenv.config()

const connectDB = async () => {
  try {
    const mongoURI = process.env.MONGOURI || 'mongodb://localhost:27017/testDB'
    await mongoose.connect(mongoURI)
    console.log('DB Connection Successful')
  } catch (error) {
    console.log('DB Connection Error: ', error)
  }
}

export default connectDB
```

---

## Create Schemas and Models

Create your schemas and models (refer to the Mongoose quickstart docs)

### VERY IMPORTANT! Import all models on entry file (`index.ts`)

You know how to create controllers and services on NestJS bro. Do it the same on Express.

---

## Creating Routes

### Create routes/product.route.ts

```typescript
import { Router } from "express";
import productsController from "../controllers/products.controller.ts";

const router = Router();

router.post('/', productsController.create)
router.get('/', productsController.findAll)
router.get('/activeProducts', productsController.findActiveProduct)
router.get('/inActiveProducts', productsController.findInActiveProduct)
router.patch('/softDelete', productsController.softDelete)
router.delete('/', productsController.hardDelete)

export default router
```

### Register Routes in index.ts

```typescript
app.use('/products', productRoutes)
```
