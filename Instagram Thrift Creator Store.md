title Instagram Thrift Creator Store

CUSTOMER [icon: user, color: yellow] {
  customer_id Int pk
  full_name String
  phone String
  instagram_handle String
  whatsapp_no String
  email String
  created_at DateTime
}

ADDRESS [icon: map-pin, color: yellow] {
  address_id Int pk
  customer_id Int
  recipient_name String
  line1 String
  line2 String
  city String
  state String
  postal_code String
  country String
}

CATEGORY [icon: tag, color: purple] {
  category_id Int pk
  category_name String
}

PRODUCT [icon: package, color: blue] {
  product_id Int pk
  category_id Int
  product_name String
  description String
  product_type String
  is_active Boolean
  created_at DateTime
}

THRIFT_PRODUCT [icon: refresh-cw, color: blue] {
  product_id Int pk
  brand String
  condition String
  thrift_source String
  acquired_date Date
}

HANDMADE_PRODUCT [icon: scissors, color: blue] {
  product_id Int pk
  material_details String
  customizable Boolean
  care_instructions String
}

PRODUCT_VARIANT [icon: layers, color: blue] {
  variant_id Int pk
  product_id Int
  sku_code String
  size String
  color String
  selling_price Decimal
  stock_qty Int
  variant_status String
}

HANDMADE_BATCH [icon: box, color: blue] {
  batch_id Int pk
  variant_id Int
  batch_date Date
  quantity_created Int
  batch_notes String
}

CUSTOMER_ORDER [icon: shopping-cart, color: green] {
  order_id Int pk
  customer_id Int
  address_id Int
  order_date Date
  order_source String
  order_status String
  subtotal Decimal
  shipping_fee Decimal
  total_amount Decimal
}

ORDER_ITEM [icon: list, color: green] {
  order_item_id Int pk
  order_id Int
  variant_id Int
  quantity Int
  unit_price Decimal
  line_total Decimal
}

PAYMENT [icon: credit-card, color: orange] {
  payment_id Int pk
  order_id Int
  payment_date Date
  amount Decimal
  payment_method String
  payment_status String
  transaction_reference String
}

SHIPMENT [icon: truck, color: orange] {
  shipment_id Int pk
  order_id Int
  courier_name String
  tracking_number String
  shipped_date Date
  delivered_date Date
  shipping_status String
}

ADDRESS.customer_id > CUSTOMER.customer_id
PRODUCT.category_id > CATEGORY.category_id
THRIFT_PRODUCT.product_id - PRODUCT.product_id
HANDMADE_PRODUCT.product_id - PRODUCT.product_id
PRODUCT_VARIANT.product_id > PRODUCT.product_id
HANDMADE_BATCH.variant_id > PRODUCT_VARIANT.variant_id
CUSTOMER_ORDER.customer_id > CUSTOMER.customer_id
CUSTOMER_ORDER.address_id > ADDRESS.address_id
ORDER_ITEM.order_id > CUSTOMER_ORDER.order_id
ORDER_ITEM.variant_id > PRODUCT_VARIANT.variant_id
PAYMENT.order_id > CUSTOMER_ORDER.order_id
SHIPMENT.order_id - CUSTOMER_ORDER.order_id