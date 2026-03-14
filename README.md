# Generic API Framework with Playwright

A scalable API testing framework built with Playwright and TypeScript, designed for testing REST APIs with reusable components and clean architecture patterns.

## 🚀 Features

- **Clean Architecture**: Implements Page Object Model for APIs
- **Factory Pattern**: Easy API instance creation and management
- **Environment Configuration**: Support for multiple environments (dev, prod)
- **Structured Logging**: Built-in logging with Pino
- **Type Safety**: Full TypeScript support with data models
- **Validation**: Schema validation for API responses
- **Retry Logic**: Configurable retry mechanisms for flaky tests

## 📁 Project Structure

```
generic-api-framework/
├── apis/                    # API Page Objects
│   ├── baseApi.ts          # Base API class with common methods
│   └── userApi.ts          # User-specific API operations
├── config/                 # Configuration files
│   └── environments.ts     # Environment-specific settings
├── models/                 # Data models and types
│   └── user.ts            # User data model
├── tests/                  # Test files
│   ├── userApi.spec.ts     # User API tests
│   ├── userListApi.spec.ts # User list API tests
│   └── userListWithoutDataModels.spec.ts
├── utils/                  # Utility functions
│   ├── apiContext.ts       # API context management
│   ├── apiFactory.ts       # Factory for creating API instances
│   └── logger.ts          # Logging configuration
├── validators/             # Response validators
│   └── validators.ts       # Schema validation functions
├── package.json           # Project dependencies
├── playwright.config.ts   # Playwright configuration
└── README.md             # This file
```

## 🛠️ Prerequisites

- Node.js (version 14 or higher)
- npm or yarn package manager

## ⚙️ Installation

1. **Clone the repository** (if applicable):
   ```bash
   git clone <repository-url>
   cd generic-api-framework
   ```

2. **Install dependencies**:
   ```bash
   npm install
   ```

3. **Install Playwright browsers** (if needed):
   ```bash
   npx playwright install
   ```

## 🏃‍♂️ Quick Start

### Running Tests

1. **Run all tests**:
   ```bash
   npm test
   ```

2. **Run tests for specific environment**:
   ```bash
   # Development environment
   npm run test:dev
   
   # Production environment
   npm run test:prod
   ```

3. **Run specific test file**:
   ```bash
   npm run test:api:users
   ```

4. **Run tests with custom options**:
   ```bash
   npx playwright test --headed --project=api-tests
   ```

### Environment Configuration

The framework supports multiple environments. Configure them in `config/environments.ts`:

```typescript
// Example environment configuration
export const environments = {
  dev: {
    baseURL: 'https://dev-api.example.com',
    timeout: 30000,
    retries: 2
  },
  prod: {
    baseURL: 'https://api.example.com',
    timeout: 60000,
    retries: 1
  }
};
```

Set environment using:
```bash
TEST_ENV=dev npm test
```

## 📖 Usage Examples

### Creating API Tests

1. **Basic API Test**:
   ```typescript
   import { test, expect } from '@playwright/test';
   import { ApiFactory } from '../utils/apiFactory';
   import { UserApi } from '../apis/userApi';

   test.describe('User API Tests', () => {
     let userApi: UserApi;

     test.beforeAll(async ({ playwright }) => {
       userApi = ApiFactory.getUserApi(await playwright.request.newContext());
     });

     test('Should get user details', async () => {
       const response = await userApi.getUser(2);
       expect(response.ok()).toBeTruthy();
       
       const responseBody = await response.json();
       expect(responseBody.data.id).toBe(2);
     });
   });
   ```

2. **Creating a New API Class**:
   ```typescript
   // apis/productApi.ts
   import { BaseApi } from './baseApi';

   export class ProductApi extends BaseApi {
     async getProduct(id: number) {
       return this.apiRequest.get(`/products/${id}`);
     }

     async createProduct(productData: any) {
       return this.apiRequest.post('/products', { data: productData });
     }
   }
   ```

### Using Data Models

```typescript
// models/product.ts
export interface Product {
  id: number;
  name: string;
  price: number;
  category: string;
}

// In your test
const product: Product = responseBody.data;
expect(product.name).toBe('Expected Product Name');
```

## 🔧 Configuration

### Playwright Configuration

Key configuration options in `playwright.config.ts`:

- **Base URL**: Automatically set from environment configuration
- **Timeout**: Configurable per environment
- **HTTP Headers**: Common headers including API keys
- **Retries**: Environment-specific retry logic
- **Reporters**: HTML reporter for test results

### Logging

The framework uses Pino for structured logging:

```typescript
import { logger } from '../utils/logger';

// Log levels: trace, debug, info, warn, error, fatal
logger.info('Test started');
logger.error('Test failed', { error: errorDetails });
```

## 🧪 Test Patterns

### Positive Test Cases
- Valid data input testing
- Successful API responses (2xx status codes)
- Data validation and schema compliance

### Negative Test Cases
- Invalid input handling
- Error response validation (4xx, 5xx status codes)
- Edge cases and boundary testing

### Example Test Structure
```typescript
test.describe('API Endpoint Tests', () => {
  // Setup
  test.beforeAll(async ({ playwright }) => {
    // Initialize API instances
  });

  // Positive scenarios
  test('Should handle valid requests', async () => {
    // Test implementation
  });

  // Negative scenarios
  test('Should return 404 for non-existent resource', async () => {
    // Test implementation
  });

  // Cleanup
  test.afterAll(async () => {
    // Cleanup operations
  });
});
```

## 📊 Test Reports

After running tests, view the HTML report:
```bash
npx playwright show-report
```

The report includes:
- Test execution results
- Request/response details
- Timing information
- Screenshots (if configured)
- Trace files for debugging

## 🤝 Contributing

1. Follow the existing code structure and patterns
2. Add appropriate tests for new features
3. Update documentation for any new APIs or configurations
4. Use TypeScript for type safety
5. Follow the established logging patterns

## 📝 Available Scripts

- `npm test` - Run all tests
- `npm run test:dev` - Run tests in development environment
- `npm run test:prod` - Run tests in production environment
- `npm run test:api:users` - Run user API specific tests

## 🔍 Debugging

1. **Enable headed mode**:
   ```bash
   npx playwright test --headed
   ```

2. **Run in debug mode**:
   ```bash
   npx playwright test --debug
   ```

3. **View trace files**:
   ```bash
   npx playwright show-trace trace.zip
   ```

## 📚 Additional Resources

- [Playwright Documentation](https://playwright.dev/docs/intro)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [Pino Logging](https://github.com/pinojs/pino)

---

**Happy Testing! 🚀**
