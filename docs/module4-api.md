---
sidebar_label: "Module 4 - Feathers API Demo"
sidebar_position: 4
---

# Module 4 - Feathers API Demo

Here is the extra `index.html` code for the space flight news article searc:

```html
<p>
  <label>Search term</label>
  <input type="text" id="search" />
</p>

<button id="findarticles">Find Articles</button>
```

Here is the extra client-side code for `app.js` to do the article search:

```javascript
// search for articles when the 'Find Articles' button is clicked
findButton.addEventListener('click', async () => {
// add the new item to the server
const articles = await app.service('articles').find({
  query: {
    search: searchField.value
  }
})
// log the results
console.log(articles)
```

Here is the complete implementation of the articles service on the server side
in `src/services/articles/articles.class.js`:

```javascript
// This is a skeleton for a custom service class. Remove or add the methods you need here
import fetch from "node-fetch";

export class ArticleService {
  constructor(options) {
    this.options = options;
  }

  async find(_params) {
    console.log(this.options.app.get("someapisecret"));
    console.log(_params);
    const response = await fetch(
      `https://api.spaceflightnewsapi.net/v4/articles/?search=${_params.query.search}`
    );
    const data = await response.json();
    return data.results;
  }

  async get(id, _params) {
    return {
      id: 0,
      text: `A new message with ID: ${id}!`,
    };
  }
  async create(data, params) {
    if (Array.isArray(data)) {
      return Promise.all(data.map((current) => this.create(current, params)));
    }

    return {
      id: 0,
      ...data,
    };
  }

  // This method has to be added to the 'methods' option to make it available to clients
  async update(id, data, _params) {
    return {
      id: 0,
      ...data,
    };
  }

  async patch(id, data, _params) {
    return {
      id: 0,
      text: `Fallback for ${id}`,
      ...data,
    };
  }

  async remove(id, _params) {
    return {
      id: 0,
      text: "removed",
    };
  }
}

export const getOptions = (app) => {
  return { app };
};
```
