---
title: renderToStaticMarkup
---

<Intro>

`renderToStaticMarkup` מרנדר עץ React לא אינטראקטיבי למחרוזת HTML.

```js
const html = renderToStaticMarkup(reactNode, options?)
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `renderToStaticMarkup(reactNode, options?)` {/*rendertostaticmarkup*/}

בצד השרת, קראו ל-`renderToStaticMarkup` כדי לרנדר את האפליקציה שלכם ל-HTML.

```js
import { renderToStaticMarkup } from 'react-dom/server';

const html = renderToStaticMarkup(<Page />);
```

הפונקציה תייצר פלט HTML לא אינטראקטיבי של קומפוננטות React שלכם.

[ראו דוגמאות נוספות בהמשך.](#usage)

#### Parameters {/*parameters*/}

* `reactNode`: React node שברצונכם לרנדר ל-HTML. למשל, JSX node כמו `<Page />`.
* **אופציונלי** `options`: אובייקט עבור רינדור שרת.
  * **אופציונלי** `identifierPrefix`: מחרוזת קידומת ש-React משתמשת בה עבור מזהים שנוצרים על ידי [`useId`.](/reference/react/useId) שימושי למניעת התנגשויות כשמשתמשים בכמה roots באותו עמוד.

#### Returns {/*returns*/}

מחרוזת HTML.

#### Caveats {/*caveats*/}

* אי אפשר לבצע hydration לפלט של `renderToStaticMarkup`.

* ל-`renderToStaticMarkup` יש תמיכה מוגבלת ב-Suspense. אם קומפוננטה מבצעת suspend, `renderToStaticMarkup` שולחת מיד את ה-fallback שלה כ-HTML.

* `renderToStaticMarkup` עובדת גם בדפדפן, אבל לא מומלץ להשתמש בה בקוד לקוח. אם צריך לרנדר קומפוננטה ל-HTML בדפדפן, [קבלו את ה-HTML על ידי רינדור ל-DOM node.](/reference/react-dom/server/renderToString#removing-rendertostring-from-the-client-code)

---

## שימוש {/*usage*/}

### רינדור עץ React לא אינטראקטיבי כ-HTML למחרוזת {/*rendering-a-non-interactive-react-tree-as-html-to-a-string*/}

קראו ל-`renderToStaticMarkup` כדי לרנדר את האפליקציה למחרוזת HTML שאפשר לשלוח בתגובת השרת:

```js {5-6}
import { renderToStaticMarkup } from 'react-dom/server';

// The route handler syntax depends on your backend framework
app.use('/', (request, response) => {
  const html = renderToStaticMarkup(<Page />);
  response.send(html);
});
```

כך יתקבל פלט ה-HTML הראשוני הלא אינטראקטיבי של קומפוננטות React שלכם.

<Pitfall>

המתודה הזו מרנדרת **HTML לא אינטראקטיבי שאי אפשר לבצע לו hydration.** זה שימושי אם רוצים להשתמש ב-React כמחולל עמודים סטטיים פשוט, או אם מרנדרים תוכן סטטי לחלוטין כמו אימיילים.

אפליקציות אינטראקטיביות צריכות להשתמש ב-[`renderToString`](/reference/react-dom/server/renderToString) בצד השרת וב-[`hydrateRoot`](/reference/react-dom/client/hydrateRoot) בצד הלקוח.

</Pitfall>
