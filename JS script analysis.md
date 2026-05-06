1. Create const variables to all the elements inc. buttons, text box, list, lables
```js
  const input = document.getElementById('todoInput');
  const addBtn = document.getElementById('addBtn');
  const listEl = document.getElementById('todoList');
  const countLabel = document.getElementById('countLabel');
  const clearBtn = document.getElementById('clearBtn');
  const filterBtns = document.querySelectorAll('.filters button');
```

2. create variable todos that will store array of todo item objects.
```js
let todos = JSON.parse(localStorage.getItem('todos') || '[]');
```
Let's break down 
`localStorage.getItem('todos')`
localStorage is an built-in browser objects to store data persistantly
getItem is a method of localStorage object lookup the key 'todos' and returns it's string representation.
`|| '[]'` is a safety default using OR operator. if the previous part returns null parse `'[]'
which will result in an empty array
`JSON` is a javascript built-in object. `parse` is a method of that object that turns a string to an actual javascript value. In this case turn string `"[{...}, {...}]"` to an array of objects.
and if the string will be `"[]"` it will create an empty array.

```
localStorage          → built-in object, holds your saved string
.getItem('todos')     → method call, retrieves the string (or null)
|| '[]'               → fallback if null
JSON.parse(...)       → built-in object + method, converts string → JS array
let todos = ...       → variable now holds a usable JS array
```

3. Add Event Listener to few of the controls we selected in step 1
addBtn when clicked will trigger addTodo func
input textbox when specific keydown (enter key) will trigger addTodo func
clearBtn when clicked will trigger clearComplete func
```js
  addBtn.addEventListener('click', addTodo);
  input.addEventListener('keydown', e => { if (e.key === 'Enter') addTodo(); });
  clearBtn.addEventListener('click', clearCompleted);
```

4. filterBtns selects a list of btns from step 1
```js
const filterBtns = document.querySelectorAll('.filters button');
```

- now we run the list of filterBtns with forEach loop adding event listener to all of them (outer loop).
- sets filter(let global variable) to the button dataset written starting `data-[name of variable]` and inserts it to filter "all", "active", or "completed".
```html
    <button class="active" data-filter="all">All</button>
    <button data-filter="active">Active</button>
    <button data-filter="completed">Completed</button>
```
- remove `class="active"` for each of them if exists.
- add `class="active"` for the clicked btn.
- render the list.
```js
  filterBtns.forEach(btn => {
    btn.addEventListener('click', () => {
      filter = btn.dataset.filter;
      filterBtns.forEach(b => b.classList.remove('active'));
      btn.classList.add('active');
      render();
    });
  });
```
