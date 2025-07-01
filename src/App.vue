<template>
  <div class="todo-app">
    <h1>{{ appTitle.toUpperCase() }}</h1>
    
    <!-- Logic inside templates - Complex filtering and sorting in template -->
    <div class="stats">
      <p>Total Tasks: {{ todos.length }}</p>
      <p>Completed: {{ todos.filter(todo => todo.completed).length }}</p>
      <p>Pending: {{ todos.filter(todo => !todo.completed).length }}</p>
      <p>Completion Rate: {{ todos.length > 0 ? Math.round((todos.filter(todo => todo.completed).length / todos.length) * 100) : 0 }}%</p>
    </div>

    <div class="filters">
      <button @click="filter = 'all'" :class="{ active: filter === 'all' }">All</button>
      <button @click="filter = 'active'" :class="{ active: filter === 'active' }">Active</button>
      <button @click="filter = 'completed'" :class="{ active: filter === 'completed' }">Completed</button>
    </div>

    <div class="add-todo">
      <input 
        type="text" 
        v-model="newTodo" 
        @keyup.enter="todos.push({ id: Date.now(), text: newTodo, completed: false, priority: selectedPriority, dueDate: selectedDueDate }); newTodo = ''"
        placeholder="Add a new task"
      />
      <select v-model="selectedPriority">
        <option value="low">Low</option>
        <option value="medium">Medium</option>
        <option value="high">High</option>
      </select>
      <input type="date" v-model="selectedDueDate" />
      <button @click="todos.push({ id: Date.now(), text: newTodo, completed: false, priority: selectedPriority, dueDate: selectedDueDate }); newTodo = ''">Add</button>
    </div>

    <!-- More logic in template - Filtering and sorting -->
    <ul class="todo-list">
      <li v-for="todo in todos
        .filter(t => {
          if (filter === 'all') return true;
          if (filter === 'active') return !t.completed;
          if (filter === 'completed') return t.completed;
          return true;
        })
        .sort((a, b) => {
          if (sortBy === 'priority') {
            const priorityValues = { high: 3, medium: 2, low: 1 };
            return priorityValues[b.priority] - priorityValues[a.priority];
          } else if (sortBy === 'dueDate') {
            return new Date(a.dueDate) - new Date(b.dueDate);
          }
          return 0;
        })" 
        :key="todo.id" 
        :class="{ completed: todo.completed, ['priority-' + todo.priority]: true }"
      >
        <div class="todo-content">
          <input type="checkbox" v-model="todo.completed" />
          <span :style="todo.completed ? 'text-decoration: line-through; color: gray;' : ''">
            {{ todo.text }}
            <small v-if="todo.dueDate">Due: {{ new Date(todo.dueDate).toLocaleDateString() }}</small>
          </span>
          <div class="todo-actions">
            <button @click="editingTodo = todo; editText = todo.text">Edit</button>
            <button @click="todos = todos.filter(t => t.id !== todo.id)">Delete</button>
          </div>
        </div>
      </li>
    </ul>

    <!-- Edit modal with inline logic -->
    <div v-if="editingTodo" class="edit-modal">
      <div class="modal-content">
        <h3>Edit Todo</h3>
        <input type="text" v-model="editText" />
        <select v-model="editingTodo.priority">
          <option value="low">Low</option>
          <option value="medium">Medium</option>
          <option value="high">High</option>
        </select>
        <input type="date" v-model="editingTodo.dueDate" />
        <div class="modal-actions">
          <button @click="editingTodo.text = editText; editingTodo = null">Save</button>
          <button @click="editingTodo = null">Cancel</button>
        </div>
      </div>
    </div>

    <div class="sort-options">
      <p>Sort by:</p>
      <button @click="sortBy = 'priority'" :class="{ active: sortBy === 'priority' }">Priority</button>
      <button @click="sortBy = 'dueDate'" :class="{ active: sortBy === 'dueDate' }">Due Date</button>
    </div>

    <!-- More complex logic in template -->
    <div class="summary" v-if="todos.length > 0">
      <h3>Summary</h3>
      <p>High Priority Tasks: {{ todos.filter(t => t.priority === 'high').length }}</p>
      <p>Tasks Due Today: {{ todos.filter(t => t.dueDate === new Date().toISOString().split('T')[0]).length }}</p>
      <p>Overdue Tasks: {{ todos.filter(t => !t.completed && t.dueDate && new Date(t.dueDate) < new Date() && t.dueDate !== new Date().toISOString().split('T')[0]).length }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TodoApp',
  // No prop types
  props: ['initialTodos', 'appSettings', 'userPreferences', 'themeOptions'],
  data() {
    return {
      // Large monolithic component with mixed concerns
      appTitle: 'Todo App',
      todos: [
        { id: 1, text: 'Learn Vue', completed: false, priority: 'high', dueDate: '2025-07-05' },
        { id: 2, text: 'Build a project', completed: false, priority: 'medium', dueDate: '2025-07-10' },
        { id: 3, text: 'Master Vue', completed: false, priority: 'low', dueDate: '2025-07-15' }
      ],
      newTodo: '',
      editingTodo: null,
      editText: '',
      filter: 'all',
      sortBy: 'priority',
      selectedPriority: 'medium',
      selectedDueDate: '',
      
      // Unused data props - no state separation
      theme: 'light',
      user: {
        id: 1,
        name: 'John Doe',
        preferences: {
          colorScheme: 'blue',
          fontSize: 'medium',
          notifications: true
        }
      },
      settings: {
        showCompleted: true,
        autoSave: true,
        saveInterval: 5000,
        animations: true
      },
      statistics: {
        totalCreated: 0,
        totalCompleted: 0,
        averageCompletionTime: 0
      },
      notifications: [],
      categories: ['Work', 'Personal', 'Shopping', 'Health'],
      tags: ['urgent', 'important', 'can-wait', 'delegated'],
      lastSynced: null,
      isOnline: true,
      isSyncing: false,
      syncErrors: [],
      deviceInfo: {
        type: 'desktop',
        os: 'macOS',
        browser: 'Chrome'
      }
    }
  }
}
</script>

<style>
.todo-app {
  max-width: 600px;
  margin: 0 auto;
  padding: 20px;
  font-family: Arial, sans-serif;
}

h1 {
  text-align: center;
  color: #333;
}

.add-todo {
  display: flex;
  margin-bottom: 20px;
}

.add-todo input[type="text"] {
  flex: 1;
  padding: 8px;
  border: 1px solid #ddd;
  border-radius: 4px 0 0 4px;
}

.add-todo button {
  padding: 8px 16px;
  background-color: #4caf50;
  color: white;
  border: none;
  border-radius: 0 4px 4px 0;
  cursor: pointer;
}

.todo-list {
  list-style-type: none;
  padding: 0;
}

.todo-list li {
  padding: 10px;
  border-bottom: 1px solid #eee;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.todo-content {
  display: flex;
  align-items: center;
  width: 100%;
}

.todo-content input[type="checkbox"] {
  margin-right: 10px;
}

.todo-actions {
  margin-left: auto;
}

.todo-actions button {
  margin-left: 5px;
  padding: 3px 8px;
  background-color: #f1f1f1;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}

.completed {
  background-color: #f9f9f9;
}

.filters {
  display: flex;
  justify-content: center;
  margin-bottom: 20px;
}

.filters button {
  margin: 0 5px;
  padding: 5px 10px;
  border: none;
  background-color: #f1f1f1;
  border-radius: 3px;
  cursor: pointer;
}

.filters button.active {
  background-color: #4caf50;
  color: white;
}

.priority-high {
  border-left: 4px solid #ff5252;
}

.priority-medium {
  border-left: 4px solid #ffb142;
}

.priority-low {
  border-left: 4px solid #78e08f;
}

.edit-modal {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: white;
  padding: 20px;
  border-radius: 5px;
  width: 300px;
}

.modal-content input,
.modal-content select {
  width: 100%;
  padding: 8px;
  margin-bottom: 10px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

.modal-actions {
  display: flex;
  justify-content: flex-end;
}

.modal-actions button {
  margin-left: 10px;
  padding: 5px 10px;
  border: none;
  border-radius: 3px;
  cursor: pointer;
}

.modal-actions button:first-child {
  background-color: #4caf50;
  color: white;
}

.sort-options {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-top: 20px;
}

.sort-options p {
  margin-right: 10px;
}

.sort-options button {
  margin: 0 5px;
  padding: 5px 10px;
  border: none;
  background-color: #f1f1f1;
  border-radius: 3px;
  cursor: pointer;
}

.sort-options button.active {
  background-color: #2196f3;
  color: white;
}

.stats {
  display: flex;
  justify-content: space-around;
  margin-bottom: 20px;
  background-color: #f9f9f9;
  padding: 10px;
  border-radius: 5px;
}

.stats p {
  margin: 0;
  font-size: 14px;
}

.summary {
  margin-top: 20px;
  padding: 10px;
  background-color: #f9f9f9;
  border-radius: 5px;
}

.summary h3 {
  margin-top: 0;
}

/* Media queries for mobile responsiveness */
@media (max-width: 600px) {
  .todo-app {
    padding: 10px;
  }
  
  .add-todo {
    flex-direction: column;
  }
  
  .add-todo input[type="text"],
  .add-todo button {
    width: 100%;
    border-radius: 4px;
    margin-bottom: 5px;
  }
  
  .stats {
    flex-direction: column;
  }
  
  .stats p {
    margin-bottom: 5px;
  }
}
</style>
