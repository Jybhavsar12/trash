# CPAN 213 - Lab 6 Report
## Redux Todo App

### Student Information
- **Name**: Jyot Harshakumar Bhavsar
- **Student ID**: N01738884
- **Course**: CPAN 213 - Cross-Platform Mobile Development
- **Lab**: Lab 6 - Redux State Management
- **Date**: October 16, 2025

---

## A. Screenshots Section

### 1. Initial App State
![answer 2025-10-16 at 10 49 41 AM](https://github.com/user-attachments/assets/a4257768-10ce-4d1f-b92f-990f095f188e)
*Caption: Redux Todo app showing initial state with sample todos, statistics display, and filter buttons set to "All"*

### 2. Adding New Todo
![answer 2025-10-16 at 10 50 10 AM](https://github.com/user-attachments/assets/f253271f-f10e-4bfe-9d02-432a53558593)
*Caption: User adding a new todo item through the input field, demonstrating the addTodo action functionality*

### 3. Filter Functionality
![answer 2025-10-16 at 10 49 53 AM](https://github.com/user-attachments/assets/c7a60160-7ee9-467e-8b50-e2bb78e7200d)
*Caption: App displaying filtered view showing only "Active" todos, demonstrating the setFilter action and state filtering*
![answer 2025-10-16 at 10 49 58 AM](https://github.com/user-attachments/assets/c838b2db-f00e-4a15-9d6c-f6fc1f68d33d)
*Caption: App displaying filtered view showing only "Completed" todos, demonstrating the setFilter action and state filtering*

---

## B. Redux Implementation Summary (200 words)

My Redux store is structured using Redux Toolkit's configureStore with a single todos slice. The store configuration in `src/store/index.js` registers the todosReducer under the 'todos' key, creating a clean separation of concerns.

I created four main actions in the todos slice:
- `addTodo`: Creates new todos with unique timestamps as IDs
- `toggleTodo`: Toggles completion status by finding todos by ID
- `deleteTodo`: Removes todos by filtering out the specified ID
- `setFilter`: Updates the current filter state ('all', 'active', 'completed')

The reducer uses Redux Toolkit's createSlice, which leverages Immer for immutable updates. State mutations appear direct but are actually immutable under the hood. The initial state includes sample todos and a default 'all' filter.

Components connect to Redux using modern hooks:
- `useSelector` extracts state data with `state => state.todos`
- `useDispatch` provides access to dispatch actions
- The TodoListScreen component subscribes to state changes and automatically re-renders when todos or filters update

This architecture ensures predictable state management with clear data flow from UI interactions through actions to state updates and back to component re-renders.

---

## C. Challenges and Learning (150 words)

Initially, I encountered a blank screen issue due to missing store configuration. The app wasn't rendering because the store wasn't properly exported from `src/store/index.js`. I debugged this by adding a test component first, then gradually adding Redux functionality.

The most confusing concept was understanding how Redux Toolkit's createSlice works with Immer. I initially thought I was mutating state directly, but learned that Immer handles immutability behind the scenes. This was different from traditional Redux reducers.

Setting up the Provider wrapper correctly was another challenge - ensuring the store import path was correct and the component hierarchy was proper. I also struggled with understanding how useSelector triggers re-renders when state changes.

Key insights gained: Redux provides predictable state management through unidirectional data flow. Actions describe what happened, reducers specify how state changes, and components subscribe to relevant state slices. This separation makes debugging easier and state changes more traceable than prop drilling.

---

## D. Testing and Verification (100 words)

I tested each Redux action manually through the UI:
- `addTodo`: Verified new todos appear in the list with correct text and uncompleted status
- `toggleTodo`: Confirmed clicking checkboxes toggles completion and applies strikethrough styling
- `deleteTodo`: Tested delete confirmation dialog and verified todos are removed from state
- `setFilter`: Validated all three filter states show correct todo subsets

Without Redux DevTools, I used console.log statements in reducers to verify state updates. I found and fixed a bug where empty todos could be added by adding input validation with `trim()`.

I confirmed state updates work correctly by observing real-time statistics updates and filter functionality. The component re-renders appropriately when state changes, demonstrating proper Redux integration.

---

*Report completed on October 16, 2025 - Total development time: 4 hours*
*Redux Toolkit version: Latest - React Native version: 0.72+*
