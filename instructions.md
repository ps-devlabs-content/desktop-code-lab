# Guided: Task Manager Application in JavaScript and TypeScript
This lab guides you through building a simple task management application with both TypeScript and JavaScript implementations.

{% dropdown label="Choose Implementation" placeholder="Select language..." %}
{% dropdown-option value="typescript" label="TypeScript" /%}
{% dropdown-option value="javascript" label="JavaScript" /%}
{% /dropdown %}

{% steps %}
{% step title="Project Setup" %}
{% callout type="info" title="Lab Overview" %}
You'll create a task manager that allows users to:
- Add new tasks
- Mark tasks as complete
- Filter tasks by status
- Delete tasks
{% /callout %}

{% dropdown-content showFor="typescript" %}
First, let's set up our project structure:
{% task %}
Create the TypeScript project structure:
{% check command="mkdir -p task-manager-ts/src" %}
Create project directories
{% /check %}
{% check command="touch task-manager-ts/src/task.ts task-manager-ts/src/task-manager.ts task-manager-ts/src/app.ts" %}
Create TypeScript files
{% /check %}

{% feedback pattern=".*" type="success" %}
✅ TypeScript project structure created successfully!
{% /feedback %}
{% /task %}

Let's create a basic TypeScript configuration file:
{% task %}
Create tsconfig.json:
{% check command="echo '{
  \"compilerOptions\": {
    \"target\": \"ES2020\",
    \"module\": \"commonjs\",
    \"outDir\": \"./dist\",
    \"strict\": true,
    \"esModuleInterop\": true
  },
  \"include\": [\"src/**/*\"]
}' > task-manager-ts/tsconfig.json" %}
Create TypeScript config
{% /check %}

{% feedback pattern=".*" type="success" %}
✅ TypeScript configuration created!
{% /feedback %}
{% /task %}
{% /dropdown-content %}

{% dropdown-content showFor="javascript" %}
{% task %}
Create the JavaScript project structure:

{% check command="mkdir -p task-manager-js/src" %}
Create project directories
{% /check %}

{% check command="touch task-manager-js/src/task.js task-manager-js/src/task-manager.js task-manager-js/src/app.js" %}
Create JavaScript files
{% /check %}

{% feedback pattern=".*" type="success" %}
✅ JavaScript project structure created successfully!
{% /feedback %}
{% /task %}
{% /dropdown-content %}
{% /step %}




{% step title="Task Model" %}
Let's create our Task model:
{% dropdown-content showFor="typescript" %}
{% task %}
Define the Task class in TypeScript:

{% check command="echo 'export class Task {
  id: number;
  title: string;
  description: string;
  completed: boolean;
  createdAt: Date;

  constructor(id: number, title: string, description: string = \"\") {
    this.id = id;
    this.title = title;
    this.description = description;
    this.completed = false;
    this.createdAt = new Date();
  }

  complete(): void {
    this.completed = true;
  }

  uncomplete(): void {
    this.completed = false;
  }
}' > task-manager-ts/src/task.ts"
        treeFile="task-manager-ts/src/task.ts"
        treeQuery="(class_declaration name: (type_identifier) @class-name (#eq? @class-name \"Task\"))"
        treeLang="typescript" %}
Create Task class
{% /check %}

{% feedback pattern="tree:has-matches" type="success" %}
✅ Task class created correctly!
{% /feedback %}

{% feedback pattern="tree:no-matches" type="error" %}
❌ Task class not found or incorrectly defined. Make sure you've created a class named Task.
{% /feedback %}
{% /task %}
{% /dropdown-content %}

{% dropdown-content showFor="javascript" %}
{% task %}
Define the Task class in JavaScript:

{% check command="echo 'class Task {
  constructor(id, title, description = \"\") {
    this.id = id;
    this.title = title;
    this.description = description;
    this.completed = false;
    this.createdAt = new Date();
  }

  complete() {
    this.completed = true;
  }

  uncomplete() {
    this.completed = false;
  }
}

module.exports = Task;' > task-manager-js/src/task.js"
        treeFile="task-manager-js/src/task.js"
        treeQuery="(class_declaration name: (identifier) @class-name (#eq? @class-name \"Task\"))"
        treeLang="javascript" %}
Create Task class
{% /check %}

{% feedback pattern="tree:has-matches" type="success" %}
✅ Task class created correctly!
{% /feedback %}

{% feedback pattern="tree:no-matches" type="error" %}
❌ Task class not found or incorrectly defined. Make sure you've created a class named Task.
{% /feedback %}
{% /task %}
{% /dropdown-content %}
{% /step %}




{% step title="Task Manager" %}
Now, let's implement the TaskManager class:

{% dropdown-content showFor="typescript" %}
{% task %}
Create the TaskManager class:

{% check command="echo 'import { Task } from \"./task\";

export class TaskManager {
  private tasks: Task[] = [];
  private nextId: number = 1;

  addTask(title: string, description: string = \"\"): Task {
    const task = new Task(this.nextId++, title, description);
    this.tasks.push(task);
    return task;
  }

  getTaskById(id: number): Task | undefined {
    return this.tasks.find(task => task.id === id);
  }

  getAllTasks(): Task[] {
    return [...this.tasks];
  }

  getCompletedTasks(): Task[] {
    return this.tasks.filter(task => task.completed);
  }

  getPendingTasks(): Task[] {
    return this.tasks.filter(task => !task.completed);
  }

  completeTask(id: number): boolean {
    const task = this.getTaskById(id);
    if (task) {
      task.complete();
      return true;
    }
    return false;
  }

  deleteTask(id: number): boolean {
    const initialLength = this.tasks.length;
    this.tasks = this.tasks.filter(task => task.id !== id);
    return this.tasks.length !== initialLength;
  }
}' > task-manager-ts/src/task-manager.ts"
        treeFile="task-manager-ts/src/task-manager.ts"
        cssQuery="class_declaration type_identifier[\"TaskManager\"]"
        treeLang="typescript" %}
{% /check %}

{% feedback pattern="tree:has-matches" type="success" %}
✅ TaskManager class created correctly!
{% /feedback %}

{% feedback pattern="tree:no-matches" type="error" %}
❌ TaskManager class not found or incorrectly defined.
{% /feedback %}
{% /task %}
{% /dropdown-content %}

{% dropdown-content showFor="javascript" %}
{% task %}
Create the TaskManager class:

{% check command="echo 'const Task = require(\"./task\");

class TaskManager {
  constructor() {
    this.tasks = [];
    this.nextId = 1;
  }

  addTask(title, description = \"\") {
    const task = new Task(this.nextId++, title, description);
    this.tasks.push(task);
    return task;
  }

  getTaskById(id) {
    return this.tasks.find(task => task.id === id);
  }

  getAllTasks() {
    return [...this.tasks];
  }

  getCompletedTasks() {
    return this.tasks.filter(task => task.completed);
  }

  getPendingTasks() {
    return this.tasks.filter(task => !task.completed);
  }

  completeTask(id) {
    const task = this.getTaskById(id);
    if (task) {
      task.complete();
      return true;
    }
    return false;
  }

  deleteTask(id) {
    const initialLength = this.tasks.length;
    this.tasks = this.tasks.filter(task => task.id !== id);
    return this.tasks.length !== initialLength;
  }
}

module.exports = TaskManager;' > task-manager-js/src/task-manager.js"
        treeFile="task-manager-js/src/task-manager.js"
        cssQuery="class_declaration identifier[\"TaskManager\"]"
        treeLang="javascript" %}
Create TaskManager class
{% /check %}

{% feedback pattern="tree:has-matches" type="success" %}
✅ TaskManager class created correctly!
{% /feedback %}

{% feedback pattern="tree:no-matches" type="error" %}
❌ TaskManager class not found or incorrectly defined.
{% /feedback %}
{% /task %}
{% /dropdown-content %}
{% /step %}




{% step title="Task Model" %}
Let's create our Task model:
{% dropdown-content showFor="typescript" %}
{% task %}
Define the Task class in TypeScript:

{% check command="echo 'export class Task {
  id: number;
  title: string;
  description: string;
  completed: boolean;
  createdAt: Date;

  constructor(id: number, title: string, description: string = \"\") {
    this.id = id;
    this.title = title;
    this.description = description;
    this.completed = false;
    this.createdAt = new Date();
  }

  complete(): void {
    this.completed = true;
  }

  uncomplete(): void {
    this.completed = false;
  }
}' > task-manager-ts/src/task.ts"
        treeFile="task-manager-ts/src/task.ts"
        treeQuery="(class_declaration name: (type_identifier) @class-name (#eq? @class-name \"Task\"))"
        treeLang="typescript" %}
Create Task class
{% /check %}

{% feedback pattern="tree:has-matches" type="success" %}
✅ Task class created correctly!
{% /feedback %}

{% feedback pattern="tree:no-matches" type="error" %}
❌ Task class not found or incorrectly defined. Make sure you've created a class named Task.
{% /feedback %}
{% /task %}
{% /dropdown-content %}

{% dropdown-content showFor="javascript" %}
{% task %}
Define the Task class in JavaScript:

{% check command="echo 'class Task {
  constructor(id, title, description = \"\") {
    this.id = id;
    this.title = title;
    this.description = description;
    this.completed = false;
    this.createdAt = new Date();
  }

  complete() {
    this.completed = true;
  }

  uncomplete() {
    this.completed = false;
  }
}

module.exports = Task;' > task-manager-js/src/task.js"
        treeFile="task-manager-js/src/task.js"
        treeQuery="(class_declaration name: (identifier) @class-name (#eq? @class-name \"Task\"))"
        treeLang="javascript" %}
Create Task class
{% /check %}

{% feedback pattern="tree:has-matches" type="success" %}
✅ Task class created correctly!
{% /feedback %}

{% feedback pattern="tree:no-matches" type="error" %}
❌ Task class not found or incorrectly defined. Make sure you've created a class named Task.
{% /feedback %}
{% /task %}
{% /dropdown-content %}
{% /step %}




{% step title="Application Entry Point" %}
Finally, let's create our application entry point:

{% dropdown-content showFor="typescript" %}
{% task %}
Create the app.ts file:

{% check command="echo 'import { TaskManager } from \"./task-manager\";

// Create a new task manager
const taskManager = new TaskManager();

// Add some sample tasks
console.log(\"Adding sample tasks...\");
taskManager.addTask(\"Complete TypeScript lab\", \"Finish all exercises in the TS lab\");
taskManager.addTask(\"Learn Tree-sitter\", \"Study tree-sitter query syntax\");
taskManager.addTask(\"Build a project\", \"Create a small application using TS\");

// Display all tasks
console.log(\"\\nAll tasks:\");
console.log(taskManager.getAllTasks());

// Complete a task
console.log(\"\\nCompleting task #1...\");
taskManager.completeTask(1);

// Show pending tasks
console.log(\"\\nPending tasks:\");
console.log(taskManager.getPendingTasks());

// Show completed tasks
console.log(\"\\nCompleted tasks:\");
console.log(taskManager.getCompletedTasks());

// Delete a task
console.log(\"\\nDeleting task #2...\");
taskManager.deleteTask(2);

// Show all tasks after deletion
console.log(\"\\nAll tasks after deletion:\");
console.log(taskManager.getAllTasks());' > task-manager-ts/src/app.ts"
        treeFile="task-manager-ts/src/app.ts"
        treeQuery="(import_statement)"
        treeLang="typescript" %}
Create app.ts
{% /check %}

{% feedback pattern="tree:has-matches" type="success" %}
✅ Application entry point created correctly!
{% /feedback %}

{% feedback pattern="tree:no-matches" type="error" %}
❌ Import statement not found. Make sure you've properly imported the TaskManager.
{% /feedback %}

{% openfile path="task-manager-ts/src/app.ts" %}
Open app.ts
{% /openfile %}

The app.ts file demonstrates how to use the TaskManager to:
- Create tasks
- Complete tasks
- Filter tasks
- Delete tasks

{% /task %}
{% /dropdown-content %}

{% dropdown-content showFor="javascript" %}
{% task %}
Create the app.js file:

{% check command="echo 'const TaskManager = require(\"./task-manager\");

// Create a new task manager
const taskManager = new TaskManager();

// Add some sample tasks
console.log(\"Adding sample tasks...\");
taskManager.addTask(\"Complete JavaScript lab\", \"Finish all exercises in the JS lab\");
taskManager.addTask(\"Learn Tree-sitter\", \"Study tree-sitter query syntax\");
taskManager.addTask(\"Build a project\", \"Create a small application using JS\");

// Display all tasks
console.log(\"\\nAll tasks:\");
console.log(taskManager.getAllTasks());

// Complete a task
console.log(\"\\nCompleting task #1...\");
taskManager.completeTask(1);

// Show pending tasks
console.log(\"\\nPending tasks:\");
console.log(taskManager.getPendingTasks());

// Show completed tasks
console.log(\"\\nCompleted tasks:\");
console.log(taskManager.getCompletedTasks());

// Delete a task
console.log(\"\\nDeleting task #2...\");
taskManager.deleteTask(2);

// Show all tasks after deletion
console.log(\"\\nAll tasks after deletion:\");
console.log(taskManager.getAllTasks());' > task-manager-js/src/app.js"
        treeFile="task-manager-js/src/app.js"
        treeQuery="(variable_declarator name: (identifier) @var-name (#eq? @var-name \"TaskManager\"))"
        treeLang="javascript" %}
Create app.js
{% /check %}

{% feedback pattern="tree:has-matches" type="success" %}
✅ Application entry point created correctly!
{% /feedback %}

{% feedback pattern="tree:no-matches" type="error" %}
❌ TaskManager variable not found. Make sure you've properly required the TaskManager.
{% /feedback %}


{% openfile path="task-manager-js/src/app.js" %}
Open app.js
{% /openfile %}

The app.js file demonstrates how to use the TaskManager to:
- Create tasks
- Complete tasks
- Filter tasks
- Delete tasks

{% /task %}
{% /dropdown-content %}
{% /step %}




{% step title="Challenge: Extend the Application" %}
{% callout type="challenge" title="Add Priority Feature" %}
Extend the Task Manager application to include task priorities:

1. Add a `priority` property to the Task class (values: 'low', 'medium', 'high')
2. Add a method to the TaskManager to filter tasks by priority
3. Update the app to demonstrate the new priority feature
{% /callout %}

{% task %}
Implement the priority feature in your chosen language and run the application to verify it works.

{% feedback pattern=".*" type="info" %}
When you've completed the challenge, you can check your implementation against our solution.
{% /feedback %}
{% /task %}
{% /step %}




{% step title="Conclusion" %}

Congratulations! You've built a simple task management application in both TypeScript and JavaScript. This lab demonstrated:

1. Object-oriented programming concepts
2. TypeScript type safety vs. JavaScript flexibility
3. Project organization and structure
4. Basic task management functionality

You can continue to extend this application with features like:
- Persistent storage (file or database)
- Due dates for tasks
- Task categories or tags
- A simple CLI or web interface
{% /step %}

{% partial file="last_step" %}
{% /steps %}
