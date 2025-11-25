# Project 2

## Project Overview

Project 2 is the **advanced continuation** of your Project 1 application. You will take the same project theme and code you chose for Project 1 (Notes App, Task Manager, Recipe App, Book Library, or Expense Tracker) and significantly enhance it with professional-grade features including:

- Multi-provider authentication (email + social login)
- Advanced Firestore data modeling with user-specific collections
- Firebase Storage integration for media uploads
- Native device features (camera, GPS, biometrics, etc.)
- Deeper REST API integration with workflows
- Clean architecture with separation of concerns
- Production-ready security rules

This project simulates real-world app development where you iterate on an existing codebase, adding enterprise-level features while maintaining code quality and user experience.

---

## Learning Objectives

By completing this project, you will demonstrate mastery of:

1. **Multi-Provider Authentication:** Implement Firebase Authentication with both email/password and at least one social provider (Google, Apple, or GitHub)
2. **Advanced Firestore Design:** Structure data with user-specific collections, subcollections, and proper security isolation
3. **Firebase Storage Integration:** Upload, store, and retrieve media files (images, documents) with proper user access control
4. **Native Device Features:** Integrate at least 2 native capabilities (camera, gallery, GPS, biometric authentication, QR scanning, notifications, sensors)
5. **Deep API Integration:** Extend REST API usage beyond basic fetching to include workflows, data enrichment, and feature integration
6. **State Management at Scale:** Implement Provider or Riverpod for app-wide state with proper separation between business logic and UI
7. **Clean Architecture:** Organize code with clear separation of services, repositories, providers, models, screens, and widgets
8. **Production Security:** Write and deploy Firebase Security Rules protecting user data, storage, and preventing unauthorized access

---

## Mandatory Requirements

All students must implement the following core features regardless of theme:

### 1. Firebase Authentication (Required)

**Email/Password Authentication:**
- User registration with email validation
- Login with email and password
- Password reset functionality
- Email verification (optional but recommended)

**Social Login (Choose at least ONE):**
- Google Sign-In
- Apple Sign-In (iOS recommended)
- GitHub OAuth

**Requirements:**
- Persistent login sessions across app restarts
- Proper error handling with user-friendly messages
- Profile screen showing auth provider information
- Sign out functionality
- Handle auth state changes with StreamBuilder

**Example Structure:**
```dart
// lib/services/auth_service.dart
class AuthService {
  Future<UserCredential?> signUpWithEmail(String email, String password);
  Future<UserCredential?> signInWithEmail(String email, String password);
  Future<UserCredential?> signInWithGoogle();
  Future<void> sendPasswordReset(String email);
  Future<void> signOut();
  Stream<User?> get authStateChanges;
}
```

### 2. Advanced Firestore Data Modeling (Required)

**User-Specific Data:**
- All user data must be isolated by UID
- Use subcollections or compound queries for complex data relationships
- Implement proper Firestore queries (where, orderBy, limit)

**Required Collections Structure:**
```
users/
  {userId}/
    profile: {...}
    items/           // Your main data (notes, tasks, recipes, etc.)
      {itemId}/
        ...fields
    settings/        // User preferences
      {settingId}/
        ...preferences
```

**Firestore Operations:**
- Real-time listeners with StreamBuilder
- Batch writes for multiple operations
- Transactions for critical updates
- Pagination for large datasets (optional but recommended)

The structure is a proposal and not mandatory, if you choose to model at your will, it needs to fulfill the requirements.

**Example:**
```dart
// lib/services/firestore_service.dart
class FirestoreService {
  Stream<List<Item>> getUserItems(String userId);
  Future<void> addItem(String userId, Item item);
  Future<void> updateItem(String userId, String itemId, Map<String, dynamic> updates);
  Future<void> deleteItem(String userId, String itemId);
  Future<List<Item>> searchItems(String userId, String query);
}
```

### 3. Firebase Storage (Required)

**File Upload Requirements:**
- Upload images (profile pictures, item images, attachments)
- Store files in user-specific directories
- Retrieve and display download URLs in UI
- Delete files when associated data is removed

**Storage Organization:**
```
users/
  {userId}/
    profile/
      avatar.jpg
    items/
      {itemId}/
        image1.jpg
        image2.jpg
```

**Requirements:**
- Progress indicators during uploads
- Image compression before upload
- Error handling for failed uploads
- Proper cleanup of orphaned files

**Example:**
```dart
// lib/services/storage_service.dart
class StorageService {
  Future<String?> uploadImage(String path, File file);
  Future<void> deleteImage(String path);
  Future<List<String>> getUserImages(String userId);
}
```

### 4. Native Device Features (Choose at least 2)

Select and implement at least **TWO** of the following:

**Option A: Camera & Gallery**
- Take photos using device camera
- Select images from gallery
- Display captured/selected images
- Required packages: `image_picker`, `camera`

**Option B: GPS & Location**
- Access user's current location
- Display location on map
- Save location data with items
- Required packages: `geolocator`, `google_maps_flutter`

**Option C: Biometric Authentication**
- Fingerprint or Face ID lock
- Secure app sections with biometrics
- Fallback to PIN/password
- Required packages: `local_auth`

**Option D: QR Code Scanner**
- Scan QR codes
- Generate QR codes for sharing
- Parse and validate scanned data
- Required packages: `qr_code_scanner`, `qr_flutter`

**Option E: Push Notifications**
- Local notifications for reminders
- Firebase Cloud Messaging integration
- Scheduled notifications
- Required packages: `flutter_local_notifications`, `firebase_messaging`

**Option F: Device Sensors**
- Access accelerometer, gyroscope, or other sensors
- Use sensor data in app functionality
- Required packages: `sensors_plus`

**Option G: Voice/Audio Recording**
- Record audio notes or voice memos
- Play back recordings
- Store audio files in Firebase Storage
- Required packages: `record`, `audioplayers`

### 5. REST API Integration (Required)

Extend your Project 1 API usage with deeper integration:

**Requirements:**
- Use the same API category from Project 1 (quotes, nutrition, books, weather, currency, etc.)
- Implement at least 3 different API endpoints
- Integrate API data into user workflows (not just display)
- Cache API responses locally
- Handle rate limits and errors gracefully

**Examples by Theme:**
- **Notes App:** Fetch inspirational quotes, save to notes, search quotes
- **Task Manager:** Weather API to suggest outdoor task timing
- **Recipe App:** Nutrition API for ingredient analysis, meal planning
- **Book App:** Google Books API for search, details, and recommendations
- **Expense Tracker:** Currency conversion API for multi-currency support

**Example Structure:**
```dart
// lib/services/api_service.dart
class ApiService {
  Future<List<ApiData>> fetchData(String query);
  Future<ApiData> getDetails(String id);
  Future<List<ApiData>> getRecommendations(String category);
}
```

### 6. State Management with Provider (Required)

**Requirements:**
- Use Provider for app-wide state
- Separate business logic from UI components
- Implement at least 3 providers:
  - AuthProvider (user authentication state)
  - DataProvider (main app data - notes, tasks, etc.)
  - SettingsProvider (user preferences, theme)

**Example Structure:**
```dart
// lib/providers/auth_provider.dart
class AuthProvider extends ChangeNotifier {
  User? _user;
  User? get user => _user;
  
  Future<void> signIn(String email, String password);
  Future<void> signOut();
}

// lib/providers/data_provider.dart
class DataProvider extends ChangeNotifier {
  List<Item> _items = [];
  List<Item> get items => _items;
  
  Future<void> fetchItems();
  Future<void> addItem(Item item);
  Future<void> updateItem(Item item);
  Future<void> deleteItem(String itemId);
}
```

### 7. Clean Architecture (Required)

Organize your project with clear separation of concerns:

```
lib/
├── main.dart
├── models/              # Data models
│   ├── user_model.dart
│   └── item_model.dart
├── services/            # Business logic & external services
│   ├── auth_service.dart
│   ├── firestore_service.dart
│   ├── storage_service.dart
│   └── api_service.dart
├── providers/           # State management
│   ├── auth_provider.dart
│   ├── data_provider.dart
│   └── settings_provider.dart
├── repositories/        # Data layer abstraction (optional but recommended)
│   ├── auth_repository.dart
│   └── data_repository.dart
├── screens/             # Full-page views
│   ├── auth/
│   │   ├── login_screen.dart
│   │   └── register_screen.dart
│   ├── home/
│   │   └── home_screen.dart
│   └── profile/
│       └── profile_screen.dart
├── widgets/             # Reusable UI components
│   ├── custom_button.dart
│   ├── loading_indicator.dart
│   └── error_widget.dart
└── utils/               # Helper functions, constants
    ├── constants.dart
    ├── validators.dart
    └── helpers.dart
```

## Project Requirements by Theme

Each theme has specific advanced requirements building on Project 1:

### 📝 Notes App Advanced Requirements

**Core Enhancements:**
1. **Rich Text Editor:** Format text (bold, italic, lists, headers)
2. **Image Attachments:** Upload multiple images per note via Firebase Storage
3. **Voice Notes:** Record and attach audio to notes
4. **Location Tagging:** Save GPS coordinates with notes, display on map
5. **Biometric Lock:** Secure sensitive notes with fingerprint/Face ID
6. **Quotes API Integration:**
   - Search and insert quotes into notes
   - Daily quote feature on home screen
   - Save favorite quotes as quick notes

**Data Structure:**
```
users/{userId}/notes/{noteId}/
  - title: string
  - content: string (rich text/markdown)
  - imageUrls: array
  - audioUrl: string (optional)
  - location: geopoint (optional)
  - locked: boolean
  - tags: array
  - createdAt: timestamp
  - updatedAt: timestamp
```

**Native Features Example:**
- Camera/Gallery for image uploads
- GPS for location tagging
- Biometric authentication for locked notes
- (Optional) Voice recording for audio notes

### ✅ Task Manager Advanced Requirements

**Core Enhancements:**
1. **Task Categories/Projects:** Organize tasks into projects
2. **Subtasks:** Break tasks into smaller subtasks with progress tracking
3. **File Attachments:** Attach documents, images to tasks
4. **Location-Based Reminders:** Get notified when near task location
5. **Collaborative Tasks:** Share tasks with other users (Firestore subcollections)
6. **Weather Integration:**
   - Display weather for outdoor tasks
   - Suggest best time for outdoor activities
   - Automatic task rescheduling based on weather

**Data Structure:**
```
users/{userId}/projects/{projectId}/
  - name: string
  - color: string
  
users/{userId}/tasks/{taskId}/
  - title: string
  - description: string
  - projectId: string
  - dueDate: timestamp
  - priority: string (low/medium/high)
  - completed: boolean
  - location: geopoint (optional)
  - attachments: array of storage URLs
  - subtasks: array
  - weather: object (optional)
  - createdAt: timestamp
```

**Native Features Example:**
- Push notifications for task reminders
- GPS for location-based reminders
- Camera/Gallery for attachments
- (Optional) Calendar integration

### 🍳 Recipe App Advanced Requirements

**Core Enhancements:**
1. **Recipe Photos:** Multiple images per recipe (steps, final dish)
2. **Video Tutorials:** Upload cooking videos to Firebase Storage
3. **Ingredient Scanner:** QR/barcode scanner for adding ingredients
4. **Meal Planning Calendar:** Plan weekly meals with recipes
5. **Shopping List Generator:** Auto-generate list from recipe ingredients
6. **Nutrition API Integration:**
   - Calculate nutritional information per recipe
   - Suggest recipes based on dietary preferences
   - Ingredient substitutions
   - Meal recommendations based on available ingredients

**Data Structure:**
```
users/{userId}/recipes/{recipeId}/
  - title: string
  - description: string
  - ingredients: array
  - instructions: array
  - imageUrls: array
  - videoUrl: string (optional)
  - prepTime: number
  - cookTime: number
  - servings: number
  - category: string
  - nutrition: object (from API)
  - tags: array
  - isFavorite: boolean
  - createdAt: timestamp
  
users/{userId}/mealPlans/{date}/
  - breakfast: recipeId
  - lunch: recipeId
  - dinner: recipeId
  
users/{userId}/shoppingLists/{listId}/
  - items: array
  - checked: array
  - createdAt: timestamp
```

**Native Features Example:**
- Camera for recipe photos
- QR/Barcode scanner for ingredients
- Push notifications for meal reminders
- (Optional) GPS for nearby grocery stores


### 📚 Book Library Advanced Requirements

**Core Enhancements:**
1. **Book Cover Upload:** Take photos or upload cover images
2. **Barcode Scanner:** Scan ISBN to auto-fetch book details
3. **Reading Progress:** Track pages read, reading sessions
4. **Book Locations:** Mark where physical books are stored (shelf, room)
5. **Book Sharing:** Share book recommendations with friends
6. **Google Books API Integration:**
   - Search books by title, author, ISBN
   - Auto-fill book details from API
   - Fetch cover images
   - Get book descriptions and ratings
   - Discover similar books

**Data Structure:**
```
users/{userId}/books/{bookId}/
  - isbn: string
  - title: string
  - author: string
  - coverUrl: string (Firebase Storage or API)
  - genre: string
  - pages: number
  - currentPage: number
  - status: string (want/reading/finished)
  - rating: number
  - review: string
  - location: string (physical location)
  - purchaseDate: timestamp
  - apiData: object (from Google Books)
  - createdAt: timestamp
  
users/{userId}/readingSessions/{sessionId}/
  - bookId: string
  - startPage: number
  - endPage: number
  - duration: number
  - date: timestamp
```

**Native Features Example:**
- Camera for book cover photos
- QR/Barcode scanner for ISBN
- Push notifications for reading reminders
- (Optional) GPS for library/bookstore locations


### 💰 Expense Tracker Advanced Requirements

**Core Enhancements:**
1. **Receipt Photos:** Attach receipt images to expenses
2. **Location Tracking:** Auto-tag expense location
3. **Budget Categories:** Set category budgets with alerts
4. **Recurring Expenses:** Auto-create monthly bills
5. **Export Reports:** Generate PDF/CSV expense reports
6. **Currency API Integration:**
   - Multi-currency support
   - Real-time exchange rates
   - Convert expenses between currencies
   - Display totals in preferred currency
   - Historical exchange rate charts

**Data Structure:**
```
users/{userId}/expenses/{expenseId}/
  - amount: number
  - currency: string
  - convertedAmount: number (in base currency)
  - category: string
  - description: string
  - date: timestamp
  - location: geopoint (optional)
  - receiptUrl: string (Firebase Storage)
  - isRecurring: boolean
  - recurringFrequency: string (optional)
  - tags: array
  - createdAt: timestamp
  
users/{userId}/budgets/{categoryId}/
  - category: string
  - limit: number
  - spent: number
  - period: string (weekly/monthly)
  
users/{userId}/settings/
  - baseCurrency: string
  - exchangeRates: object (cached from API)
  - budgetAlerts: boolean
```

**Native Features Example:**
- Camera for receipt photos
- GPS for expense location
- Push notifications for budget alerts
- (Optional) Biometric auth for sensitive data

## Architecture Expectations

### Minimum Project Structure

```
project_root/
├── lib/
│   ├── main.dart
│   ├── models/
│   │   ├── user_model.dart
│   │   ├── item_model.dart
│   │   └── api_model.dart
│   ├── services/
│   │   ├── auth_service.dart
│   │   ├── firestore_service.dart
│   │   ├── storage_service.dart
│   │   ├── api_service.dart
│   │   └── native_service.dart (camera, GPS, etc.)
│   ├── providers/
│   │   ├── auth_provider.dart
│   │   ├── data_provider.dart
│   │   └── settings_provider.dart
│   ├── repositories/ (optional but recommended)
│   │   ├── auth_repository.dart
│   │   └── data_repository.dart
│   ├── screens/
│   │   ├── auth/
│   │   ├── home/
│   │   ├── detail/
│   │   ├── profile/
│   │   └── settings/
│   ├── widgets/
│   │   ├── common/
│   │   └── specific/
│   └── utils/
│       ├── constants.dart
│       ├── validators.dart
│       └── helpers.dart
├── assets/
│   ├── images/
│   └── icons/
├── firebase_rules/
│   ├── firestore.rules
│   └── storage.rules
└── README.md
```

### Separation of Concerns

**Services Layer:**
- Handles all external interactions (Firebase, APIs, device features)
- No UI code or state management
- Returns Futures or Streams
- Throws meaningful exceptions

**Providers Layer:**
- Manages app state using Provider/Riverpod
- Calls service methods
- Notifies listeners of state changes
- No direct Firebase or API calls

**Repositories Layer (Optional):**
- Abstracts data sources
- Combines multiple services
- Provides clean interface to providers

**Screens Layer:**
- Full-page views
- Minimal business logic
- Uses providers for data
- Handles navigation

**Widgets Layer:**
- Reusable UI components
- Stateless when possible
- Accepts data via parameters
- Emits events via callbacks


## Deliverables

Submit the following by the deadline:

### 1. GitHub Repository (Required)

**Repository Requirements:**
- Public repository on GitHub
- Clear commit history showing development progress
- Meaningful commit messages
- `.gitignore` excluding build files and secrets
- Branch structure (optional): main, develop, feature branches

**Repository Name Format:** `project2-[theme]-[your-name]`  
Example: `project2-notes-app-john-doe`

### 2. README.md (Required)

Your README must include:

**Section 1: Project Overview**
- App name and tagline
- Brief description (200-300 words)
- Link to demo video
- Screenshots (6-10 images showing key features)

**Section 2: Features List**
- All implemented features
- Native features used
- API integration description
- Firebase services utilized

**Section 3: Architecture**
- Folder structure explanation
- State management approach
- Design patterns used
- Architecture diagram (optional but recommended)

**Section 4: Setup Instructions**
- Prerequisites (Flutter version, Firebase account)
- Firebase setup steps
- How to run the app locally
- Environment configuration

**Section 5: Firebase Configuration**
- Firestore collections structure
- Storage organization
- Security rules explanation

**Section 6: API Documentation**
- Which APIs are used
- API endpoints utilized
- Example API responses

**Section 7: Technologies Used**
- Flutter version
- Key packages and versions
- Firebase services
- Native features

**Section 8: Challenges & Solutions**
- Technical challenges faced
- How you solved them
- What you learned

**Section 9: Future Enhancements**
- Features you'd add with more time
- Known limitations

**Section 10: Credits**
- APIs used
- Asset sources
- Tutorials referenced




## Bonus Features (Optional)

Implement any of these for **extra credit**:

### Bonus Feature 1: Dark Mode 
- System theme detection
- Manual theme toggle
- Consistent dark theme across all screens
- Smooth theme transitions

### Bonus Feature 2: Offline Support 
- Local caching with Hive or SharedPreferences
- Offline mode indicator
- Queue operations when offline
- Sync when connection restored

### Bonus Feature 3: Advanced Animations
- Custom page transitions
- Hero animations
- List item animations
- Loading animations

### Bonus Feature 4: Push Notifications 
- Firebase Cloud Messaging setup
- Background message handling
- Notification actions
- Scheduled notifications

### Bonus Feature 5: Multi-language Support
- At least 2 languages
- Language switcher in settings
- Proper localization files

### Bonus Feature 6: Advanced Search 
- Full-text search
- Filters and sorting
- Search history
- Search suggestions

### Bonus Feature 7: Data Analytics 
- Firebase Analytics integration
- Custom events tracking
- User behavior insights
- Crash reporting (Crashlytics)


**Good luck programming!**
