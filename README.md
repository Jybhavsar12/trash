# React Native Navigation Lab Report

  ## A. Screenshots Section


### Screenshot 1: Home Screen (Stack Navigator)
![answer 2025-12-03 at 3 16 41 PM](https://github.com/user-attachments/assets/c6e355ab-9c42-4e70-a2e3-10de01448e60)
*Caption: Main home screen showing the initial stack navigator screen with navigation buttons to Details and Profile screens.*

### Screenshot 2: Details Screen with Parameters
![answer 2025-12-03 at 3 17 12 PM](https://github.com/user-attachments/assets/599590ec-c693-4a74-b9b9-d2e2f78fc843)
*Caption: Details screen displaying passed parameters (itemId: 42, itemName: "Sample Item") demonstrating parameter passing between screens.*

### Screenshot 3: Profile Screen
![answer 2025-12-03 at 3 17 28 PM](https://github.com/user-attachments/assets/eb30cba3-4f11-4483-b86a-16bd14768df9)
*Caption: Profile screen accessible from the Home screen, showing stack navigation functionality.*

### Screenshot 4: Search Screen (Tab Navigation)
![answer 2025-12-03 at 3 16 56 PM](https://github.com/user-attachments/assets/e9987945-3ef3-4397-9333-245dc2ea72df)
*Caption: Search tab with text input field and live search display, demonstrating tab navigation and state management.*

### Screenshot 5: Settings Screen (Tab Navigation)
![answer 2025-12-03 at 3 17 05 PM](https://github.com/user-attachments/assets/66b858d1-a60f-42ff-882d-ba07ec183cd6)
*Caption: Settings tab with toggle switches for notifications and dark mode, showing interactive components in tab navigation.*

## B. Navigation Implementation 

The navigation structure follows a hierarchical approach with `NavigationContainer` as the root wrapper. The main navigation is handled by `TabNavigator` using `createBottomTabNavigator()`, which provides three tabs: Home, Search, and Settings.

The Stack Navigator is nested within the Home tab using `createStackNavigator()`, containing three screens: Home, Details, and Profile. This nested structure allows for complex navigation patterns where users can navigate through a stack of screens within a specific tab context.

Configuration includes custom styling with `tabBarActiveTintColor: '#3498db'` for active tabs and `headerStyle` with blue background for stack screens. The `headerShown: false` option in TabNavigator prevents duplicate headers.

Parameter passing is implemented using the `navigation.navigate()` method with a second parameter object. For example: `navigation.navigate('Details', {itemId: 42, itemName: 'Sample Item'})`. The receiving screen accesses parameters through `route.params` in the options function or component props.

The TabNavigator uses `screenOptions` for consistent styling across tabs, while individual screens can override these with their own `options` prop. This structure provides both tab-based navigation for main sections and stack-based navigation for detailed workflows within each section.

## C. Challenges and Learning 

The primary challenge was understanding the nested navigation structure and how TabNavigator and StackNavigator interact. Initially, I struggled with header duplication when both navigators tried to render headers simultaneously, resolved by setting `headerShown: false` in TabNavigator.

Parameter passing required understanding the difference between navigation props and route props. Debugging was accomplished using React Native Debugger and console.log statements to verify parameter values.

The most confusing concept was the navigation prop availability across different navigator types. I learned that each screen receives navigation props automatically, but the available methods depend on the parent navigator type.

TypeScript integration posed additional challenges with proper typing for navigation and route parameters. I solved implementation problems by carefully reading React Navigation documentation and testing each navigation method individually.

Screen options configuration was initially confusing, particularly the difference between static options and dynamic options functions that receive navigation and route parameters.

## D. Testing and Verification 

For Stack Navigator testing, I verified forward and backward navigation between Home, Details, and Profile screens. I tested the hardware back button behavior and gesture navigation on both platforms.

Tab Navigator testing included switching between all three tabs and ensuring state preservation when returning to previously visited tabs. I verified that the nested stack navigator maintained its state within the Home tab.

Parameter passing was tested by navigating to Details screen with different parameter values and confirming they displayed correctly. I used console.log statements to verify parameter reception.

Navigation flow testing involved complex scenarios like navigating deep into the stack, switching tabs, and returning to verify proper state management and navigation history preservation.

## Navigation Flow Diagram

```
NavigationContainer (Root)
│
└── TabNavigator
    ├── HomeStack Tab
    │   └── StackNavigator
    │       ├── HomeScreen
    │       ├── DetailsScreen
    │       └── ProfileScreen
    │
    ├── Search Tab
    │   └── SearchScreen
    │
    └── Settings Tab
        └── SettingsScreen
```

### Component Hierarchy:
- **NavigationContainer**: Root navigation wrapper
- **TabNavigator**: Bottom tab navigation with 3 tabs
- **StackNavigator**: Nested in Home tab, manages screen stack
- **Screen Components**: Individual screens with navigation capabilities

### Navigation Methods Used:
- `navigation.navigate()` - Navigate to specific screen
- `navigation.goBack()` - Return to previous screen
- Tab switching through TabNavigator UI
- Parameter passing via navigation options

## Code Structure Summary

### Key Files:
- `App.tsx` - Root component with NavigationContainer
- `TabNavigator.js` - Bottom tab navigation setup
- `StackNavigator.js` - Stack navigation for Home tab
- Screen components in `src/screens/` directory

### Dependencies:
- `@react-navigation/native`
- `@react-navigation/bottom-tabs`
- `@react-navigation/stack`
- `react-native-gesture-handler`
- `react-native-screens`
