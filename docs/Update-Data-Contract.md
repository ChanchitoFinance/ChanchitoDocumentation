Copy the below code block and paste it in the [Swagger Editor](https://editor.swagger.io/) to see the Data Contract in a rich view.

```yaml
openapi: 3.0.3
info:
  title: Chanchito API
  version: "1.0.0"
  description: >
    Mobile-first personal finance MVP for quick expense logging and price tracking.
    Notes on numeric precision:
    - Monetary and quantity values are modeled as strings to avoid float drift.
    - Scales: quantity up to 3 decimals, money up to 4 decimals.

servers:
  - url: https://api.chanchito.example.com/v1

# All routes require auth unless they explicitly override with `security: []`
security:
  - BearerAuth: []

tags:
  - name: Auth
  - name: Users
  - name: Categories
  - name: Products
  - name: Expenses

paths:

  /auth/register:
    post:
      tags: [Auth]
      summary: Register a new user
      security: []   # public
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/RegisterRequest"
      responses:
        "201":
          description: Registered
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/AuthResponse"
        "409": { $ref: "#/components/responses/Conflict" }
        "422": { $ref: "#/components/responses/UnprocessableEntity" }

  /auth/login:
    post:
      tags: [Auth]
      summary: Login with email and password
      security: []   # public
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/LoginRequest"
      responses:
        "200":
          description: Authenticated
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/AuthResponse"
        "401": { $ref: "#/components/responses/Unauthorized" }

  /auth/social-login/google:
    post:
      tags: [Auth]
      summary: Login with Google service provider
      description: >
        Accepts a Google ID token in the `Authorization` header (Bearer token).
        The backend verifies the token with Google and returns the app's own tokens.
      security: []   # public – client sends Google ID token in Authorization header
      responses:
        "200":
          description: Authenticated
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/AuthResponse"
        "401":
          description: Unauthorized
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Error"

  /me:
    get:
      tags: [Users]
      summary: Get current user profile
      responses:
        "200":
          description: Current user
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/User"
        "401": { $ref: "#/components/responses/Unauthorized" }

  /categories:
    get:
      tags: [Categories]
      summary: List categories as a nested tree (global or user-owned)
      parameters:
        - $ref: "#/components/parameters/OwnerType"
        - $ref: "#/components/parameters/OwnerId"
      responses:
        "200":
          description: Nested categories
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/NestedCategoryList"
    post:
      tags: [Categories]
      summary: Create category (optionally as a child of another)
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/CreateCategoryRequest"
      responses:
        "201":
          description: Created
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Category"
        "409": { $ref: "#/components/responses/Conflict" }
        "422": { $ref: "#/components/responses/UnprocessableEntity" }

  /categories/{id}:
    get:
      tags: [Categories]
      summary: Get category by id (with children)
      parameters:
        - $ref: "#/components/parameters/Id"
      responses:
        "200":
          description: Category (nested)
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/NestedCategory"
        "404": { $ref: "#/components/responses/NotFound" }
    patch:
      tags: [Categories]
      summary: Update category (partial)
      parameters:
        - $ref: "#/components/parameters/Id"
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/UpdateCategoryRequest"
      responses:
        "200":
          description: Updated
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Category"
        "404": { $ref: "#/components/responses/NotFound" }

  /products:
    get:
      tags: [Products]
      summary: List products with filters
      parameters:
        - $ref: "#/components/parameters/OwnerType"
        - $ref: "#/components/parameters/OwnerId"
        - name: categoryId
          in: query
          schema: { type: string, format: uuid }
        - name: q
          in: query
          description: Case-insensitive name search
          schema: { type: string }
        - $ref: "#/components/parameters/Page"
        - $ref: "#/components/parameters/PageSize"
      responses:
        "200":
          description: Paged products
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/PagedProduct"
    post:
      tags: [Products]
      summary: Create product
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/CreateProductRequest"
      responses:
        "201":
          description: Created
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Product"
        "409": { $ref: "#/components/responses/Conflict" }
        "422": { $ref: "#/components/responses/UnprocessableEntity" }

  /products/{id}:
    get:
      tags: [Products]
      summary: Get product (includes recent price samples)
      parameters:
        - $ref: "#/components/parameters/Id"
        - name: priceLimit
          in: query
          description: Max number of recent price records to include
          schema: { type: integer, minimum: 0, maximum: 50, default: 5 }
      responses:
        "200":
          description: Product with embedded prices
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/ProductWithPrices"
        "404": { $ref: "#/components/responses/NotFound" }
    patch:
      tags: [Products]
      summary: Update product
      parameters:
        - $ref: "#/components/parameters/Id"
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/UpdateProductRequest"
      responses:
        "200":
          description: Updated
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Product"
        "404": { $ref: "#/components/responses/NotFound" }

  /expenses:
    get:
      tags: [Expenses]
      summary: List expenses with filters
      parameters:
        - name: dateFrom
          in: query
          description: Inclusive ISO date or date-time
          schema: { type: string, format: date-time }
        - name: dateTo
          in: query
          description: Inclusive ISO date or date-time
          schema: { type: string, format: date-time }
        - name: categoryId
          in: query
          schema: { type: string, format: uuid }
        - name: productId
          in: query
          schema: { type: string, format: uuid }
        - name: currency
          in: query
          schema: { $ref: "#/components/schemas/CurrencyCode" }
        - $ref: "#/components/parameters/Page"
        - $ref: "#/components/parameters/PageSize"
      responses:
        "200":
          description: Paged expenses
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/PagedExpense"
    post:
      tags: [Expenses]
      summary: Create expense with line items
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/CreateExpenseRequest"
      responses:
        "201":
          description: Created
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Expense"
        "422": { $ref: "#/components/responses/UnprocessableEntity" }

  /expenses/{id}:
    get:
      tags: [Expenses]
      summary: Get expense by id
      parameters:
        - $ref: "#/components/parameters/Id"
      responses:
        "200":
          description: Expense
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Expense"
        "404": { $ref: "#/components/responses/NotFound" }
    patch:
      tags: [Expenses]
      summary: Update expense (partial)
      parameters:
        - $ref: "#/components/parameters/Id"
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: "#/components/schemas/UpdateExpenseRequest"
      responses:
        "200":
          description: Updated
          content:
            application/json:
              schema:
                $ref: "#/components/schemas/Expense"
        "404": { $ref: "#/components/responses/NotFound" }

components:

  securitySchemes:
    BearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT

  parameters:
    Id:
      name: id
      in: path
      required: true
      schema: { type: string, format: uuid }
    Page:
      name: page
      in: query
      schema: { type: integer, minimum: 1, default: 1 }
    PageSize:
      name: pageSize
      in: query
      schema: { type: integer, minimum: 1, maximum: 100, default: 20 }
    OwnerType:
      name: ownerType
      in: query
      description: system (global) or user
      schema:
        type: string
        enum: [system, user]
    OwnerId:
      name: ownerId
      in: query
      description: Required when ownerType=user (UUID of owner user)
      schema:
        type: string
        format: uuid

  responses:
    Unauthorized:
      description: Unauthorized
      content:
        application/json: { schema: { $ref: "#/components/schemas/Error" } }
    NotFound:
      description: Not found
      content:
        application/json: { schema: { $ref: "#/components/schemas/Error" } }
    Conflict:
      description: Conflict
      content:
        application/json: { schema: { $ref: "#/components/schemas/Error" } }
    UnprocessableEntity:
      description: Validation error
      content:
        application/json: { schema: { $ref: "#/components/schemas/ValidationError" } }

  schemas:

    # --- Common types ---
    CurrencyCode:
      type: string
      description: ISO 4217 currency code (e.g., USD, BOB, EUR)
      minLength: 3
      maxLength: 3
      example: BOB

    Money4:
      type: string
      description: Decimal string with up to 4 fractional digits (e.g., 123.4567)
      pattern: "^-?\\d{1,13}(\\.\\d{1,4})?$"
      example: "19.9900"

    Quantity3:
      type: string
      description: Decimal string with up to 3 fractional digits
      pattern: "^-?\\d{1,13}(\\.\\d{1,3})?$"
      example: "1.250"

    PagedMeta:
      type: object
      properties:
        page: { type: integer, minimum: 1 }
        pageSize: { type: integer, minimum: 1 }
        totalItems: { type: integer, minimum: 0 }
        totalPages: { type: integer, minimum: 0 }
      required: [page, pageSize, totalItems, totalPages]

    Error:
      type: object
      properties:
        code: { type: string }
        message: { type: string }
      required: [code, message]

    ValidationError:
      allOf:
        - $ref: "#/components/schemas/Error"
        - type: object
          properties:
            errors:
              type: array
              items:
                type: object
                properties:
                  field: { type: string }
                  message: { type: string }

    # --- Auth & User ---
    RegisterRequest:
      type: object
      properties:
        fullName: { type: string }
        email:
          type: string
          format: email
        password: { type: string, format: password, minLength: 8 }
      required: [fullName, email, password]

    LoginRequest:
      type: object
      properties:
        email: { type: string, format: email }
        password: { type: string, format: password }
      required: [email, password]

    AuthResponse:
      type: object
      properties:
        accessToken: { type: string, description: "JWT for API access" }
        refreshToken: { type: string, description: "JWT/opaque token to refresh access" }
        user:
          $ref: "#/components/schemas/User"
      required: [accessToken, refreshToken, user]

    User:
      type: object
      properties:
        id: { type: string, format: uuid }
        fullName: { type: string }
        email: { type: string, format: email }
        createdAt: { type: string, format: date-time }
      required: [id, fullName, email, createdAt]

    # --- Categories (nested for reads; parentId only in write models) ---
    Category:
      type: object
      description: Flat category representation (used in create/update responses).
      properties:
        id: { type: string, format: uuid }
        name: { type: string }
        ownerType: { type: string, enum: [system, user] }
        ownerId:
          type: string
          format: uuid
          nullable: true
          description: Required when ownerType=user
        createdAt: { type: string, format: date-time }
      required: [id, name, ownerType, createdAt]

    NestedCategory:
      type: object
      description: Category with recursive children for GET endpoints.
      properties:
        id: { type: string, format: uuid }
        name: { type: string }
        ownerType: { type: string, enum: [system, user] }
        ownerId: { type: string, format: uuid, nullable: true }
        createdAt: { type: string, format: date-time }
        children:
          type: array
          items: { $ref: "#/components/schemas/NestedCategory" }
      required: [id, name, ownerType, createdAt, children]

    NestedCategoryList:
      type: object
      properties:
        data:
          type: array
          items: { $ref: "#/components/schemas/NestedCategory" }
      required: [data]

    CreateCategoryRequest:
      type: object
      properties:
        name: { type: string }
        ownerType: { type: string, enum: [system, user], default: user }
        ownerId: { type: string, format: uuid, nullable: true }
        parentId:
          type: string
          format: uuid
          nullable: true
          description: Optional when creating a subcategory
      required: [name, ownerType]

    UpdateCategoryRequest:
      type: object
      properties:
        name: { type: string }
        parentId:
          type: string
          format: uuid
          nullable: true
          description: Move under a different parent (null to promote to root)

    # --- Products (unit removed from all schemas) ---
    Product:
      type: object
      properties:
        id: { type: string, format: uuid }
        name: { type: string }
        categoryId: { type: string, format: uuid }
        ownerType: { type: string, enum: [system, user] }
        ownerId: { type: string, format: uuid, nullable: true }
        notes: { type: string, nullable: true }
        createdAt: { type: string, format: date-time }
      required: [id, name, categoryId, ownerType, createdAt]

    ProductWithPrices:
      allOf:
        - $ref: "#/components/schemas/Product"
        - type: object
          properties:
            prices:
              description: Recent price samples for this product (embedded; no standalone endpoint).
              type: array
              items:
                type: object
                properties:
                  price: { $ref: "#/components/schemas/Money4" }
                  currency: { $ref: "#/components/schemas/CurrencyCode" }
                  location: { type: string }
                  source: { type: string }
                  collectedAt: { type: string, format: date-time }
                required: [price, currency, collectedAt]

    CreateProductRequest:
      type: object
      properties:
        name: { type: string }
        categoryId: { type: string, format: uuid }
        ownerType: { type: string, enum: [system, user], default: user }
        ownerId: { type: string, format: uuid, nullable: true }
        notes: { type: string, nullable: true }
      required: [name, categoryId, ownerType]

    UpdateProductRequest:
      type: object
      properties:
        name: { type: string }
        categoryId: { type: string, format: uuid }
        notes: { type: string, nullable: true }

    PagedProduct:
      type: object
      properties:
        data:
          type: array
          items: { $ref: "#/components/schemas/Product" }
        meta: { $ref: "#/components/schemas/PagedMeta" }
      required: [data, meta]

    # --- Expenses (totals removed from all schemas) ---
    Expense:
      type: object
      properties:
        id: { type: string, format: uuid }
        userId: { type: string, format: uuid }
        transactionDate: { type: string, format: date-time }
        currency: { $ref: "#/components/schemas/CurrencyCode" }
        notes: { type: string, nullable: true }
        items:
          type: array
          items: { $ref: "#/components/schemas/ExpenseItem" }
        createdAt: { type: string, format: date-time }
      required: [id, userId, transactionDate, currency, items, createdAt]

    ExpenseItem:
      type: object
      properties:
        productId: { type: string, format: uuid }
        quantity: { $ref: "#/components/schemas/Quantity3" }
        unitPrice: { $ref: "#/components/schemas/Money4" }
        notes: { type: string, nullable: true }
      required: [productId, quantity, unitPrice]

    CreateExpenseRequest:
      type: object
      properties:
        transactionDate: { type: string, format: date-time }
        currency: { $ref: "#/components/schemas/CurrencyCode" }
        notes: { type: string, nullable: true }
        items:
          type: array
          minItems: 1
          items:
            type: object
            properties:
              productId: { type: string, format: uuid }
              quantity: { $ref: "#/components/schemas/Quantity3" }
              unitPrice: { $ref: "#/components/schemas/Money4" }
              notes: { type: string, nullable: true }
            required: [productId, quantity, unitPrice]
      required: [transactionDate, currency, items]

    UpdateExpenseRequest:
      type: object
      properties:
        transactionDate: { type: string, format: date-time }
        currency: { $ref: "#/components/schemas/CurrencyCode" }
        notes: { type: string, nullable: true }
        items:
          type: array
          items:
            $ref: "#/components/schemas/ExpenseItem"

    PagedExpense:
      type: object
      properties:
        data:
          type: array
          items: { $ref: "#/components/schemas/Expense" }
        meta: { $ref: "#/components/schemas/PagedMeta" }
      required: [data, meta]
```