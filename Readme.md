# Smart Bookmark Manager
---
I have been using google chrome's bookmark manager a lot because it has syncing functionality which i really like .
But using Google Chrome's Bookmark manager is usually very slow process since i have a lots of folder and links . 
Thats why i wanted to create faster and more easily navigated Bookmark manager.



---
This project is using
- https://github.com/Kocal/vue-web-extension
- VueJs
- Icomoon

--- 
## Usage
```
npm i
npm run watch:dev
```
After running `npm run watch:dev` you can import dist folder to google chrome extensions and start develop. Any change you made on code can be reflected on browser. But when you want to deploy app use `npm run build`.

### `npm run build`

Build the extension into `dist` folder for **production**.

### `npm run build:dev`

Build the extension into `dist` folder for **development**.

### `npm run watch`

Watch for modifications then run `npm run build`.

### `npm run watch:dev`

Watch for modifications then run `npm run build:dev`.
