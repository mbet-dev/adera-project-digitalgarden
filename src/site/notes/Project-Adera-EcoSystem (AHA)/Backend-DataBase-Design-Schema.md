---
{"dg-publish":true,"dg-path":"Project-Adera-EcoSystem (AHA)/Backend-DataBase-Design-Schema.md","permalink":"/Project-Adera-EcoSystem (AHA)/Backend-DataBase-Design-Schema/","dgPassFrontmatter":true,"noteIcon":""}
---


## 🔷 **Entities & Attributes**

### 1. **User**

- **PK**: `id` (UUID)
- **Attributes**:
    - Contact: `email`, `phone`
    - Identity: `first_name`, `last_name`, `role` (USER-DEFINED e.g., customer, driver, partner, shop_owner)
    - Profile: `avatar_url`, `profile_picture`, `business_name`, `business_license`
    - Location: `address`, `location` (PostGIS `point`)
    - Preferences: `language`, `email_notifications`, `push_notifications`, `notification_preference`, `language_preference`
    - Status: `is_verified`, `is_active`, `account_status`
    - Security/Activity: `created_at`, `updated_at`, `last_login_at`, `last_login`
    - Financial: `wallet_balance`

> **Notes**:
> 
> - Used as **customer**, **shop owner**, **driver**, **pickup/dropoff partner**, etc., based on role.

---

### 2. **Shop**

- **PK**: `id` (UUID)
- **FK**: `owner_id` → `users.id`
- **Attributes**:
    - Info: `name`, `description`, `category`, `logo_url`
    - Location: `location` (point), `address`
    - Contact: `phone`
    - Operations: `operating_hours` (JSONB), `delivery_radius` (meters)
    - Payments: `accepted_payment_methods` (array of `payment_method`)
    - Status: `is_active`, `is_verified`
    - Stats: `total_orders`, `average_rating`, `total_reviews`
    - Logistic Flags: `is_hub`, `hub_capacity`, `is_pickup`, `is_dropoff`, `shop_location_pic`

---

### 3. **Product**

- **PK**: `id` (UUID)
- **FK**: `shop_id` → `shops.id`
- **Attributes**:
    - Basic: `name`, `description`, `category`, `sku`
    - Pricing: `price`, `original_price`, `cost_price`
    - Inventory: `stock_quantity`, `min_stock_level`, `max_stock_level`
    - Media: `images` (text array)
    - Physical: `weight`, `dimensions` (JSONB)
    - Meta: `tags` (text array), `is_available`, `is_featured`
    - Analytics: `view_count`, `purchase_count`
    - Timestamps: `created_at`, `updated_at`

---

### 4. **Order**

- **PK**: `id` (UUID)
- **FKs**:
    - `customer_id` → `users.id`
    - `shop_id` → `shops.id`
    - `parcel_id` → `parcels.id` (nullable)
- **Attributes**:
    - Identifiers: `order_number` (unique)
    - Delivery: `delivery_address`, `delivery_location` (point), `delivery_phone`, `delivery_notes`
    - Financial: `subtotal`, `delivery_fee`, `tax_amount`, `discount_amount`, `total_amount`
    - Payment: `payment_method`, `payment_status`, `paid_at`
    - Status: `status` (e.g., 'pending', 'delivered')
    - Flags: `auto_create_parcel`
    - Timestamps: `created_at`, `updated_at`, `delivered_at`

---

### 5. **OrderItem**

- **PK**: `id` (UUID)
- **FKs**:
    - `order_id` → `orders.id`
    - `product_id` → `products.id`
- **Attributes**:
    - Product Snapshot: `product_name`, `product_price` (denormalized for immutability)
    - Quantity: `quantity`, `item_total`
    - Timestamp: `created_at`

> **Note**: Denormalized product info ensures order history remains consistent even if product data changes.

---

### 6. **Parcel**

- **PK**: `id` (UUID)
- **FKs**:
    - `sender_id` → `users.id`
    - `dropoff_partner_id`, `pickup_partner_id`, `driver_id` → `users.id` (nullable)
    - `dropoff_shop_id`, `pickup_shop_id` → `shops.id` (nullable)
- **Attributes**:
    - Tracking: `tracking_id` (unique)
    - Recipient: `recipient_name`, `recipient_phone`
    - Locations:
        
        - `pickup_location`, `pickup_address`
        - `delivery_location`, `delivery_address`
        - `current_location` (point, updated during transit)
        
    - Package: `description`, `weight`, `dimensions`, `declared_value`, `fragile`, `urgent`, `package_size`, `package_type`
    - Pricing: `delivery_fee`, `insurance_fee`, `total_amount`, `estimated_price`
    - Distance: `distance_km`
    - Status: `status` (integer code, e.g., 0 = created, 1 = picked up, etc.)
    - Payments: `payment_method`, `payment_status`, `paid_at`
    - Instructions: `pickup_instructions`, `delivery_instructions`
    - Compliance: `terms_accepted`, `expires_at` (auto-expire unclaimed parcels)
    - Timestamps: `created_at`, `updated_at`, `estimated_delivery`

---

### 7. **ParcelEvent**

- **PK**: `id` (UUID)
- **FKs**:
    - `parcel_id` → `parcels.id`
    - `actor_id` → `users.id`
- **Attributes**:
    - Status: `status` (integer, same coding as parcel?)
    - Actor Role: `actor_role` (USER-DEFINED: e.g., driver, partner, customer)
    - Location: `location` (point), `address`
    - Time: `event_time`
    - Evidence: `notes`, `photos` (text array), `signature_url`, `qr_code_hash`, `verification_method`
    - Timestamp: `created_at`

> **Purpose**: Audit trail/log of parcel handling (like shipment tracking events).

---

### 8. **Payment**

- **PK**: `id` (UUID)
- **FKs**:
    - `parcel_id` → `parcels.id` (nullable)
    - `order_id` → `orders.id` (nullable)
    - `user_id` → `users.id`
- **Attributes**:
    - Amount: `amount`, `currency` (default 'ETB')
    - Method/Status: `payment_method`, `payment_status`
    - Gateway: `gateway_transaction_id`, `gateway_response` (JSONB)
    - Timestamps: `created_at`, `updated_at`, `completed_at`

> **Design Note**: A payment can be for **either** an order **or** a parcel (not both), so one FK is always NULL.

---

### 9. **Review**

- **PK**: `id` (UUID)
- **FKs**:
    - `user_id` → `auth.users.id` (note: references `auth.users`, not `public.users` – potential inconsistency!)
    - `product_id` → `products.id`
- **Attributes**:
    - `rating` (1–5), `comment`
    - Timestamps: `created_at`, `updated_at`

> ⚠️ **Issue**: `reviews.user_id` references `auth.users`, while all other tables reference `public.users`. This may cause referential integrity problems unless `auth.users` and `public.users` are the same table (common in Supabase setups where `auth.users` is extended via a view or foreign data wrapper).

---

### 10. **Notification**

- **PK**: `id` (UUID)
- **FK**: `user_id` → `users.id`
- **Attributes**:
    - Content: `title`, `body`, `type`
    - Context: `reference_id` (UUID), `reference_type` (e.g., 'order', 'parcel')
    - Delivery Status:
        
        - `is_read`, `read_at`
        - `is_push_sent`, `is_email_sent`, `is_sms_sent`
        
    - Timestamp: `created_at`

> **Polymorphic Reference**: `reference_id` + `reference_type` allows linking to any entity (e.g., order, parcel, review).

---

### 11. **spatial_ref_sys** (PostGIS Internal Table)

- Not part of business logic; used by PostGIS for coordinate systems.
- Can be ignored in ER modeling.

---

## 🔗 **Relationships Summary**

|From Entity|To Entity|Cardinality|Notes|
|---|---|---|---|
|User|Shop (→)|1 : N|Owner can own multiple shops|
|Shop|Product (→)|1 : N|Products belong to one shop|
|User|Order (→)|1 : N|As customer|
|Shop|Order (→)|1 : N|Orders placed from a shop|
|Order|OrderItem (→)|1 : N|Each order has many items|
|Product|OrderItem (→)|1 : N|Product appears in many order items|
|Order|Parcel (→)|0..1 : 1|orders.parcel_id → parcels.id (optional 1:1)|
|User|Parcel (as sender, driver, partner) (→)|1 : N|Multiple roles via different FKs|
|Shop|Parcel (as pickup/dropoff) (→)|1 : N|Via pickup_shop_id, dropoff_shop_id|
|Parcel|ParcelEvent (→)|1 : N|Timeline of events|
|User|ParcelEvent (actor) (→)|1 : N|Who performed the event|
|User|Payment (→)|1 : N|Payer|
|Order / Parcel|Payment (→)|1 : 0..1|Payment linked to one or the other|
|User|Review (→)|1 : N|Customer reviews|
|Product|Review (→)|1 : N|Reviews about product|
|User|Notification (→)|1 : N|Notifications sent to user|

---

## 🗺️ **ER Diagram (Textual Representation)**



[User] 1─── owns ───N [Shop]

[Shop] 1─── has ───N [Product]

[User] 1─── places ───N [Order]

[Shop] 1─── receives ───N [Order]

[Order] 1─── contains ───N [OrderItem]

[Product] 1─── used_in ───N [OrderItem]

  

[Order] 0..1 ─── linked_to ───1 [Parcel]

[User] 1─── (as sender) ───N [Parcel]

[User] 1─── (as driver/partner) ───N [Parcel]

[Shop] 1─── (as pickup/dropoff) ───N [Parcel]

  

[Parcel] 1─── has ───N [ParcelEvent]

[User] 1─── (actor) ───N [ParcelEvent]

  

[User] 1─── makes ───N [Payment]

[Order] 0..1 ─── paid_by ───1 [Payment]

[Parcel] 0..1 ─── paid_by ───1 [Payment]

  

[User] 1─── writes ───N [Review]

[Product] 1─── reviewed_by ───N [Review]

  

[User] 1─── receives ───N [Notification]

> **Polymorphic**: `Notification.reference_id` → (Order, Parcel, etc.)

---

## 📌 **Data Model Observations & Recommendations**

1. **User Role Flexibility**:
    
    - A single `users` table supports multiple roles (customer, driver, partner, shop owner). This is efficient but requires app-level logic to enforce role-specific rules.
2. **Denormalization**:
    
    - `order_items` stores `product_name` and `product_price` → good for immutability.
    - `shops` stores `average_rating` and `total_reviews` → consider computed columns or triggers to keep in sync with `reviews`.
3. **Polymorphic References**:
    
    - `notifications.reference_id` + `reference_type` is flexible but not foreign-key enforceable. Consider separate tables per type if referential integrity is critical.
4. **Review Table Issue**:
    
    - `reviews.user_id` references `auth.users` while others use `public.users`.  
        ✅ **Fix**: Align to `public.users(id)` unless you're using Supabase auth extension intentionally.
5. **Payment Flexibility**:
    
    - Payments support both orders and parcels via nullable FKs — clean design.
6. **Geospatial Data**:
    
    - Use of `point` (PostGIS) for locations → enables distance/radius queries (e.g., delivery radius).
7. **Status Tracking**:
    
    - `parcels.status` (int) and `parcel_events.status` → define a lookup table or enum for clarity.
8. **Custom Types**:
    
    - `USER-DEFINED` types like `payment_status`, `user_role`, `payment_method`, `actor_role` — ensure these are created as PostgreSQL `ENUM`s or domains.

---

## ✅ Final Notes

This schema supports a **multi-role e-commerce + logistics platform** with:

- Online ordering from shops
- Parcel delivery with tracking
- Payments for both orders and standalone parcels
- Reviews and notifications