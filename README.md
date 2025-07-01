# Vue Flawed TODO App

A deliberately flawed Vue 3 TODO application created to test AI code reviewers.

## Intentional Flaws

This application contains the following intentional flaws:

1. **Logic inside templates** - Complex filtering, sorting, and calculations are placed directly in the template instead of using computed properties or methods
2. **No prop types** - Component props are defined without type validation
3. **Large monolithic component** - The entire application is built as a single large component instead of being broken down into smaller, reusable components
4. **Unused data props and no state separation** - The component contains many unused data properties and lacks proper state management

## Running the App

To run the application locally:

```bash
npm install
npm run dev
```

## Project Structure

The project uses Vue 3 with Vite as the build tool and plain CSS for styling.
