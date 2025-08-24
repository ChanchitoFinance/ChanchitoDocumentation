```sql
-- USERS
CREATE TABLE users (
    id TEXT PRIMARY KEY,
    username VARCHAR(50) NOT NULL,
    first_name VARCHAR(100) NOT NULL,
    last_name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP NOT NULL,
    updated_at TIMESTAMP
);

-- CATEGORIES (Main Category)
CREATE TABLE categories (
    id TEXT PRIMARY KEY,
    owner_type VARCHAR(10) NOT NULL CHECK (owner_type IN ('system', 'user')),
    owner_id TEXT, -- nullable if system-owned
    name VARCHAR(100) NOT NULL,
    icon VARCHAR(50),
    created_at TIMESTAMP NOT NULL,
    updated_at TIMESTAMP
);

-- SUBCATEGORIES
CREATE TABLE subcategories (
    id TEXT PRIMARY KEY,
    category_id TEXT NOT NULL REFERENCES categories(id) ON DELETE CASCADE,
    name VARCHAR(100) NOT NULL,
    icon VARCHAR(50),
    created_at TIMESTAMP NOT NULL,
    updated_at TIMESTAMP
);

-- PRODUCTS
CREATE TABLE products (
    id TEXT PRIMARY KEY,
    owner_type VARCHAR(10) NOT NULL CHECK (owner_type IN ('system', 'user')),
    owner_id TEXT,
    category_id TEXT REFERENCES categories(id),
    subcategory_id TEXT REFERENCES subcategories(id),
    name VARCHAR(200) NOT NULL,
    description TEXT,
    is_active BOOLEAN NOT NULL,
    created_at TIMESTAMP NOT NULL,
    updated_at TIMESTAMP
);

-- PRODUCT PRICES
CREATE TABLE product_prices (
    id TEXT PRIMARY KEY,
    product_id TEXT NOT NULL REFERENCES products(id) ON DELETE CASCADE,
    price DECIMAL(15, 4) NOT NULL,
    currency VARCHAR(3) NOT NULL, -- e.g., 'USD'
    location VARCHAR(200),
    source VARCHAR(100),
    is_active BOOLEAN NOT NULL,
    created_at TIMESTAMP NOT NULL,
    updated_at TIMESTAMP
);

-- TRANSACTIONS
CREATE TABLE transactions (
    id TEXT PRIMARY KEY,
    user_id TEXT NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    currency VARCHAR(3) NOT NULL,
    transaction_date TIMESTAMP NOT NULL,
    notes TEXT,
    created_at TIMESTAMP NOT NULL,
    updated_at TIMESTAMP
);


CREATE TABLE transaction_items (
    id TEXT PRIMARY KEY,
    transaction_id TEXT NOT NULL REFERENCES transactions(id) ON DELETE CASCADE,
    product_id TEXT NOT NULL REFERENCES products(id),
    quantity DECIMAL(10, 3) NOT NULL,
    unit_price DECIMAL(15, 4) NOT NULL,
    total_amount DECIMAL(15, 4) GENERATED ALWAYS AS (quantity * unit_price) STORED,
    created_at TIMESTAMP NOT NULL,
    updated_at TIMESTAMP
);
```