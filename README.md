# Lab Report: Redux Shopping Cart Implementation

## Overview
This lab involved creating a React Native shopping cart application using Redux for state management. The project included product listing, cart functionality, and navigation between screens.

### 1. Product List Screen
![answer 2025-11-12 at 8 23 35 PM](https://github.com/user-attachments/assets/8ab13bd3-63f4-4c49-8e6d-ede1a664afd4)
*Figure 1: Main product listing screen showing all available products with "Add to Cart" buttons*

### 2. Product Added to Cart (Button State Change)
![answer 2025-11-12 at 8 23 42 PM](https://github.com/user-attachments/assets/4bfb45ae-1abd-449c-a2a4-32d06363ce72)
![answer 2025-11-12 at 8 23 49 PM](https://github.com/user-attachments/assets/bc031dc2-5438-415d-9f70-3f1b5c8488fc)
*Figure 2: Product card showing button state change from "Add to Cart" to "In Cart (1)" after adding item*

### 3. Cart Screen with Items
![answer 2025-11-12 at 8 24 16 PM](https://github.com/user-attachments/assets/0222df7d-19d9-4bd8-b767-ca129b86f761)
*Figure 3: Shopping cart screen displaying added items with quantities and total price*

### 4. Cart with Updated Quantities
![answer 2025-11-12 at 8 24 26 PM](https://github.com/user-attachments/assets/683b405c-6fa9-4947-a646-59b7e510001e)

*Figure 4: Cart screen showing updated quantities after adding multiple items or increasing quantities*

## Challenges Faced

### 1. Asset Resolution Issues
**Problem**: `Unable to resolve module ../../assets/placeholder.png` error
- **Root Cause**: Missing placeholder image file in assets directory
- **Impact**: Prevented app from loading ProductCard components
- **Error Message**: 
  ```
  ERROR Error: Unable to resolve module ../../assets/placeholder.png from 
  /Users/.../ShoppingCartRedux/src/components/ProductCard.js
  ```

### 2. Metro Configuration Problems
**Problem**: `missing-asset-registry-path` error in React Navigation assets
- **Root Cause**: Incomplete Metro bundler configuration for asset handling
- **Impact**: Navigation components couldn't load their icon assets
- **Error Message**:
  ```
  Error: Unable to resolve module missing-asset-registry-path from 
  .../node_modules/@react-navigation/elements/lib/module/assets/back-icon.png
  ```

### 3. Runtime Invariant Violations
**Problem**: "Runtime not ready" errors during app initialization
- **Root Cause**: Circular imports and improper module loading sequence
- **Impact**: App crashed on startup before reaching Redux components

### 4. Native Module Registration
**Problem**: "Verify that a module by this name is registered in native binary"
- **Root Cause**: React Navigation native dependencies not properly linked
- **Impact**: Navigation functionality completely broken

### 5. White Screen Issue
**Problem**: App loaded but showed blank white screen
- **Root Cause**: Missing Stack Navigator configuration in App.js
- **Impact**: No UI components rendered despite Redux store working

## Solutions Implemented

### 1. Asset Resolution Fix
```javascript
// Temporary solution: Removed defaultSource prop
<Image 
  source={{ uri: product.image }} 
  style={styles.image} 
/>
```
**Long-term solution**: Create `assets/` directory and add placeholder.png

### 2. Metro Configuration Update
```javascript
// Updated metro.config.js
const {getDefaultConfig, mergeConfig} = require('@react-native/metro-config');
const defaultConfig = getDefaultConfig(__dirname);

const config = {
  resolver: {
    assetExts: defaultConfig.resolver.assetExts.filter(ext => ext !== 'svg'),
    sourceExts: [...defaultConfig.resolver.sourceExts, 'svg'],
  },
};

module.exports = mergeConfig(defaultConfig, config);
```

### 3. Runtime Issues Resolution
```bash
# Clear Metro cache and restart
npx react-native start --reset-cache
rm -rf /tmp/metro-*
rm -rf node_modules/.cache

# Full clean when needed
rm -rf node_modules
npm install
```

### 4. Native Module Linking
```bash
# Reinstall and link navigation dependencies
npm install @react-navigation/native @react-navigation/stack
npm install react-native-screens react-native-safe-area-context
cd ios && pod install && cd ..
```

### 5. Complete App.js Setup
```javascript
import { NavigationContainer } from '@react-navigation/native';
import { createStackNavigator } from '@react-navigation/stack';
import React from 'react';
import { Provider } from 'react-redux';
import CartScreen from './src/screens/CartScreen';
import ProductListScreen from './src/screens/ProductListScreen';
import { store } from './src/store';

const Stack = createStackNavigator();

export default function App() {
  return (
    <Provider store={store}>
      <NavigationContainer>
        <Stack.Navigator
          initialRouteName="Products"
          screenOptions={{
            headerStyle: { backgroundColor: '#3498db' },
            headerTintColor: '#fff',
            headerTitleStyle: { fontWeight: 'bold' },
          }}
        >
          <Stack.Screen
            name="Products"
            component={ProductListScreen}
            options={{ title: 'Shop' }}
          />
          <Stack.Screen
            name="Cart"
            component={CartScreen}
            options={{ title: 'Shopping Cart' }}
          />
        </Stack.Navigator>
      </NavigationContainer>
    </Provider>
  );
}
```

## What I Learned About Redux

### 1. Store Architecture
- **Redux Toolkit Benefits**: `configureStore()` simplifies store setup compared to traditional Redux
- **Slice Pattern**: Using `createSlice()` combines actions, action creators, and reducers
- **State Structure**: Organized state into logical domains (products, cart)
- **Middleware Integration**: Built-in middleware for async actions and dev tools

### 2. State Management Patterns

#### Immutable Updates
```javascript
// Redux Toolkit uses Immer internally, allowing "mutative" syntax
addToCart: (state, action) => {
  const existingItem = state.items.find(item => item.id === action.payload.id);
  if (existingItem) {
    existingItem.quantity += 1;
  } else {
    state.items.push({ ...action.payload, quantity: 1 });
  }
}
```

#### Async Thunks
```javascript
// Handling async operations with createAsyncThunk
export const fetchProducts = createAsyncThunk(
  'products/fetchProducts',
  async () => {
    const response = await fetchProductsAPI();
    return response;
  }
);
```

### 3. Component Integration
- **useSelector Hook**: Accessing state in components
- **useDispatch Hook**: Dispatching actions from components
- **Provider Pattern**: Wrapping app with Redux Provider for state access

### 4. Debugging and Development
- **Redux DevTools**: Essential for tracking state changes and actions
- **Action Logging**: Understanding the flow of data through the application
- **Error Handling**: Proper error states in async thunks

### 5. Integration Challenges
- **React Native Specifics**: Metro bundler configuration affects Redux setup
- **Navigation Integration**: Redux state must work with React Navigation
- **TypeScript vs JavaScript**: Mixing type definitions caused runtime errors

## Key Takeaways

### Technical Insights
1. **Redux Toolkit** significantly reduces boilerplate compared to traditional Redux
2. **Proper project structure** is crucial for maintainable Redux applications
3. **Metro configuration** affects how modules are resolved and bundled
4. **Native dependencies** require proper linking in React Native projects

### Development Process
1. **Systematic debugging** is essential when multiple systems interact
2. **Clean builds** often resolve mysterious runtime errors
3. **Documentation reading** is crucial for understanding error messages
4. **Incremental testing** helps isolate issues in complex setups

### Best Practices Learned
- Keep reducers pure and predictable
- Use meaningful action names and types
- Structure state logically and avoid deep nesting
- Handle loading and error states consistently
- Separate API logic from Redux logic
- Always wrap the app with Provider at the root level

## Conclusion
This lab provided hands-on experience with Redux in a React Native environment. While Redux offers powerful state management capabilities, it requires careful setup and understanding of the React Native ecosystem. The debugging process taught valuable lessons about module resolution, native dependencies, and the importance of proper configuration in mobile development.

The final application successfully demonstrates Redux concepts including:
- Centralized state management
- Async data fetching
- Component-to-store communication
- Navigation integration
- Error handling and loading states

---

**Note**: To include actual screenshots, create a `screenshots/` folder in your project directory and add the following images:
- `product-list.png` - Product listing screen
- `product-added.png` - Product with "In Cart" button state
- `cart-screen.png` - Cart screen with items
- `cart-updated.png` - Cart with multiple quantities

