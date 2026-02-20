---
title: createFactory
---

<Deprecated>

ה-API הזה יוסר בגרסה ראשית עתידית של React. [ראו חלופות.](#alternatives)

</Deprecated>

<Intro>

`createFactory` מאפשרת ליצור פונקציה שמייצרת React elements מסוג נתון.

```js
const factory = createFactory(type)
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `createFactory(type)` {/*createfactory*/}

קראו ל-`createFactory(type)` כדי ליצור פונקציית factory שמייצרת React elements מסוג `type` נתון.

```js
import { createFactory } from 'react';

const button = createFactory('button');
```

ואז אפשר להשתמש בה כדי ליצור React elements בלי JSX:

```js
export default function App() {
  return button({
    onClick: () => {
      alert('Clicked!')
    }
  }, 'Click me');
}
```

[ראו דוגמאות נוספות בהמשך.](#usage)

#### Parameters {/*parameters*/}

* `type`: הארגומנט `type` חייב להיות סוג קומפוננטת React תקין. למשל, מחרוזת שם תגית (כמו `'div'` או `'span'`), או קומפוננטת React (פונקציה, מחלקה, או קומפוננטה מיוחדת כמו [`Fragment`](/reference/react/Fragment)).

#### Returns {/*returns*/}

מחזירה פונקציית factory. פונקציית ה-factory הזו מקבלת אובייקט `props` כארגומנט ראשון, אחריו רשימת ארגומנטים `...children`, ומחזירה React element עם ה-`type`, ה-`props` וה-`children` הנתונים.

---

## שימוש {/*usage*/}

### יצירת React elements עם factory {/*creating-react-elements-with-a-factory*/}

למרות שרוב פרויקטי React משתמשים ב-[JSX](/learn/writing-markup-with-jsx) כדי לתאר את ממשק המשתמש, JSX אינה חובה. בעבר, `createFactory` הייתה אחת הדרכים לתאר ממשק משתמש בלי JSX.

קראו ל-`createFactory` כדי ליצור *פונקציית factory* עבור סוג אלמנט מסוים כמו `'button'`:

```js
import { createFactory } from 'react';

const button = createFactory('button');
```

קריאה לפונקציית ה-factory הזו תייצר React elements עם ה-props וה-children שסיפקתם:

<Sandpack>

```js src/App.js
import { createFactory } from 'react';

const button = createFactory('button');

export default function App() {
  return button({
    onClick: () => {
      alert('Clicked!')
    }
  }, 'Click me');
}
```

</Sandpack>

כך השתמשו ב-`createFactory` כחלופה ל-JSX. אבל `createFactory` הוצאה משימוש, ולא כדאי לקרוא לה בקוד חדש. ראו בהמשך איך לבצע מיגרציה מ-`createFactory`.

---

## חלופות {/*alternatives*/}

### העתקת `createFactory` לתוך הפרויקט שלכם {/*copying-createfactory-into-your-project*/}

אם בפרויקט שלכם יש הרבה קריאות ל-`createFactory`, העתיקו את המימוש הבא של `createFactory.js` לתוך הפרויקט:

<Sandpack>

```js src/App.js
import { createFactory } from './createFactory.js';

const button = createFactory('button');

export default function App() {
  return button({
    onClick: () => {
      alert('Clicked!')
    }
  }, 'Click me');
}
```

```js src/createFactory.js
import { createElement } from 'react';

export function createFactory(type) {
  return createElement.bind(null, type);
}
```

</Sandpack>

כך אפשר להשאיר את כל הקוד ללא שינוי מלבד הייבואים.

---

### החלפת `createFactory` ב-`createElement` {/*replacing-createfactory-with-createelement*/}

אם יש לכם מעט קריאות ל-`createFactory` ואתם לא מתנגדים לבצע המרה ידנית, ואתם לא רוצים להשתמש ב-JSX, אפשר להחליף כל קריאה לפונקציית factory בקריאה ל-[`createElement`](/reference/react/createElement). למשל, אפשר להחליף את הקוד הזה:

```js {1,3,6}
import { createFactory } from 'react';

const button = createFactory('button');

export default function App() {
  return button({
    onClick: () => {
      alert('Clicked!')
    }
  }, 'Click me');
}
```

בקוד הזה:


```js {1,4}
import { createElement } from 'react';

export default function App() {
  return createElement('button', {
    onClick: () => {
      alert('Clicked!')
    }
  }, 'Click me');
}
```

הנה דוגמה מלאה לשימוש ב-React ללא JSX:

<Sandpack>

```js src/App.js
import { createElement } from 'react';

export default function App() {
  return createElement('button', {
    onClick: () => {
      alert('Clicked!')
    }
  }, 'Click me');
}
```

</Sandpack>

---

### החלפת `createFactory` ב-JSX {/*replacing-createfactory-with-jsx*/}

לבסוף, אפשר להשתמש ב-JSX במקום `createFactory`. זו הדרך הנפוצה ביותר להשתמש ב-React:

<Sandpack>

```js src/App.js
export default function App() {
  return (
    <button onClick={() => {
      alert('Clicked!');
    }}>
      Click me
    </button>
  );
};
```

</Sandpack>

<Pitfall>

לפעמים קוד קיים עשוי להעביר משתנה כלשהו כ-`type` במקום קבוע כמו `'button'`:

```js {3}
function Heading({ isSubheading, ...props }) {
  const type = isSubheading ? 'h2' : 'h1';
  const factory = createFactory(type);
  return factory(props);
}
```

כדי לעשות את אותו הדבר ב-JSX, צריך לשנות את שם המשתנה כדי שיתחיל באות גדולה, למשל `Type`:

```js {2,3}
function Heading({ isSubheading, ...props }) {
  const Type = isSubheading ? 'h2' : 'h1';
  return <Type {...props} />;
}
```

אחרת React תפרש `<type>` כתגית HTML מובנית כי היא באותיות קטנות.

</Pitfall>
