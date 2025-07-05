# Claude Code Project Index Creation Prompts

## Spring Boot Specific (Detailed)

```
Create lightweight Spring Boot index:

**Analyze:**
- @RestController, @Service, @Entity classes
- API endpoints (@GetMapping, @PostMapping, etc.)
- Key application.yml/properties settings
- Security configuration
- Database entities and relationships

**Output format:**
```json
{
  "framework": "Spring Boot",
  "api": [
    {"method": "GET", "path": "/api/users", "controller": "UserController", "desc": "Get user list"}
  ],
  "services": ["UserService", "AuthService"],
  "entities": ["User", "Role"],
  "config": {"port": 8080, "database": "mysql", "profile": "production"},
  "security": ["JWT", "OAuth2"]
}
```

**Limit:** Under 1500 tokens
**Output:** `spring_index.json`
**Language:** All JSON keys, values, and descriptions in English
```

## iOS Specific (Detailed)

```
Create lightweight iOS index:

**Analyze:**
- ViewControllers, SwiftUI views
- Data models (struct, class, Core Data entities)
- API client calls and networking
- Navigation structure (Storyboard/SwiftUI)
- App delegate and scene delegate

**Output format:**
```json
{
  "platform": "iOS",
  "views": ["UserListViewController", "UserDetailViewController"],
  "models": ["User", "AuthResponse", "UserProfile"],
  "apis": ["/api/users", "/api/auth/login", "/api/profile"],
  "navigation": {
    "root": "TabBarController", 
    "tabs": ["UserList", "Settings", "Profile"]
  },
  "networking": "URLSession"
}
```

**Limit:** Under 1500 tokens
**Output:** `ios_index.json`
**Language:** All JSON keys, values, and descriptions in English
```

## Android Specific (Detailed)

```
Create lightweight Android index:

**Analyze:**
- Activities, Fragments, Compose screens
- Data models and Room entities
- API service interfaces (Retrofit, etc.)
- Navigation components
- ViewModels and repositories

**Output format:**
```json
{
  "platform": "Android",
  "screens": ["MainActivity", "UserListFragment", "UserDetailActivity"],
  "models": ["User", "AuthResponse", "UserEntity"],
  "apis": ["/api/users", "/api/auth/login", "/api/profile"],
  "navigation": {
    "main": "MainActivity",
    "fragments": ["UserListFragment", "SettingsFragment"]
  },
  "networking": "Retrofit"
}
```

**Limit:** Under 1500 tokens
**Output:** `android_index.json`
**Language:** All JSON keys, values, and descriptions in English
```

## Universal/Other Projects (Auto-Adaptive)

```
Analyze this project and create an appropriate lightweight index following the patterns above:

**Instructions:**
1. Identify the project type/framework automatically
2. Follow the same structure pattern as Spring Boot/iOS/Android examples above
3. Extract the most important architectural elements for this specific technology
4. Adapt the JSON format to match the project's patterns (controllers→handlers, views→components, etc.)
5. Focus on: main structural files, API/routing, data models, configuration

**Examples of what to look for by project type:**
- Frontend frameworks: Components, routes, services, state management
- Backend frameworks: Routes/endpoints, controllers/handlers, models, middleware
- Desktop apps: Windows/forms, services, configuration
- Scripts/tools: Main functions, modules, configuration

**Output format:** Follow Spring Boot/iOS/Android JSON structure but adapted to your framework
**Limit:** Under 1500 tokens
**Output:** `project_index.json`
**Language:** All JSON keys, values, and descriptions in English

**Important:** Mimic the detail level and structure of the Spring Boot/iOS/Android examples above, but adapt field names and content to match your detected framework.
```

## Multi-Project Workspace

```
Detect all project types in this workspace and create appropriate indexes:

**For each detected project:**
- Use detailed prompts above for Spring Boot/iOS/Android
- Use universal prompt for other project types
- Create `api_map.json` if multiple projects communicate

**API mapping format:**
```json
{
  "GET /api/users": {
    "backend": "UserController.getUsers",
    "frontend": "UserService.fetchUsers",
    "mobile": "UserRepository.getUsers",
    "description": "Retrieve user list"
  }
}
```

**Language:** All JSON content must be in English only
```

## CLAUDE.md Configuration

```markdown
# Claude Code Configuration

## Required File Loading

**ALWAYS read these index files FIRST (if they exist):**

- `spring_index.json` - Spring Boot structure
- `ios_index.json` - iOS structure
- `android_index.json` - Android structure
- `project_index.json` - Other project types
- `api_map.json` - API mapping between projects

## Usage

1. **Before code analysis** - Reference relevant index files
2. **When suggesting changes** - Check consistency with index
3. **When adding features** - Suggest index updates
4. **Multi-project work** - Use api_map.json for cross-project understanding

## Index Updates

If index is outdated, regenerate with appropriate prompt above

## Notes

- Index files are lightweight - check actual code for details
- Always update index after major structural changes
- Spring Boot/iOS/Android use detailed templates
- Other projects use adaptive universal template
```

## Usage Guidelines

1. **Start with specific prompts** - Use Spring Boot/iOS/Android detailed prompts when applicable
2. **Fall back to universal** - Use adaptive prompt for other technologies
3. **Regular updates** - Regenerate after major changes
4. **Token monitoring** - Keep under specified limits
5. **English only** - All JSON content in English
6. **CLAUDE.md placement** - Place in project root