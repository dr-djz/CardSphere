# CardSphere Website Specification

## 1. Overview

**CardSphere** is a web-based trading card marketplace tailored for the European market, inspired by TCGPlayer.com. It provides a centralized platform for buying and selling trading cards such as *Magic: The Gathering*, *Pokémon*, *Yu-Gi-Oh!*, *Flesh and Blood*, and more. The platform emphasizes a user-friendly interface, real-time search with autocomplete, detailed card information, valuation analytics, multilingual support, and GDPR compliance. Key features include secure transactions, seller tools, and a responsive design optimized for both desktop and mobile users.

---

## 2. Functional Requirements

This section outlines the core features and functionalities the CardSphere platform must deliver.

### 2.1 User Authentication
- **Registration and Login:**
  - Users register with an email and password; email verification required via a confirmation link.
  - Login via email and password with a "Forgot Password" reset option.
- **Profile Management:**
  - Update shipping address, preferred currency (e.g., Euro, GBP), and payment methods.
  - View order history and account settings.
- **Role-Based Access:**
  - Roles: `buyer`, `seller`, `admin`.
  - Buyers can purchase cards; sellers can list cards; admins manage users and content.

### 2.2 Marketplace
- **Card Listing:**
  - Sellers create listings with fields: card name, condition (e.g., Mint, Near Mint), price, quantity.
  - Form validation ensures valid inputs (e.g., positive price, non-zero quantity).
- **Buying Process:**
  - Buyers add listings to a cart and proceed to checkout with a summary of items and total cost.
  - Support for guest checkout optional but not prioritized.
- **Order Management:**
  - Buyers and sellers view orders with statuses: `pending`, `shipped`, `completed`.
  - Sellers update order status; buyers track shipping.

### 2.3 Search Functionality
- **Real-Time Search:**
  - Prominent search bar on all pages with autocomplete predicting card names after 300ms debounce.
  - Suggestions include card name and current market price.
- **Filtering:**
  - Filters for set, rarity, condition, price range, and seller location.
  - Reset filter option provided.

### 2.4 Card Valuation
- **Market Price Trends:**
  - Display 7-day and 30-day average prices based on recent sales data.
  - Graphical representation (e.g., line chart) for trends.
- **Analytics:**
  - Historical price data and card popularity metrics (e.g., views, sales volume).

### 2.5 Seller Tools
- **Dashboard:**
  - Manage listings: add, edit, delete.
  - View sales analytics: revenue, top-selling cards, order volume.
  - Track orders with status updates.
- **Bulk Upload:**
  - Import listings via CSV with columns: card name, set, condition, price, quantity.
- **Notifications:**
  - Email or in-app alerts for new orders and buyer messages.

### 2.6 Localization
- **Multilingual Support:**
  - Languages: English, French, German, Spanish, Italian (extensible to others).
  - Language selector in UI header/footer.
- **Currency Handling:**
  - Support for Euro (€) and GBP (£); prices convert based on user preference.

### 2.7 Payment Integration
- **Providers:**
  - Stripe or PayPal EU for secure transactions.
- **Process:**
  - Buyers pay via card or PayPal; funds held until order completion (escrow-like).
  - Sellers receive payouts minus platform fees (e.g., 5-10%).

### 2.8 Admin Panel
- **User Management:**
  - View, edit, ban users; assign roles.
- **Card Management:**
  - Add/edit card details manually or via automated jobs.
- **Order Oversight:**
  - Resolve disputes, view all transactions.

---

## 3. Non-Functional Requirements

These requirements ensure the platform meets performance, security, and usability standards.

### 3.1 Performance
- Page load times under 2 seconds using Next.js SSR and Redis caching.
- Database queries optimized with indexing and efficient joins.

### 3.2 Security
- JWT for stateless, secure authentication; tokens expire after 24 hours with refresh option.
- Encrypt sensitive data (e.g., passwords, payment info) with AES-256.
- GDPR compliance:
  - Cookie consent banner on first visit.
  - User data access/update/deletion via profile settings.
  - Privacy policy link in footer.

### 3.3 Scalability
- Handle 10,000 concurrent users with DigitalOcean App Platform auto-scaling.
- Use managed PostgreSQL and Redis for database and caching scalability.

### 3.4 Usability
- Responsive design for mobile (min-width: 320px) and desktop (max-width: 1920px).
- Intuitive navigation with consistent UI patterns (e.g., Material-UI/Ant Design).

### 3.5 Reliability
- 99.9% uptime with DigitalOcean's managed services.
- Daily database backups; error logging with Sentry.

---

## 4. Technical Architecture

### 4.1 Frontend
- **Framework:** Next.js (React-based) with App Router for better file-based routing and server components.
- **Styling:** CSS Modules for component-scoped styling with Material-UI for component library.
- **UI Library:** Material-UI for consistent, responsive components.

### 4.2 Backend
- **Framework:** Node.js with Express.js for RESTful APIs.
- **Authentication:** JWT with middleware for route protection.

### 4.3 Database
- **Type:** PostgreSQL, managed by DigitalOcean.
- **ORM:** Sequelize for database interactions.

### 4.4 Caching
- **Technology:** DigitalOcean Managed Redis for API and search caching.

### 4.5 Hosting
- **Platform:** DigitalOcean App Platform for frontend and backend deployment.

### 4.6 CI/CD
- **Tools:** Git with GitHub; GitHub Actions for automated testing/deployment.

---

## 5. User Interface

### 5.0 Global Header
- **Logo**: Links to the homepage.
- **Card Brands Navigation**: Horizontal menu listing major trading card brands (e.g., Magic: The Gathering, Pokémon, Yu-Gi-Oh!, Flesh and Blood). Each links to the brand's dedicated page.
- **Search Bar**: Allows searching across all brands or within a selected brand.
- **User Menu**: Dropdown for login, registration, profile, and logout.

### 5.1 Homepage
- **Global Header**: As described in 5.0.
- **Top Banner/Carousel**: Rotates the latest TCG news or featured sets.
- **Graphic Buttons**: Icons representing different trading card brands with character artwork, each linking to a blog or article about that brand's trading cards.
- **Search Bar**:
  - Centered, with autocomplete dropdown showing card names and prices.

### 5.2 Card Pages
- **Details:**
  - Card name, image (300x420px), set, rarity, condition (using PSA grading system), price, market trends.
  - Tooltips for rules text or additional info.
- **Buy Options:**
  - List of available sellers with price and condition.

### 5.3 Seller Dashboard
- **Layout:**
  - Tabs for Listings, Analytics, Orders.
  - Table view for listings with edit/delete buttons.

### 5.4 Responsive Design
- Mobile-first approach with CSS Grid/Flexbox.
- Touch-friendly buttons (min-size: 48x48px).

---

## 6. Database Schema

### 6.1 Users Table
| Column         | Type       | Constraints       | Description           |
|----------------|------------|-------------------|-----------------------|
| `user_id`      | SERIAL     | Primary Key       | Unique user ID        |
| `username`     | VARCHAR(50)| Not Null          | Display name          |
| `email`        | VARCHAR(255)| Unique, Not Null | Login email           |
| `password_hash`| VARCHAR(255)| Not Null         | Hashed password       |
| `role`         | ENUM       | Not Null          | `buyer`, `seller`, `admin` |

### 6.2 Cards Table
| Column        | Type       | Constraints       | Description           |
|---------------|------------|-------------------|-----------------------|
| `card_id`     | SERIAL     | Primary Key       | Unique card ID        |
| `name`        | VARCHAR(100)| Not Null         | Card name             |
| `set_name`    | VARCHAR(100)| Not Null         | Set name              |
| `set_number`  | VARCHAR(20)| Not Null          | Set identifier        |
| `rarity`      | VARCHAR(20)| Not Null          | Rarity level          |
| `image_url`   | VARCHAR(255)| Not Null         | Image URL             |
| `market_price`| DECIMAL(10,2)| Not Null       | Current market price  |

### 6.3 Listings Table
| Column      | Type       | Constraints       | Description           |
|-------------|------------|-------------------|-----------------------|
| `listing_id`| SERIAL     | Primary Key       | Unique listing ID     |
| `card_id`   | INTEGER    | Foreign Key (Cards)| Card reference        |
| `seller_id` | INTEGER    | Foreign Key (Users)| Seller reference      |
| `condition` | ENUM       | Not Null          | PSA grading (1-10)    |
| `price`     | DECIMAL(10,2)| Not Null        | Listing price         |
| `quantity`  | INTEGER    | Not Null          | Available stock       |

### 6.4 Orders Table
| Column       | Type       | Constraints       | Description           |
|--------------|------------|-------------------|-----------------------|
| `order_id`   | SERIAL     | Primary Key       | Unique order ID       |
| `buyer_id`   | INTEGER    | Foreign Key (Users)| Buyer reference       |
| `seller_id`  | INTEGER    | Foreign Key (Users)| Seller reference      |
| `total_amount`| DECIMAL(10,2)| Not Null        | Total cost            |
| `status`     | ENUM       | Not Null          | `pending`, `shipped`, etc. |

---

## 7. API Endpoints

### 7.1 Authentication
- **POST /api/auth/register** - Register a new user.
- **POST /api/auth/login** - Login and return JWT.
- **GET /api/auth/profile** - Fetch user profile (authenticated).

### 7.2 Cards
- **GET /api/cards** - List all cards with pagination.
- **GET /api/cards/:id** - Fetch card details.
- **POST /api/cards** - Add new card (admin only).

### 7.3 Listings
- **GET /api/listings** - List all listings with filters.
- **POST /api/listings** - Create a listing (seller only).
- **PUT /api/listings/:id** - Update listing (seller only).
- **DELETE /api/listings/:id** - Delete listing (seller only).

### 7.4 Orders
- **GET /api/orders** - List user orders (buyer/seller).
- **POST /api/orders** - Create an order (buyer).
- **PUT /api/orders/:id** - Update order status (seller).

### 7.5 Search
- **GET /api/search?q=query** - Return autocomplete suggestions with prices.

---

## 8. Third-Party Integrations

- **Payment Gateways:** Stripe or PayPal EU for transactions.
- **Localization:** i18next for multilingual support.
- **Monitoring:** Sentry for errors; LogRocket for session replays.

---

## 9. Deployment and Hosting

- **Platform:** DigitalOcean App Platform for frontend and backend.
- **Database:** DigitalOcean Managed PostgreSQL.
- **Caching:** DigitalOcean Managed Redis.
- **CI/CD:** GitHub Actions with automated testing and deployment.

---

## 10. Testing and Quality Assurance

- **Unit Tests:** Test frontend components and backend logic.
- **Integration Tests:** Validate API endpoints and database interactions.
- **E2E Tests:** Simulate user flows (e.g., search, buy, list).
- **Performance Tests:** Ensure <2s load times under load.

---

## 11. Maintenance and Support

- **Updates:** Monthly dependency updates and security patches.
- **Backups:** Daily database snapshots via DigitalOcean.
- **Support:** Feedback form in UI; email support channel.

---

This specification provides a detailed, actionable guide for building the CardSphere platform, ensuring it meets the needs of European trading card enthusiasts while adhering to technical and regulatory standards.