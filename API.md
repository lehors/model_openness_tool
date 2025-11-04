# REST API

## Overview

This API provides read-only access to a collection of models. It is open to the public and does not require an API key for access.

### Response Format

- All responses are in **JSON** format.

### Rate Limiting

- **Limit**: 10,000 requests per hour per IP address.
- Exceeding this limit will result in temporary rate-limiting.
- Rate-limited responses include the following structure:
```json
{
  "error": {
    "code": 429,
    "message": "Rate limit exceeded. Try again later."
  }
}
```

---

## Endpoints

### **GET /api/v1/models**

Lists all (by default) models with pagination. Using query parameters, the list can be limited to models with a given string in their name and/or a given string in the name of their organization.

#### Query Parameters:
- `name` (optional): A string to be found in the model's name. This is case insensitive.
- `org` (optional): A string to be found in the model's organization name. This is case insensitive.
- `page` (optional): The page number to retrieve (default: `1`). Pages are 1-indexed.
- `limit` (optional): The number of models per page (default: `100`).

#### Example Request:
```http
GET /api/v1/models?page=2&limit=50
```

#### Example Response:
```json
{
  "pager": {
    "total_items": 235,
    "total_pages": 5,
    "current_page": 2
  },
  "models": [...]
}
```

#### Example Request:
```http
GET /api/v1/models?name=olmo-7b
```

#### Example Response:
```json
{
  "pager": {
    "total_items": 3,
    "total_pages": 1,
    "current_page": 1
  },
  "models": [
    ...
        "name": "OLMo-7B-Twin-2T-hf",
    ...
        "name": "OLMo-7B",
    ...
        "name": "OLMo-7B-hf",
    ...
  ]
}
```

#### Example Request:
```http
GET /api/v1/models?org=allen
```

#### Example Response:
```json
{
  "pager": {
    "total_items": 4,
    "total_pages": 1,
    "current_page": 1
  },
  "models": [
    ...
        "name": "OLMo-7B-Twin-2T-hf",
        "producer": "Allen Institute for AI",
    ...
        "name": "OLMo-1B-hf",
        "producer": "Allen Institute for AI",
    ...
        "name": "OLMo-7B",
        "producer": "Allen Institute for AI",
    ...
        "name": "OLMo-1B-hf",
        "producer": "Allen Institute for AI",
    ...
  ]
}
```

---

### **GET /api/v1/model/{model}**

Retrieves details of a specific model specified by ID or name.

#### Path Parameters:
- `model`: The ID or name of the model to retrieve. Model IDs and names can be found using the `/api/v1/models` endpoint. The name is case insensitive.

#### Example Request:
```http
GET /api/v1/model/1130
```

#### Example Response:
```json
{
  "id": "1130",
  "framework": {
    "name": "Model Openness Framework",
    "version": "1.0",
    "date": "2024-12-15"
  },
  "release": {
    "name": "OLMo-7B",
    ...
  },
  "classification": {
    "class": 1,
    "label": "Class I - Open Science",
    "progress": {
      "1": 100,
      "2": 100,
      "3": 100
    }
  }
}
```

#### Example Request:
```http
GET /api/v1/model/olmo-7b
```

#### Example Response:
```json
{
  "id": "1130",
  "framework": {
    "name": "Model Openness Framework",
    "version": "1.0",
    "date": "2024-12-15"
  },
  "release": {
    "name": "OLMo-7B",
    ...
  },
  "classification": {
    "class": 1,
    "label": "Class I - Open Science",
    "progress": {
      "1": 100,
      "2": 100,
      "3": 100
    }
  }
}
```
