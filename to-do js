const taskForm = document.getElementById("taskForm");
const taskInput = document.getElementById("taskInput");
const taskList = document.getElementById("taskList");
const prioritySelect = document.getElementById("priority");

let tasks = JSON.parse(localStorage.getItem("tasks")) || [];

function renderTasks() {
  taskList.innerHTML = "";
  tasks.forEach((task, index) => {
    const li = document.createElement("li");
    li.setAttribute("draggable", true);
    li.setAttribute("data-priority", task.priority);
    li.className = task.completed ? "done" : "";
    li.innerHTML = `
      <span>${task.text}</span>
      <button class="delete-btn" onclick="deleteTask(${index})">🗑️</button>
    `;
    li.addEventListener("click", () => toggleTask(index));
    li.addEventListener("dragstart", () => (li.classList.add("dragging")));
    li.addEventListener("dragend", () => {
      li.classList.remove("dragging");
      saveTasks();
    });
    taskList.appendChild(li);
  });
  addDragAndDrop();
  localStorage.setItem("tasks", JSON.stringify(tasks));
}

function addDragAndDrop() {
  taskList.addEventListener("dragover", (e) => {
    e.preventDefault();
    const dragging = document.querySelector(".dragging");
    const afterElement = getDragAfterElement(taskList, e.clientY);
    if (afterElement == null) {
      taskList.appendChild(dragging);
    } else {
      taskList.insertBefore(dragging, afterElement);
    }
    updateTaskOrder();
  });
}

function getDragAfterElement(container, y) {
  const draggableElements = [...container.querySelectorAll("li:not(.dragging)")];

  return draggableElements.reduce((closest, child) => {
    const box = child.getBoundingClientRect();
    const offset = y - box.top - box.height / 2;
    if (offset < 0 && offset > closest.offset) {
      return { offset, element: child };
    } else {
      return closest;
    }
  }, { offset: Number.NEGATIVE_INFINITY }).element;
}

function updateTaskOrder() {
  const newTasks = [];
  document.querySelectorAll("#taskList li").forEach(li => {
    const text = li.querySelector("span").innerText;
    const priority = li.getAttribute("data-priority");
    const completed = li.classList.contains("done");
    newTasks.push({ text, priority, completed });
  });
  tasks = newTasks;
  localStorage.setItem("tasks", JSON.stringify(tasks));
}

function addTask(text, priority) {
  tasks.push({ text, priority, completed: false });
  renderTasks();
}

function toggleTask(index) {
  tasks[index].completed = !tasks[index].completed;
  renderTasks();
}

function deleteTask(index) {
  tasks.splice(index, 1);
  renderTasks();
}

taskForm.addEventListener("submit", (e) => {
  e.preventDefault();
  const text = taskInput.value.trim();
  const priority = prioritySelect.value;
  if (text) {
    addTask(text, priority);
    taskInput.value = "";
  }
});

renderTasks();
