---
sidebar_label: "Module 4 - Feathers Demo"
sidebar_position: 3
---

# Module 4 - Feathers API Demo

Here is the starter `index.html` code for the Feathers demo:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>DGL 213 Grocery List</title>
    <link rel="stylesheet" href="https://cdn.simplecss.org/simple.min.css" />
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
        <input type="text" id="item" />
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
        <tbody id="grocerylist"></tbody>
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

Here is the completed code for `app.js`:

```javascript
(() => {
  window.addEventListener("load", async (event) => {
    // create the feathers application object
    const app = feathers();

    // create the feathers REST client
    const restClient = feathers.rest();

    // configure the feathers applicaiton to use the the REST client
    // and the browser's fetch() method
    app.configure(restClient.fetch(window.fetch.bind(window)));

    // get a reference to the tbody element in the grocery list table
    const groceryListElement = document.getElementById("grocerylist");

    // regex for delete button id
    // the id of each delete button has the item id coded in it
    // so we can use that to figure out which item to delete
    const regex = /g[0-9]+/;

    // we add the event listener to the entire tbody element
    // when a user clicks on a 'Delete' button, it will bubble
    // up we will handle it here
    groceryListElement.addEventListener("click", async (ev) => {
      // if the id matches the expected pattern then we know which item to delete
      if (ev.target.id.match(regex)) {
        // delete the item from the server
        const deletedItem = await app
          .service("items")
          .remove(parseInt(ev.target.id.replace("g", "")));
        // also delete the row from the table
        ev.target.parentNode.parentNode.parentNode.removeChild(
          ev.target.parentNode.parentNode
        );
      }
    });

    // build a new row from an item object
    const appendItem = (item) => {
      // quantity column
      const quantityElement = document.createElement("td");
      quantityElement.textContent = item.quantity;

      // item column
      const itemElement = document.createElement("td");
      itemElement.textContent = item.text;

      // delete column with 'Delete' button
      // item id is coded in the 'id' attribute of the button
      const deleteElemement = document.createElement("td");
      const deleteButton = document.createElement("button");
      deleteButton.textContent = "Delete";
      deleteButton.setAttribute("id", "g" + item.id);
      deleteElemement.appendChild(deleteButton);

      // construct to whole row
      const rowElement = document.createElement("tr");
      rowElement.append(quantityElement, itemElement, deleteElemement);

      // add the row to the tbody
      groceryListElement.appendChild(rowElement);
    };

    // get all the grocery list items from the server
    const items = await app.service("items").find();

    // add all the items to the table
    items.data.forEach((e) => {
      appendItem(e);
    });

    // get the references to all the input controls on page
    const quantityField = document.getElementById("quantity");
    const itemField = document.getElementById("item");
    const addButton = document.getElementById("add");

    // add a new item when the 'Add' button is clicked
    addButton.addEventListener("click", async () => {
      // add the new item to the server
      const newItem = await app.service("items").create({
        text: itemField.value,
        quantity: parseInt(quantityField.value),
      });
      // add the item to the table
      appendItem(newItem);
    });
  });
})();
```
