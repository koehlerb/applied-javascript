---
sidebar_label: 'Module 4 - Feathers Demo'
sidebar_position: 3
---

# Module 1 - Feathers Demo

Here is the starter `index.html` code for the Feathers demo:

```html
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="utf-8">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <title>DGL 213 Grocery List</title>
  <link rel="stylesheet" href="https://cdn.simplecss.org/simple.min.css">
</head>

<body>
  <header>
    <h1>DGL 213 Grocery List</h1>
    <p>Food</p>
  </header>

  <main>
    <p>
      <label>Quantity</label>
      <select id="quantity">
        <option selected="selected">1</option>
        <option>2</option>
        <option>3</option>
        <option>4</option>
        <option>5</option>
    </select>
    </p>
  
    <p>
    <label>Item</label>
    <input type="text" id="item">
    </p>

    <button id="add">Add</button>
    <table>
      <thead>
        <tr>
          <th>Quantity</th>
          <th>Item</th>
          <th>Delete</th>
        </tr>
      </thead>
      <tbody id="grocerylist">
      </tbody>
    </table>
  </main>

  
  <footer>
    <p>DGL 213 Grocery List</p>
  </footer>

  <script src="https://unpkg.com/@feathersjs/client@^5.0.11/dist/feathers.js"></script>

  <script src="app.js"></script>
</body>
</html>
```