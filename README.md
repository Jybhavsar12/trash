# Navigation Flow Diagram (Mermaid)

```mermaid
graph TD
    A[NavigationContainer<br/>Root] --> B[TabNavigator<br/>Bottom Tabs]
    
    B --> C[HomeStack Tab<br/>StackNavigator]
    B --> D[Search Tab<br/>SearchScreen]
    B --> E[Settings Tab<br/>SettingsScreen]
    
    C --> F[HomeScreen<br/>Entry Point]
    C --> G[DetailsScreen<br/>Receives Parameters]
    C --> H[ProfileScreen<br/>User Profile]
    
    F -->|navigate with params| G
    F -->|navigate| H
    G -->|goBack| F
    H -->|goBack| F
    
    style A fill:#e1f5fe
    style B fill:#f3e5f5
    style C fill:#e8f5e8
    style D fill:#fff3e0
    style E fill:#fff3e0
    style F fill:#e8f5e8
    style G fill:#e8f5e8
    style H fill:#e8f5e8
```

## Component Details

### Navigation Structure
- **Root**: NavigationContainer wraps entire app
- **Main**: TabNavigator provides 3 bottom tabs
- **Nested**: StackNavigator within Home tab
- **Screens**: Individual screen components

### Parameter Passing
```javascript
// From HomeScreen to DetailsScreen
navigation.navigate('Details', {
  itemId: 42,
  itemName: 'Sample Item'
});

// In DetailsScreen
const { itemId, itemName } = route.params;
```
