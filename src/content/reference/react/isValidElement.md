---
title: isValidElement
---

<Intro>

`isValidElement` בודקת האם ערך הוא React element.

```js
const isElement = isValidElement(value)
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `isValidElement(value)` {/*isvalidelement*/}

קראו ל-`isValidElement(value)` כדי לבדוק האם `value` הוא React element.

```js
import { isValidElement, createElement } from 'react';

// ✅ React elements
console.log(isValidElement(<p />)); // true
console.log(isValidElement(createElement('p'))); // true

// ❌ Not React elements
console.log(isValidElement(25)); // false
console.log(isValidElement('Hello')); // false
console.log(isValidElement({ age: 42 })); // false
```

[ראו דוגמאות נוספות בהמשך.](#usage)

#### Parameters {/*parameters*/}

* `value`: הערך שברצונכם לבדוק. הוא יכול להיות מכל סוג.

#### Returns {/*returns*/}

`isValidElement` מחזירה `true` אם `value` הוא React element. אחרת היא מחזירה `false`.

#### Caveats {/*caveats*/}

* **רק [JSX tags](/learn/writing-markup-with-jsx) ואובייקטים שמוחזרים מ-[`createElement`](/reference/react/createElement) נחשבים ל-React elements.** למשל, למרות שמספר כמו `42` הוא React *node* תקין (ואפשר להחזיר אותו מקומפוננטה), הוא לא React element תקין. גם מערכים ו-portals שנוצרו עם [`createPortal`](/reference/react-dom/createPortal) *לא* נחשבים React elements.

---

## שימוש {/*usage*/}

### בדיקה אם משהו הוא React element {/*checking-if-something-is-a-react-element*/}

קראו ל-`isValidElement` כדי לבדוק אם ערך כלשהו הוא *React element*.

React elements הם:

- ערכים שמיוצרים מכתיבת [JSX tag](/learn/writing-markup-with-jsx)
- ערכים שמיוצרים מקריאה ל-[`createElement`](/reference/react/createElement)

עבור React elements, `isValidElement` מחזירה `true`:

```js
import { isValidElement, createElement } from 'react';

// ✅ JSX tags are React elements
console.log(isValidElement(<p />)); // true
console.log(isValidElement(<MyComponent />)); // true

// ✅ Values returned by createElement are React elements
console.log(isValidElement(createElement('p'))); // true
console.log(isValidElement(createElement(MyComponent))); // true
```

כל ערך אחר, כמו מחרוזות, מספרים או אובייקטים ומערכים שרירותיים, אינו React element.

עבורם, `isValidElement` מחזירה `false`:

```js
// ❌ These are *not* React elements
console.log(isValidElement(null)); // false
console.log(isValidElement(25)); // false
console.log(isValidElement('Hello')); // false
console.log(isValidElement({ age: 42 })); // false
console.log(isValidElement([<div />, <div />])); // false
console.log(isValidElement(MyComponent)); // false
```

נדיר מאוד שצריך `isValidElement`. לרוב זה שימושי רק כשקוראים ל-API אחר ש*מקבל רק elements* (כמו [`cloneElement`](/reference/react/cloneElement)) ורוצים להימנע משגיאה כשהארגומנט אינו React element.

אלא אם יש לכם סיבה מאוד ספציפית להוסיף בדיקת `isValidElement`, כנראה שאין בה צורך.

<DeepDive>

#### React elements לעומת React nodes {/*react-elements-vs-react-nodes*/}

כשכותבים קומפוננטה, אפשר להחזיר ממנה כל סוג של *React node*:

```js
function MyComponent() {
  // ... you can return any React node ...
}
```

React node יכול להיות:

- React element שנוצר כמו `<div />` או `createElement('div')`
- portal שנוצר עם [`createPortal`](/reference/react-dom/createPortal)
- מחרוזת
- מספר
- `true`, `false`, `null`, או `undefined` (שאינם מוצגים)
- מערך של React nodes אחרים

**שימו לב ש-`isValidElement` בודקת האם הארגומנט הוא *React element*, ולא האם הוא React node.** לדוגמה, `42` אינו React element תקין. עם זאת, הוא React node תקין לחלוטין:

```js
function MyComponent() {
  return 42; // It's ok to return a number from component
}
```

לכן לא כדאי להשתמש ב-`isValidElement` כדי לבדוק אם משהו ניתן לרינדור.

</DeepDive>
