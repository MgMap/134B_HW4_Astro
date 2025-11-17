---
layout: ../../layouts/BlogPostLayout.astro
title: 'Working with REST APIs'
description: 'Learn how to consume and build RESTful APIs, including authentication, error handling, and best practices.'
publishDate: '2025-11-16'
author: 'Min Aung Paing'
authorRole: 'Software Engineer'
authorBio: 'Web client languages student, junior year'
tags: ['APIs', 'REST', 'Backend']
---

## What is a REST API?

REST (Representational State Transfer) is an architectural style for building web services. RESTful APIs use HTTP methods to perform CRUD operations and are the backbone of modern web applications.

## HTTP Methods

### GET - Retrieve Data

```javascript
// Fetch all users
fetch('https://api.example.com/users')
  .then(response => response.json())
  .then(data => console.log(data));

// Fetch specific user
fetch('https://api.example.com/users/123')
  .then(response => response.json())
  .then(user => console.log(user));
```

### POST - Create Data

```javascript
// Create new user
fetch('https://api.example.com/users', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    name: 'John Doe',
    email: 'john@example.com'
  })
})
  .then(response => response.json())
  .then(user => console.log('Created:', user));
```

### PUT - Update Data

```javascript
// Update entire user
fetch('https://api.example.com/users/123', {
  method: 'PUT',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    name: 'John Updated',
    email: 'john.updated@example.com'
  })
})
  .then(response => response.json())
  .then(user => console.log('Updated:', user));
```

### PATCH - Partial Update

```javascript
// Update only specific fields
fetch('https://api.example.com/users/123', {
  method: 'PATCH',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    email: 'newemail@example.com'
  })
})
  .then(response => response.json())
  .then(user => console.log('Patched:', user));
```

### DELETE - Remove Data

```javascript
// Delete user
fetch('https://api.example.com/users/123', {
  method: 'DELETE'
})
  .then(response => {
    if (response.ok) {
      console.log('User deleted successfully');
    }
  });
```

## Modern Fetch with Async/Await

Much cleaner syntax using async/await:

```javascript
async function getUsers() {
  try {
    const response = await fetch('https://api.example.com/users');

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const users = await response.json();
    return users;
  } catch (error) {
    console.error('Error fetching users:', error);
    throw error;
  }
}

// Usage
const users = await getUsers();
```

## Authentication

### Bearer Token Authentication

```javascript
async function fetchWithAuth(url) {
  const token = localStorage.getItem('authToken');

  const response = await fetch(url, {
    headers: {
      'Authorization': `Bearer ${token}`,
      'Content-Type': 'application/json'
    }
  });

  return response.json();
}
```

### API Key Authentication

```javascript
const API_KEY = 'your-api-key';

async function fetchWithApiKey(url) {
  const response = await fetch(url, {
    headers: {
      'X-API-Key': API_KEY
    }
  });

  return response.json();
}
```

## Error Handling

### Comprehensive Error Handling

```javascript
async function robustFetch(url, options = {}) {
  try {
    const response = await fetch(url, options);

    // Handle HTTP errors
    if (!response.ok) {
      const errorData = await response.json().catch(() => ({}));

      throw new Error(
        errorData.message ||
        `HTTP ${response.status}: ${response.statusText}`
      );
    }

    // Parse JSON
    const data = await response.json();
    return { data, error: null };

  } catch (error) {
    // Network errors, JSON parsing errors, etc.
    console.error('Fetch error:', error);
    return { data: null, error: error.message };
  }
}

// Usage
const { data, error } = await robustFetch('/api/users');

if (error) {
  console.error('Failed to fetch users:', error);
} else {
  console.log('Users:', data);
}
```

## Building an API Client

Create a reusable API client class:

```javascript
class APIClient {
  constructor(baseURL) {
    this.baseURL = baseURL;
    this.token = null;
  }

  setToken(token) {
    this.token = token;
  }

  async request(endpoint, options = {}) {
    const url = `${this.baseURL}${endpoint}`;

    const headers = {
      'Content-Type': 'application/json',
      ...options.headers
    };

    if (this.token) {
      headers.Authorization = `Bearer ${this.token}`;
    }

    const config = {
      ...options,
      headers
    };

    const response = await fetch(url, config);

    if (!response.ok) {
      throw new Error(`API Error: ${response.status}`);
    }

    return response.json();
  }

  // Convenience methods
  get(endpoint) {
    return this.request(endpoint);
  }

  post(endpoint, data) {
    return this.request(endpoint, {
      method: 'POST',
      body: JSON.stringify(data)
    });
  }

  put(endpoint, data) {
    return this.request(endpoint, {
      method: 'PUT',
      body: JSON.stringify(data)
    });
  }

  delete(endpoint) {
    return this.request(endpoint, {
      method: 'DELETE'
    });
  }
}

// Usage
const api = new APIClient('https://api.example.com');
api.setToken('your-token');

const users = await api.get('/users');
const newUser = await api.post('/users', { name: 'John' });
```

## Query Parameters

### Building URLs with Parameters

```javascript
// Manual approach
const userId = 123;
const filter = 'active';
const url = `https://api.example.com/users?id=${userId}&status=${filter}`;

// Better: Using URLSearchParams
const params = new URLSearchParams({
  id: userId,
  status: filter,
  page: 1,
  limit: 10
});

const url = `https://api.example.com/users?${params}`;
// https://api.example.com/users?id=123&status=active&page=1&limit=10

// In fetch
const response = await fetch(url);
```

## Pagination

### Handling Paginated Responses

```javascript
async function fetchAllPages(baseUrl) {
  let allData = [];
  let page = 1;
  let hasMore = true;

  while (hasMore) {
    const response = await fetch(`${baseUrl}?page=${page}&limit=100`);
    const data = await response.json();

    allData = allData.concat(data.results);

    hasMore = data.hasNextPage;
    page++;
  }

  return allData;
}
```

## Rate Limiting

### Respecting Rate Limits

```javascript
class RateLimitedClient {
  constructor(requestsPerSecond = 10) {
    this.queue = [];
    this.processing = false;
    this.interval = 1000 / requestsPerSecond;
  }

  async request(url, options) {
    return new Promise((resolve, reject) => {
      this.queue.push({ url, options, resolve, reject });
      this.processQueue();
    });
  }

  async processQueue() {
    if (this.processing || this.queue.length === 0) {
      return;
    }

    this.processing = true;
    const { url, options, resolve, reject } = this.queue.shift();

    try {
      const response = await fetch(url, options);
      const data = await response.json();
      resolve(data);
    } catch (error) {
      reject(error);
    }

    setTimeout(() => {
      this.processing = false;
      this.processQueue();
    }, this.interval);
  }
}
```

## Best Practices

1. **Use HTTPS** - Always use secure connections
2. **Handle errors gracefully** - Provide meaningful error messages
3. **Implement retries** - Retry failed requests with exponential backoff
4. **Cache responses** - Reduce unnecessary API calls
5. **Validate data** - Check data before sending to API
6. **Use appropriate methods** - GET for reading, POST for creating, etc.
7. **Version your API** - Include version in URL or headers
8. **Document everything** - Good documentation makes APIs usable

## Conclusion

Working with REST APIs is a fundamental skill for modern web development. By understanding HTTP methods, proper error handling, authentication, and best practices, you can build robust applications that communicate effectively with backend services.

Remember to always handle errors gracefully, respect rate limits, and write clean, maintainable code that other developers can understand and use!
