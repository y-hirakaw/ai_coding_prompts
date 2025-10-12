# Claude Code Project Index Creation Prompts v2.0

## Overview

This document provides prompts for creating lightweight, modular project indexes that Claude Code can use to understand project structure efficiently.

**Key Improvements:**
- File splitting strategy (1500 token limit per file)
- Version tracking in all index files
- Manifest file for navigation
- Infrastructure and error handling coverage
- Dependency and test structure documentation

---

## File Naming Convention

```
{framework}_{category}.json

Examples:
- spring_api.json
- spring_entities.json
- spring_security.json
- ios_views.json
- android_screens.json
```

---

## Spring Boot Modular Index

```
Create modular Spring Boot index with file splitting:

**Analysis Targets:**
1. API endpoints (controllers, mappings)
2. Entities and database schema
3. Services and business logic
4. Security (authentication, authorization)
5. Configuration (application.yml, env vars)
6. Dependencies (major libraries, versions)
7. Error handling (global handlers, exceptions)
8. Testing structure
9. Infrastructure (Docker, deployment)

**File Strategy:**
- If category > 1500 tokens → Create separate file
- If category < 1500 tokens → Can combine into main index
- Each file must have version and lastUpdated fields

**Output Files:**

### 1. `spring_api.json` - API Endpoints
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "endpoints": [
    {
      "method": "GET",
      "path": "/api/users",
      "controller": "UserController",
      "handler": "getUsers",
      "params": ["page", "size"],
      "response": "Page<UserDTO>",
      "auth": "JWT required",
      "description": "Get paginated user list"
    }
  ]
}
```

### 2. `spring_entities.json` - Database Models
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "entities": [
    {
      "name": "User",
      "table": "users",
      "fields": [
        {"name": "id", "type": "Long", "pk": true},
        {"name": "email", "type": "String", "unique": true, "nullable": false}
      ],
      "relationships": [
        {"type": "OneToMany", "target": "Role", "field": "roles"}
      ],
      "indexes": ["idx_email"]
    }
  ],
  "database": {
    "type": "MySQL",
    "version": "8.0",
    "migrations": "Flyway",
    "schema": "app_db"
  }
}
```

### 3. `spring_services.json` - Business Logic
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "services": [
    {
      "name": "UserService",
      "methods": ["createUser", "updateUser", "deleteUser"],
      "dependencies": ["UserRepository", "EmailService"],
      "transactions": true
    }
  ],
  "repositories": [
    {
      "name": "UserRepository",
      "type": "JpaRepository",
      "entity": "User",
      "customQueries": ["findByEmailContaining"]
    }
  ]
}
```

### 4. `spring_security.json` - Security Configuration
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "authentication": {
    "type": "JWT",
    "tokenExpiry": "24h",
    "refreshToken": true,
    "provider": "JwtAuthenticationProvider"
  },
  "authorization": {
    "method": "Role-based",
    "roles": ["ADMIN", "USER", "GUEST"],
    "permissions": ["READ", "WRITE", "DELETE"]
  },
  "cors": {
    "enabled": true,
    "allowedOrigins": ["https://example.com"],
    "allowedMethods": ["GET", "POST", "PUT", "DELETE"],
    "allowedHeaders": ["Authorization", "Content-Type"]
  },
  "csrf": {
    "enabled": false,
    "reason": "JWT-based API"
  }
}
```

### 5. `spring_config.json` - Configuration
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "application": {
    "port": 8080,
    "contextPath": "/api",
    "profile": "production"
  },
  "requiredEnvVars": [
    {"name": "DATABASE_URL", "description": "MySQL connection string"},
    {"name": "JWT_SECRET", "description": "Secret key for JWT signing"},
    {"name": "AWS_ACCESS_KEY", "description": "AWS S3 access key"}
  ],
  "optionalEnvVars": [
    {"name": "LOG_LEVEL", "default": "INFO"}
  ],
  "features": {
    "caching": "Redis",
    "logging": "Logback",
    "monitoring": "Spring Actuator",
    "apiDocs": "Swagger/OpenAPI"
  }
}
```

### 6. `spring_dependencies.json` - Key Libraries
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "framework": {
    "springBoot": "3.2.0",
    "java": "17"
  },
  "core": [
    {"name": "spring-boot-starter-web", "version": "3.2.0"},
    {"name": "spring-boot-starter-data-jpa", "version": "3.2.0"},
    {"name": "spring-boot-starter-security", "version": "3.2.0"}
  ],
  "database": [
    {"name": "mysql-connector-java", "version": "8.0.33"}
  ],
  "utilities": [
    {"name": "lombok", "version": "1.18.30"},
    {"name": "mapstruct", "version": "1.5.5"}
  ],
  "testing": [
    {"name": "spring-boot-starter-test", "version": "3.2.0"}
  ]
}
```

### 7. `spring_errors.json` - Error Handling
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "globalHandler": {
    "class": "GlobalExceptionHandler",
    "annotation": "@RestControllerAdvice"
  },
  "exceptions": [
    {
      "type": "ResourceNotFoundException",
      "httpStatus": 404,
      "handler": "handleResourceNotFound",
      "responseFormat": "ErrorResponse"
    },
    {
      "type": "ValidationException",
      "httpStatus": 400,
      "handler": "handleValidation",
      "responseFormat": "ValidationErrorResponse"
    }
  ],
  "validation": {
    "framework": "javax.validation",
    "annotations": ["@Valid", "@NotNull", "@Email"]
  },
  "responseFormat": {
    "standardError": {
      "timestamp": "ISO-8601",
      "status": "HTTP status code",
      "error": "Error type",
      "message": "User-friendly message",
      "path": "Request path"
    }
  }
}
```

### 8. `spring_tests.json` - Testing Structure
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "framework": "JUnit 5",
  "coverage": {
    "tool": "JaCoCo",
    "target": "80%",
    "excludes": ["**/config/**", "**/dto/**"]
  },
  "types": [
    {
      "type": "unit",
      "location": "src/test/java/unit",
      "annotations": ["@Test", "@Mock"]
    },
    {
      "type": "integration",
      "location": "src/test/java/integration",
      "annotations": ["@SpringBootTest", "@AutoConfigureMockMvc"]
    }
  ],
  "mocking": "Mockito",
  "testContainers": {
    "enabled": true,
    "containers": ["MySQL", "Redis"]
  }
}
```

### 9. `spring_infrastructure.json` - Infrastructure
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "docker": {
    "baseImage": "openjdk:17-alpine",
    "dockerfile": "Dockerfile",
    "compose": true,
    "composeFile": "docker-compose.yml",
    "services": ["app", "mysql", "redis"]
  },
  "deployment": {
    "platform": "AWS ECS",
    "ci": "GitHub Actions",
    "ciFile": ".github/workflows/deploy.yml",
    "environments": ["dev", "staging", "production"]
  },
  "monitoring": {
    "apm": "Datadog",
    "logs": "CloudWatch",
    "metrics": "Prometheus",
    "alerts": "PagerDuty"
  },
  "backup": {
    "database": "Automated daily snapshots",
    "retention": "30 days"
  }
}
```

### 10. `_index_manifest.json` - Manifest (Required)
```json
{
  "project": "Spring Boot API",
  "projectType": "Spring Boot",
  "indexVersion": "1.0.0",
  "lastUpdated": "2025-10-12",
  "files": [
    {"file": "spring_api.json", "category": "API endpoints", "tokens": 1200},
    {"file": "spring_entities.json", "category": "Database entities", "tokens": 800},
    {"file": "spring_services.json", "category": "Business logic", "tokens": 600},
    {"file": "spring_security.json", "category": "Security config", "tokens": 700},
    {"file": "spring_config.json", "category": "Configuration", "tokens": 500},
    {"file": "spring_dependencies.json", "category": "Dependencies", "tokens": 400},
    {"file": "spring_errors.json", "category": "Error handling", "tokens": 500},
    {"file": "spring_tests.json", "category": "Testing", "tokens": 450},
    {"file": "spring_infrastructure.json", "category": "Infrastructure", "tokens": 550}
  ],
  "totalTokens": 5700
}
```

**Index Creation Rules:**
1. Each file MUST have version and lastUpdated
2. If category exceeds 1500 tokens → separate file
3. If all categories < 3000 tokens combined → can use single spring_index.json
4. ALWAYS create _index_manifest.json
5. Use English for all content

**Token Limit:** 1500 tokens per file (except manifest)
**Language:** All JSON content in English
```

---

## iOS Modular Index

```
Create modular iOS index with file splitting:

**Analysis Targets:**
1. ViewControllers and SwiftUI views
2. Data models (struct, class, Core Data)
3. API client and networking
4. Navigation structure
5. Dependencies (CocoaPods/SPM)
6. Testing structure
7. App configuration

**Output Files:**

### 1. `ios_views.json`
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "viewControllers": [
    {
      "name": "UserListViewController",
      "type": "UITableViewController",
      "storyboard": "Main",
      "segues": ["showUserDetail"]
    }
  ],
  "swiftUIViews": [
    {
      "name": "UserListView",
      "type": "View",
      "stateObjects": ["@StateObject viewModel"]
    }
  ]
}
```

### 2. `ios_models.json`
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "models": [
    {
      "name": "User",
      "type": "struct",
      "codable": true,
      "properties": ["id: UUID", "email: String", "name: String"]
    }
  ],
  "coreData": {
    "enabled": true,
    "entities": ["UserEntity", "ProfileEntity"]
  }
}
```

### 3. `ios_api.json`
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "networking": "URLSession",
  "baseURL": "https://api.example.com",
  "endpoints": [
    {
      "method": "GET",
      "path": "/api/users",
      "response": "[User]",
      "client": "UserAPIClient.getUsers"
    }
  ],
  "authentication": {
    "type": "Bearer Token",
    "storage": "Keychain"
  }
}
```

### 4. `ios_navigation.json`
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "pattern": "TabBar + Navigation",
  "root": "TabBarController",
  "tabs": [
    {"title": "Users", "icon": "person", "root": "UserListViewController"},
    {"title": "Settings", "icon": "gear", "root": "SettingsViewController"}
  ],
  "deepLinks": {
    "enabled": true,
    "scheme": "myapp"
  }
}
```

### 5. `ios_dependencies.json`
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "manager": "Swift Package Manager",
  "packages": [
    {"name": "Alamofire", "version": "5.8.0"},
    {"name": "Kingfisher", "version": "7.10.0"}
  ],
  "ios": {
    "minimumVersion": "15.0",
    "targetVersion": "17.0"
  }
}
```

### 6. `ios_tests.json`
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "framework": "XCTest",
  "types": [
    {"type": "unit", "target": "AppTests"},
    {"type": "ui", "target": "AppUITests"}
  ],
  "coverage": {
    "enabled": true,
    "target": "75%"
  }
}
```

### 7. `ios_config.json`
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "bundleId": "com.example.app",
  "schemes": ["Debug", "Release"],
  "configurations": {
    "apiEndpoint": {
      "debug": "https://dev-api.example.com",
      "release": "https://api.example.com"
    }
  },
  "requiredPermissions": [
    "NSCameraUsageDescription",
    "NSLocationWhenInUseUsageDescription"
  ]
}
```

### 8. `_index_manifest.json`
```json
{
  "project": "iOS App",
  "projectType": "iOS",
  "indexVersion": "1.0.0",
  "lastUpdated": "2025-10-12",
  "files": [
    {"file": "ios_views.json", "category": "Views", "tokens": 1000},
    {"file": "ios_models.json", "category": "Models", "tokens": 600},
    {"file": "ios_api.json", "category": "API", "tokens": 800},
    {"file": "ios_navigation.json", "category": "Navigation", "tokens": 400},
    {"file": "ios_dependencies.json", "category": "Dependencies", "tokens": 300},
    {"file": "ios_tests.json", "category": "Tests", "tokens": 350},
    {"file": "ios_config.json", "category": "Configuration", "tokens": 400}
  ],
  "totalTokens": 3850
}
```

**Token Limit:** 1500 tokens per file
**Language:** All JSON content in English
```

---

## Android Modular Index

```
Create modular Android index with file splitting:

**Analysis Targets:**
1. Activities, Fragments, Compose screens
2. Data models and Room entities
3. API service interfaces
4. Navigation components
5. Dependencies (Gradle)
6. Testing structure
7. App configuration

**Output Files:**

### 1. `android_screens.json`
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "activities": [
    {
      "name": "MainActivity",
      "layout": "activity_main",
      "viewModel": "MainViewModel"
    }
  ],
  "fragments": [
    {
      "name": "UserListFragment",
      "layout": "fragment_user_list",
      "viewModel": "UserListViewModel"
    }
  ],
  "composeScreens": [
    {
      "name": "UserListScreen",
      "route": "user_list",
      "viewModel": "UserListViewModel"
    }
  ]
}
```

### 2. `android_models.json`
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "dataClasses": [
    {
      "name": "User",
      "package": "com.example.model",
      "properties": ["id: Long", "email: String", "name: String"]
    }
  ],
  "roomEntities": [
    {
      "name": "UserEntity",
      "tableName": "users",
      "primaryKey": "id",
      "indices": ["email"]
    }
  ],
  "database": {
    "name": "AppDatabase",
    "version": 1,
    "entities": ["UserEntity", "ProfileEntity"]
  }
}
```

### 3. `android_api.json`
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "networking": "Retrofit",
  "baseURL": "https://api.example.com",
  "endpoints": [
    {
      "method": "GET",
      "path": "/api/users",
      "response": "List<User>",
      "service": "UserApiService.getUsers"
    }
  ],
  "authentication": {
    "type": "Bearer Token",
    "interceptor": "AuthInterceptor"
  },
  "serialization": "Gson"
}
```

### 4. `android_navigation.json`
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "type": "Navigation Component",
  "graph": "nav_graph.xml",
  "destinations": [
    {"id": "userListFragment", "type": "Fragment"},
    {"id": "userDetailFragment", "type": "Fragment"}
  ],
  "deepLinks": {
    "enabled": true,
    "scheme": "myapp"
  }
}
```

### 5. `android_dependencies.json`
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "gradle": "8.0",
  "kotlin": "1.9.0",
  "core": [
    {"name": "androidx.core:core-ktx", "version": "1.12.0"},
    {"name": "androidx.lifecycle:lifecycle-viewmodel-ktx", "version": "2.6.2"}
  ],
  "networking": [
    {"name": "com.squareup.retrofit2:retrofit", "version": "2.9.0"}
  ],
  "database": [
    {"name": "androidx.room:room-runtime", "version": "2.6.0"}
  ],
  "ui": [
    {"name": "androidx.compose.ui:ui", "version": "1.5.4"}
  ],
  "minSdk": 24,
  "targetSdk": 34,
  "compileSdk": 34
}
```

### 6. `android_tests.json`
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "framework": "JUnit 4",
  "types": [
    {
      "type": "unit",
      "location": "src/test/java",
      "framework": "JUnit + Mockito"
    },
    {
      "type": "instrumented",
      "location": "src/androidTest/java",
      "framework": "Espresso"
    }
  ],
  "coverage": {
    "enabled": true,
    "tool": "JaCoCo"
  }
}
```

### 7. `android_config.json`
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "applicationId": "com.example.app",
  "buildTypes": ["debug", "release"],
  "flavors": ["dev", "prod"],
  "configurations": {
    "apiEndpoint": {
      "dev": "https://dev-api.example.com",
      "prod": "https://api.example.com"
    }
  },
  "requiredPermissions": [
    "INTERNET",
    "ACCESS_FINE_LOCATION",
    "CAMERA"
  ]
}
```

### 8. `_index_manifest.json`
```json
{
  "project": "Android App",
  "projectType": "Android",
  "indexVersion": "1.0.0",
  "lastUpdated": "2025-10-12",
  "files": [
    {"file": "android_screens.json", "category": "Screens", "tokens": 1100},
    {"file": "android_models.json", "category": "Models", "tokens": 700},
    {"file": "android_api.json", "category": "API", "tokens": 800},
    {"file": "android_navigation.json", "category": "Navigation", "tokens": 400},
    {"file": "android_dependencies.json", "category": "Dependencies", "tokens": 500},
    {"file": "android_tests.json", "category": "Tests", "tokens": 400},
    {"file": "android_config.json", "category": "Configuration", "tokens": 450}
  ],
  "totalTokens": 4350
}
```

**Token Limit:** 1500 tokens per file
**Language:** All JSON content in English
```

---

## Universal/Other Projects (Auto-Adaptive)

```
Analyze this project and create an appropriate modular index:

**Instructions:**
1. Identify the project type/framework automatically
2. Determine which categories are relevant (API, models, config, etc.)
3. Create appropriate {framework}_{category}.json files
4. Each file max 1500 tokens
5. Always create _index_manifest.json

**Common Categories to Consider:**
- api: Routes, endpoints, handlers
- models: Data structures, schemas
- services: Business logic, utilities
- config: Configuration, environment variables
- dependencies: Libraries, frameworks, versions
- errors: Error handling, validation
- tests: Test structure, coverage
- infrastructure: Deployment, Docker, CI/CD
- database: Schema, migrations (if applicable)
- auth: Authentication, authorization (if applicable)

**Examples by Project Type:**

**Node.js/Express:**
- express_api.json
- express_models.json
- express_middleware.json
- express_config.json
- express_dependencies.json

**Python/Django:**
- django_api.json
- django_models.json
- django_views.json
- django_config.json
- django_dependencies.json

**Python/FastAPI:**
- fastapi_api.json
- fastapi_models.json
- fastapi_dependencies.json
- fastapi_config.json

**.NET:**
- dotnet_api.json
- dotnet_models.json
- dotnet_services.json
- dotnet_config.json
- dotnet_dependencies.json

**Output format:** Follow Spring Boot/iOS/Android JSON structure patterns
**Token Limit:** 1500 tokens per file
**Language:** All JSON content in English
**Required:** _index_manifest.json with all files listed
```

---

## Multi-Project Workspace

```
Detect all project types in this workspace and create appropriate modular indexes:

**For each detected project:**
- Use Spring Boot prompt for Spring Boot projects
- Use iOS prompt for iOS projects
- Use Android prompt for Android projects
- Use Universal prompt for other project types

**Additional file for multi-project:**

### `_workspace_api_map.json`
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "projects": {
    "backend": "Spring Boot",
    "ios": "iOS",
    "android": "Android"
  },
  "apiMapping": [
    {
      "endpoint": "GET /api/users",
      "backend": {
        "project": "backend",
        "file": "UserController.java",
        "method": "getUsers"
      },
      "ios": {
        "project": "ios",
        "file": "UserAPIClient.swift",
        "method": "fetchUsers"
      },
      "android": {
        "project": "android",
        "file": "UserApiService.kt",
        "method": "getUsers"
      },
      "description": "Retrieve paginated user list"
    }
  ]
}
```

**Language:** All JSON content in English
**Structure:** Each project gets its own directory with manifest
```

---

## CLAUDE.md Configuration

```markdown
# Claude Code Configuration

## Index Structure

This project uses modular index files for efficient context loading.

### File Location

All index files are in: `.claude/workspace/project_index/`

### Index File Pattern

`{framework}_{category}.json`

Examples:
- `spring_api.json` - API endpoints
- `spring_entities.json` - Database entities
- `ios_views.json` - iOS views
- `android_screens.json` - Android screens

### Required File Loading

**ALWAYS check `_index_manifest.json` first to see available indexes.**

**Load relevant files based on task:**

| Task Type | Load These Files |
|-----------|------------------|
| API work | `*_api.json` |
| Database | `*_entities.json`, `*_models.json` |
| Security | `*_security.json` |
| Configuration | `*_config.json` |
| Dependencies | `*_dependencies.json` |
| Error handling | `*_errors.json` |
| Testing | `*_tests.json` |
| Infrastructure | `*_infrastructure.json` |

### Multi-Project Workspaces

If `_workspace_api_map.json` exists, load it for cross-project understanding.

## Index File Format

All index files contain:
```json
{
  "version": "1.0.0",
  "lastUpdated": "2025-10-12",
  "category-specific-content": "..."
}
```

## Usage Guidelines

1. **Before code analysis** - Check manifest, load relevant index files
2. **When suggesting changes** - Verify consistency with index
3. **When adding features** - Suggest index updates
4. **Multi-project work** - Use workspace API map

## Index Updates

**Regenerate indexes when:**
- Major structural changes occur
- New endpoints/models added
- Dependencies updated
- Security configuration changes
- Database schema modifications

**How to regenerate:**
1. Run appropriate creation prompt for your project type
2. Update `_index_manifest.json` with new token counts
3. Update `lastUpdated` field in all modified files
4. Increment `version` if structure changes

## Token Limits

- Maximum 1500 tokens per index file
- Manifest file has no token limit
- If category exceeds limit, split into sub-categories

## Notes

- Index files provide structure overview only
- Always check actual code for implementation details
- Indexes are lightweight and fast to load
- English only for all content
- Version tracking enables change detection
