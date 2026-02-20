---
title: unmountComponentAtNode
---

<Deprecated>

ה-API הזה יוסר בגרסה ראשית עתידית של React.

ב-React 18, `unmountComponentAtNode` הוחלפה ב-[`root.unmount()`](/reference/react-dom/client/createRoot#root-unmount).

</Deprecated>

<Intro>

`unmountComponentAtNode` מסירה קומפוננטת React שהורכבה מה-DOM.

```js
unmountComponentAtNode(domNode)
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `unmountComponentAtNode(domNode)` {/*unmountcomponentatnode*/}

קראו ל-`unmountComponentAtNode` כדי להסיר קומפוננטת React שהורכבה מה-DOM ולנקות את event handlers וה-state שלה.

```js
import { unmountComponentAtNode } from 'react-dom';

const domNode = document.getElementById('root');
render(<App />, domNode);

unmountComponentAtNode(domNode);
```

[ראו דוגמאות נוספות בהמשך.](#usage)

#### Parameters {/*parameters*/}

* `domNode`: [אלמנט DOM.](https://developer.mozilla.org/en-US/docs/Web/API/Element) React תסיר ממנו קומפוננטת React שהורכבה.

#### Returns {/*returns*/}

`unmountComponentAtNode` מחזירה `true` אם קומפוננטה הוסרה ו-`false` אחרת.

---

## שימוש {/*usage*/}

קראו ל-`unmountComponentAtNode` כדי להסיר <CodeStep step={1}>קומפוננטת React שהורכבה</CodeStep> מ-<CodeStep step={2}>DOM node בדפדפן</CodeStep>, ולנקות את event handlers וה-state שלה.

```js [[1, 5, "<App />"], [2, 5, "rootNode"], [2, 8, "rootNode"]]
import { render, unmountComponentAtNode } from 'react-dom';
import App from './App.js';

const rootNode = document.getElementById('root');
render(<App />, rootNode);

// ...
unmountComponentAtNode(rootNode);
```


### הסרת אפליקציית React מאלמנט DOM {/*removing-a-react-app-from-a-dom-element*/}

לעיתים תרצו "לפזר" React בתוך עמוד קיים, או עמוד שלא נכתב כולו ב-React. במקרים כאלה ייתכן שתצטרכו "לעצור" את אפליקציית React, על ידי הסרת כל ה-UI, ה-state וה-listeners מה-DOM node שאליו היא רונדרה.

בדוגמה הזו, לחיצה על "Render React App" תרנדר אפליקציית React. לחצו על "Unmount React App" כדי להשמיד אותה:

<Sandpack>

```html index.html
<!DOCTYPE html>
<html>
  <head><title>My app</title></head>
  <body>
    <button id='render'>Render React App</button>
    <button id='unmount'>Unmount React App</button>
    <!-- This is the React App node -->
    <div id='root'></div>
  </body>
</html>
```

```js src/index.js active
import './styles.css';
import { render, unmountComponentAtNode } from 'react-dom';
import App from './App.js';

const domNode = document.getElementById('root');

document.getElementById('render').addEventListener('click', () => {
  render(<App />, domNode);
});

document.getElementById('unmount').addEventListener('click', () => {
  unmountComponentAtNode(domNode);
});
```

```js src/App.js
export default function App() {
  return <h1>Hello, world!</h1>;
}
```

</Sandpack>
