# Google Merchant Center Action Reference

**Provider:** `google`
**Service:** `merchant-center`
**App name:** `google-merchant-center`

## Actions

### list_products

List products in Google Merchant Center.

```bash
curl -s -X POST "$GATORHUB_URL/api/v1/action" \
  -H "Authorization: Bearer $GATORHUB_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "merchant-center",
    "action": "list_products",
    "params": {"merchant_id": "123456789", "max_results": 50}
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `merchant_id` | string | yes | Merchant Center account ID |
| `max_results` | integer | no | Max products to return (default 50) |
| `page_token` | string | no | Pagination token |

### get_product

Get details of a specific product in Google Merchant Center. Returns full product data including status and diagnostics.

```bash
curl -s -X POST "$GATORHUB_URL/api/v1/action" \
  -H "Authorization: Bearer $GATORHUB_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "merchant-center",
    "action": "get_product",
    "params": {
      "merchant_id": "123456789",
      "product_id": "online:en:US:sku-001"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `merchant_id` | string | yes | Merchant Center account ID |
| `product_id` | string | yes | Product ID |

### insert_product

Insert (create) a new product in Google Merchant Center.

```bash
curl -s -X POST "$GATORHUB_URL/api/v1/action" \
  -H "Authorization: Bearer $GATORHUB_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "merchant-center",
    "action": "insert_product",
    "params": {
      "merchant_id": "123456789",
      "offer_id": "sku-001",
      "title": "Premium Wireless Mouse",
      "description": "Ergonomic wireless mouse with USB-C receiver",
      "link": "https://mystore.com/products/wireless-mouse",
      "price_value": "29.99",
      "price_currency": "USD",
      "availability": "in_stock",
      "condition": "new"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `merchant_id` | string | yes | Merchant Center account ID |
| `offer_id` | string | yes | Unique product offer ID |
| `title` | string | yes | Product title |
| `description` | string | no | Product description |
| `link` | string | yes | URL to the product page |
| `price_value` | string | yes | Price value (e.g. 19.99) |
| `price_currency` | string | yes | Currency code (e.g. USD, BRL) |
| `availability` | string | no | Availability: in_stock, out_of_stock (default in_stock) |
| `condition` | string | no | Product condition: new, refurbished, used (default new) |

### update_product

Update an existing product in Google Merchant Center. Only provided fields are updated.

```bash
curl -s -X POST "$GATORHUB_URL/api/v1/action" \
  -H "Authorization: Bearer $GATORHUB_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "merchant-center",
    "action": "update_product",
    "params": {
      "merchant_id": "123456789",
      "product_id": "online:en:US:sku-001",
      "price_value": "24.99",
      "price_currency": "USD",
      "availability": "in_stock"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `merchant_id` | string | yes | Merchant Center account ID |
| `product_id` | string | yes | Product ID |
| `title` | string | no | New title |
| `price_value` | string | no | New price value |
| `price_currency` | string | no | New currency code |
| `availability` | string | no | New availability |

### delete_product

Delete a product from Google Merchant Center. This action is permanent and cannot be undone.

```bash
curl -s -X POST "$GATORHUB_URL/api/v1/action" \
  -H "Authorization: Bearer $GATORHUB_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "google",
    "service": "merchant-center",
    "action": "delete_product",
    "params": {
      "merchant_id": "123456789",
      "product_id": "online:en:US:sku-001"
    }
  }'
```

| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
| `merchant_id` | string | yes | Merchant Center account ID |
| `product_id` | string | yes | Product ID |
