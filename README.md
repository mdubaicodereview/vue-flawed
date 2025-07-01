# Vue Flawed TODO App

A deliberately flawed Vue 3 TODO application created to test AI code reviewers.

## Features

- Add, edit, and delete tasks
- Filter tasks by completion status (All, Active, Completed)
- Filter tasks by categories (Work, Personal, Shopping, Health)
- Sort tasks by priority, due date, or category
- Set task priority (Low, Medium, High)
- Set task due dates
- Assign categories to tasks
- Mobile responsive design

## Intentional Flaws

This application contains the following intentional flaws:

1. **Logic inside templates** - Complex filtering, sorting, and calculations are placed directly in the template instead of using computed properties or methods
2. **No prop types** - Component props are defined without type validation
3. **Large monolithic component** - The entire application is built as a single large component instead of being broken down into smaller, reusable components
4. **Unused data props and no state separation** - The component contains many unused data properties and lacks proper state management
5. **Inconsistent category handling** - Categories feature uses optional chaining in some places but not others, potentially causing runtime errors
6. **Duplicate code** - The same logic for adding todos is duplicated in multiple places
7. **Inefficient DOM updates** - Category filtering causes unnecessary re-renders due to direct manipulation in the template
8. **Missing accessibility features** - Category UI elements lack proper ARIA attributes and keyboard navigation

## Running the App

To run the application locally:

```bash
npm install
npm run dev
```

## Project Structure

The project uses Vue 3 with Vite as the build tool and plain CSS for styling.
