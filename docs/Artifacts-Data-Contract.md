```yaml
openapi: 3.0.1
info:
  title: Chanchito API
  version: 1.0.0
  description: API contract for the MVP scope of Chanchito, a mobile-first social finance application.

servers:
  - url: https://api.chanchito.app

paths:

  /auth/login:
    post:
      summary: Login user
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/LoginRequest'
      responses:
        '200':
          description: Successful login

  /auth/login/social:
    post:
      summary: Login with a social account (Google or Facebook)
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/SocialLoginRequest'
      responses:
        '200':
          description: Successful social login
          
  /auth/register:
    post:
      summary: Register a new user
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/RegisterRequest'
      responses:
        '201':
          description: User registered

  /expenses:
    post:
      summary: Create a quick expense
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateExpenseRequest'
      responses:
        '201':
          description: Expense created
      security:
        - bearerAuth: []

    get:
      summary: Get all expenses
      responses:
        '200':
          description: List of user expenses
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/Expense'
      security:
        - bearerAuth: []

  /expenses/{id}:
    patch:
      summary: Update an existing expense
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateExpenseRequest'
      responses:
        '200':
          description: Expense updated
      security:
        - bearerAuth: []

    delete:
      summary: Delete an existing expense
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
      responses:
        '204':
          description: Expense deleted
      security:
        - bearerAuth: []

  /products:
    get:
      summary: Get list of predefined financial entries
      responses:
        '200':
          description: Predefined financial entries
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/ProductSummary'
      security:
        - bearerAuth: []

    post:
      summary: Add new product or category
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateProductRequest'
      responses:
        '201':
          description: Product created
      security:
        - bearerAuth: []

  /products/{id}:
    patch:
      summary: Update a product or category
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateProductRequest'
      responses:
        '200':
          description: Product updated
      security:
        - bearerAuth: []

    delete:
      summary: Delete a product or category
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
      responses:
        '204':
          description: Product deleted
      security:
        - bearerAuth: []

  /feedback:
    post:
      summary: Submit user feedback or bug
      requestBody:
        required: true
        content:
          application/json:
            schema:
              $ref: '#/components/schemas/CreateFeedbackRequest'
      responses:
        '200':
          description: Feedback submitted
      security:
        - bearerAuth: []

    get:
      summary: Get all feedback entries
      responses:
        '200':
          description: List of feedback
          content:
            application/json:
              schema:
                type: array
                items:
                  $ref: '#/components/schemas/FeedbackRequest'
      security:
        - bearerAuth: []

  /feedback/{id}:
    delete:
      summary: Delete feedback entry
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
      responses:
        '204':
          description: Feedback deleted
      security:
        - bearerAuth: []

components:
  securitySchemes:
    bearerAuth:
      type: http
      scheme: bearer
      bearerFormat: JWT

  schemas:
    LoginRequest:
      type: object
      properties:
        email:
          type: string
        password:
          type: string
      required: [email, password]

    RegisterRequest:
      type: object
      properties:
        name:
          type: string
        email:
          type: string
        password:
          type: string
      required: [name, email, password]

    CreateExpenseRequest:
      type: object
      properties:
        description:
          type: string
        currency:
          type: string
        products:
          type: array
          items:
            type: object
            properties:
              productId:
                type: string
              quantity:
                type: integer
              price:
                type: number
        total:
          type: number
        date:
          type: string
          format: date
      required: [description, currency, products, total, date]

    Expense:
      type: object
      properties:
        id:
          type: string
        description:
          type: string
        date:
          type: string
          format: date
        currency:
          type: string
        total:
          type: number
        items:
          type: array
          items:
            type: object
            properties:
              productId:
                type: string
              quantity:
                type: integer
              price:
                type: number
              name:
                type: string
      required: [id, description, date, currency, total, items]

    CreateProductRequest:
      type: object
      properties:
        name:
          type: string
        category:
          type: string
        subcategory:
          type: string
        prices:
          type: array
          items:
            type: number
        description:
          type: string
      required: [name, type, category, subcategory, prices, description]
      
    SocialLoginRequest:
      type: object
      properties:
        provider:
          type: string
          enum: [google, facebook]
        token:
          type: string
      required: [provider, token]

    ProductSummary:
      type: object
      properties:
        id:
          type: string
        name:
          type: string
        category:
          type: string
        subcategory:
          type: string
        prices:
          type: array
          items:
            type: number
      required: [name, category, subcategory, prices]

    CreateFeedbackRequest:
      type: object
      properties:
        message:
          type: string
        date:
          type: string
          format: date
      required: [message, date]
      
    FeedbackRequest:
      type: object
      properties:
        id:
          type: string
        message:
          type: string
        date:
          type: string
          format: date
        email:
          type: string
      required: [id, message, date, email]
```