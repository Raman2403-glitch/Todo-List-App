<!DOCTYPE html>
<html>
<head>
    <title>To-Do List App</title>
    <link rel="stylesheet" href="style.css">
</head>
<body>
    <div class="container">
        <h1>My To-Do List</h1>
        <input type="text" id="taskInput" placeholder="Enter a task...">
        <button onclick="addTask()">Add Task</button>
        <ul id="taskList"></ul>
    </div>
    <script src="script.js"></script>
</body>
</html># Todo-List-App

body {
    font-family: Arial, sans-serif;
    margin: 0;
    padding: 20px;
    background-color: #f4f4f9;
}
.container {
    max-width: 500px;
    margin: 0 auto;
    background: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 0 10px rgba(0,0,0,0.1);
}
#taskInput {
    width: 70%;
    padding: 10px;
    margin-right: 10px;
}
button {
    padding: 10px 20px;
    background: #28a745;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
}
button:hover {
    background: #218838;
}
ul {
    list-style: none;
    padding: 0;
}
li {
    padding: 10px;
    background: #f9f9f9;
    margin-bottom: 5px;
    border-radius: 4px;
    display: flex;
    justify-content: space-between;
}
.completed {
    text-decoration: line-through;
    color: #888;
}

document.addEventListener('DOMContentLoaded', loadTasks);

function loadTasks() {
    const tasks = JSON.parse(localStorage.getItem('tasks')) || [];
    const taskList = document.getElementById('taskList');
    taskList.innerHTML = '';
    tasks.forEach(task => {
        addTaskToDOM(task.text, task.completed);
    });
}

function addTask() {
    const taskInput = document.getElementById('taskInput');
    const taskText = taskInput.value.trim();
    if (taskText === '') return;

    addTaskToDOM(taskText, false);
    saveTaskToLocalStorage(taskText, false);
    taskInput.value = '';
}

function addTaskToDOM(taskText, isCompleted) {
    const taskList = document.getElementById('taskList');
    const li = document.createElement('li');
    li.className = isCompleted ? 'completed' : '';
    li.innerHTML = `
        <span>${taskText}</span>
        <div>
            <button onclick="toggleComplete(this)">✓</button>
            <button onclick="deleteTask(this)">✗</button>
        </div>
    `;
    taskList.appendChild(li);
}

function toggleComplete(button) {
    const li = button.parentElement.parentElement;
    li.classList.toggle('completed');
    updateLocalStorage();
}

function deleteTask(button) {
    const li = button.parentElement.parentElement;
    li.remove();
    updateLocalStorage();
}

function saveTaskToLocalStorage(taskText, isCompleted) {
    const tasks = JSON.parse(localStorage.getItem('tasks')) || [];
    tasks.push({ text: taskText, completed: isCompleted });
    localStorage.setItem('tasks', JSON.stringify(tasks));
}

function updateLocalStorage() {
    const tasks = [];
    document.querySelectorAll('#taskList li').forEach(li => {
        tasks.push({
            text: li.querySelector('span').textContent,
            completed: li.classList.contains('completed')
        });
    });
    localStorage.setItem('tasks', JSON.stringify(tasks));
}
