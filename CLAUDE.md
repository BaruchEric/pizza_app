# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Flutter pizza delivery app built with Firebase and BLoC pattern. The app features authentication, pizza browsing, and follows clean architecture principles with separate repository packages.

## Development Commands

### Build and Run
```bash
# Run the app in development mode
flutter run

# Run on specific device
flutter run -d chrome      # Web
flutter run -d macos       # macOS
flutter run -d ios         # iOS simulator

# Build for production
flutter build apk          # Android APK
flutter build ios          # iOS
flutter build web          # Web
flutter build macos        # macOS
```

### Testing and Quality
```bash
# Run tests
flutter test

# Run tests for specific package
flutter test packages/user_repository/
flutter test packages/pizza_repository/

# Analyze code
flutter analyze

# Format code
flutter format .

# Check dependencies
flutter pub deps
```

### Package Management
```bash
# Get dependencies (run from root)
flutter pub get

# Get dependencies for packages
cd packages/user_repository && flutter pub get
cd packages/pizza_repository && flutter pub get

# Clean and reinstall
flutter clean && flutter pub get
```

### Firebase Setup
```bash
# Firebase login and setup
firebase login
firebase init

# Deploy Firebase rules/functions (if configured)
firebase deploy
```

## Architecture Overview

### Project Structure
- **Main App** (`lib/`): Contains the Flutter app with BLoC state management
- **Repository Packages** (`packages/`): Separate packages for business logic and data access
- **Firebase Integration**: Authentication, Firestore, and Storage
- **BLoC Pattern**: State management throughout the app

### Key Architecture Components

#### Repository Pattern
The app follows a clean architecture with repository abstractions:
- `UserRepository` (abstract) → `FirebaseUserRepo` (implementation)
- `PizzaRepo` (abstract) → `FirebasePizzaRepo` (implementation)

Both repositories are in separate packages with their own models and entities.

#### State Management
Uses BLoC pattern with these main blocs:
- `AuthenticationBloc`: Manages user authentication state
- `SignInBloc` / `SignUpBloc`: Handle authentication flows
- `GetPizzaBloc`: Manages pizza data fetching

#### Navigation Structure
- **Unauthenticated**: `WelcomeScreen` → Sign In/Up flows
- **Authenticated**: `HomeScreen` with pizza browsing and details

### Package Dependencies

#### Main App Dependencies
- `bloc: ^8.1.2` & `flutter_bloc: ^8.1.3`: State management
- `firebase_core: ^2.16.0`: Firebase initialization
- `equatable: ^2.0.5`: Value equality
- `font_awesome_flutter: ^10.6.0`: Icons

#### Repository Packages
Both packages use:
- `firebase_auth: ^4.16.0` (user_repository only)
- `cloud_firestore: ^4.9.2`: Database
- `firebase_storage: ^11.6.0`: File storage
- `rxdart: ^0.27.7`: Stream utilities

### Data Models

#### User Repository
- `MyUser`: User model with authentication data
- `UserEntity`: Firestore entity representation
- Abstract `UserRepository` with Firebase implementation

#### Pizza Repository  
- `Pizza`: Pizza model with nutritional data
- `Macros`: Nutritional information model
- `PizzaEntity` & `MacrosEntity`: Firestore entities
- Abstract `PizzaRepo` with Firebase implementation

### Firebase Configuration
- Uses FlutterFire for Firebase integration
- Requires `google-services.json` (Android) and `GoogleService-Info.plist` (iOS/macOS)
- Firebase configuration files are gitignored for security

## Development Guidelines

### Working with Packages
When modifying repository packages:
1. Navigate to the specific package directory
2. Make changes to models, entities, or repository implementations
3. Run tests from the package directory
4. Update exports in the main library file if adding new classes

### State Management Patterns
- Use BLoC for all state management
- Keep business logic in repository packages
- UI screens should only trigger events and react to state changes
- Use `MultiBlocProvider` when multiple blocs are needed

### Firebase Integration
- All Firebase operations are abstracted through repository interfaces
- Entity classes handle Firestore serialization
- Use streams for real-time data updates
- Implement proper error handling for network operations

### Testing Strategy
- Test repository implementations with mock Firebase services
- Test BLoC state changes and event handling
- Test UI widgets in isolation when possible
- Run tests at both package and app levels