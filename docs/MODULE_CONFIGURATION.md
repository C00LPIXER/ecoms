# Module Configuration Guide

## Overview
This guide explains how to configure and customize the e-commerce API for different types of shops. Each module can be configured independently and can be enabled/disabled as needed.

## Module Configuration

### 1. Product Module
```json
{
  "enabled": true,
  "config": {
    "types": {
      "physical": {
        "enabled": true,
        "attributes": ["weight", "dimensions", "shipping_class"]
      },
      "digital": {
        "enabled": true,
        "attributes": ["file_type", "download_limit", "expiry_days"]
      },
      "subscription": {
        "enabled": true,
        "attributes": ["billing_cycle", "trial_period", "auto_renew"]
      }
    },
    "variants": {
      "enabled": true,
      "attributes": ["size", "color", "material"],
      "pricing": ["base", "variant", "combination"]
    },
    "inventory": {
      "tracking": true,
      "low_stock_threshold": 10,
      "out_of_stock_threshold": 0
    }
  }
}
```

### 2. Order Module
```json
{
  "enabled": true,
  "config": {
    "status": {
      "flow": ["pending", "processing", "shipped", "delivered"],
      "cancellation": {
        "allowed": true,
        "time_limit": 24
      }
    },
    "payment": {
      "methods": ["credit_card", "paypal", "bank_transfer"],
      "currency": "USD",
      "tax": {
        "enabled": true,
        "calculation": "per_order"
      }
    },
    "shipping": {
      "methods": ["standard", "express", "pickup"],
      "zones": {
        "enabled": true,
        "calculation": "weight"
      }
    }
  }
}
```

### 3. Customer Module
```json
{
  "enabled": true,
  "config": {
    "registration": {
      "required_fields": ["email", "password"],
      "optional_fields": ["phone", "address"],
      "verification": {
        "email": true,
        "phone": false
      }
    },
    "profiles": {
      "addresses": {
        "max": 5,
        "types": ["shipping", "billing"]
      },
      "preferences": {
        "language": true,
        "currency": true,
        "notifications": true
      }
    }
  }
}
```

### 4. Inventory Module
```json
{
  "enabled": true,
  "config": {
    "tracking": {
      "method": "real_time",
      "update_frequency": "immediate"
    },
    "warehouses": {
      "multiple": true,
      "transfer": {
        "enabled": true,
        "approval_required": true
      }
    },
    "alerts": {
      "low_stock": true,
      "out_of_stock": true,
      "reorder_point": true
    }
  }
}
```

### 5. Marketing Module
```json
{
  "enabled": true,
  "config": {
    "promotions": {
      "types": ["discount", "coupon", "bundle"],
      "rules": {
        "stacking": false,
        "max_discount": 50
      }
    },
    "loyalty": {
      "enabled": true,
      "points": {
        "earn_rate": 1,
        "redemption_rate": 100
      }
    },
    "notifications": {
      "channels": ["email", "sms", "push"],
      "triggers": ["abandoned_cart", "low_stock", "price_drop"]
    }
  }
}
```

## Customization Examples

### 1. Clothing Store
```json
{
  "product": {
    "types": {
      "physical": {
        "attributes": ["size", "color", "material", "care_instructions"]
      }
    },
    "variants": {
      "attributes": ["size", "color"],
      "pricing": "variant"
    }
  },
  "inventory": {
    "tracking": true,
    "low_stock_threshold": 5
  }
}
```

### 2. Digital Products Store
```json
{
  "product": {
    "types": {
      "digital": {
        "enabled": true,
        "attributes": ["file_type", "download_limit", "license_type"]
      }
    },
    "inventory": {
      "tracking": false
    }
  },
  "order": {
    "shipping": {
      "enabled": false
    }
  }
}
```

### 3. Subscription Service
```json
{
  "product": {
    "types": {
      "subscription": {
        "enabled": true,
        "attributes": ["billing_cycle", "trial_period", "auto_renew"]
      }
    }
  },
  "order": {
    "payment": {
      "methods": ["credit_card", "paypal"],
      "recurring": true
    }
  }
}
```

## Module Dependencies

### Optional Dependencies
```json
{
  "cart": {
    "product": {
      "required": false
    }
  },
  "order": {
    "cart": {
      "required": false
    },
    "product": {
      "required": false
    }
  },
  "review": {
    "product": {
      "required": false
    },
    "customer": {
      "required": false
    }
  }
}
```

## Feature Flags

### Global Features
```json
{
  "features": {
    "multi_currency": true,
    "multi_language": true,
    "multi_warehouse": true,
    "multi_vendor": false,
    "subscription": false,
    "digital_products": false
  }
}
```

### Module-specific Features
```json
{
  "product": {
    "variants": true,
    "bundles": true,
    "subscriptions": false
  },
  "order": {
    "partial_payments": true,
    "backorders": true,
    "preorders": false
  },
  "customer": {
    "wholesale": false,
    "loyalty": true,
    "referral": true
  }
}
```

## Integration Configuration

### External Services
```json
{
  "payment": {
    "stripe": {
      "enabled": true,
      "mode": "live"
    },
    "paypal": {
      "enabled": true,
      "mode": "sandbox"
    }
  },
  "shipping": {
    "fedex": {
      "enabled": true,
      "account_number": "xxx"
    },
    "ups": {
      "enabled": false
    }
  },
  "email": {
    "sendgrid": {
      "enabled": true,
      "templates": true
    }
  }
}
```

### Webhooks
```json
{
  "webhooks": {
    "order_created": {
      "url": "https://api.example.com/webhooks/order",
      "secret": "xxx",
      "events": ["created", "updated", "deleted"]
    },
    "product_updated": {
      "url": "https://api.example.com/webhooks/product",
      "secret": "xxx",
      "events": ["updated"]
    }
  }
}
``` 