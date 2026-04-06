# Instagram Thrift & Handmade Store - ER Diagram

## Overview
This project designs an **ER diagram** for a small Instagram-based store that sells **thrifted** and **handmade** products.  
The database is designed to manage:

- products
- stock
- customers
- orders
- payments
- shipping

## Main Entities
- Customer
- Address
- Category
- Product
- Thrift_Product
- Handmade_Product
- Product_Variant
- Handmade_Batch
- Customer_Order
- Order_Item
- Payment
- Shipment

## Key Features
- Supports both **unique thrift items** and **multi-unit handmade products**
- Stores product details like **size, color, condition, price, and stock**
- Allows **one customer to place multiple orders**
- Allows **one order to contain multiple products**
- Separates **payment** and **shipment** details from orders
- Uses `Order_Item` as the junction table for order-product relationship

## Purpose
This ER design helps track:
- what products are sold
- product type: thrift or handmade
- available stock
- customer orders
- payment status
- shipping and delivery status

## Conclusion
The design is **clean, normalized, and practical** for a growing small business selling through Instagram and WhatsApp.

