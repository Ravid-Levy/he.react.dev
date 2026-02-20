---
title: renderToNodeStream
---

<Deprecated>

ה-API הזה יוסר בגרסה ראשית עתידית של React. השתמשו ב-[`renderToPipeableStream`](/reference/react-dom/server/renderToPipeableStream) במקום.

</Deprecated>

<Intro>

`renderToNodeStream` מרנדר עץ React ל-[Node.js Readable Stream.](https://nodejs.org/api/stream.html#readable-streams)

```js
const stream = renderToNodeStream(reactNode, options?)
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `renderToNodeStream(reactNode, options?)` {/*rendertonodestream*/}

בשרת, קראו ל-`renderToNodeStream` כדי לקבל [Node.js Readable Stream](https://nodejs.org/api/stream.html#readable-streams) שאפשר לבצע לו pipe לתגובה.

```js
import { renderToNodeStream } from 'react-dom/server';

const stream = renderToNodeStream(<App />);
stream.pipe(response);
```

בצד הלקוח, קראו ל-[`hydrateRoot`](/reference/react-dom/client/hydrateRoot) כדי להפוך את ה-HTML שנוצר בשרת לאינטראקטיבי.

[ראו דוגמאות נוספות בהמשך.](#usage)

#### Parameters {/*parameters*/}

* `reactNode`: React node שברצונכם לרנדר ל-HTML. לדוגמה, JSX element כמו `<App />`.

* **אופציונלי** `options`: אובייקט עבור רינדור שרת.
  * **אופציונלי** `identifierPrefix`: מחרוזת קידומת ש-React משתמשת בה עבור מזהים שנוצרים על ידי [`useId`.](/reference/react/useId) שימושי למניעת התנגשויות כשמשתמשים בכמה roots באותו עמוד. חייב להיות זהה לקידומת שמועברת ל-[`hydrateRoot`.](/reference/react-dom/client/hydrateRoot#parameters)

#### Returns {/*returns*/}

[Node.js Readable Stream](https://nodejs.org/api/stream.html#readable-streams) שמפיק מחרוזת HTML.

#### Caveats {/*caveats*/}

* המתודה הזו תחכה שכל [גבולות Suspense](/reference/react/Suspense) יסתיימו לפני החזרת פלט.

* החל מ-React 18, המתודה הזו מאחסנת את כל הפלט בבאפר, ולכן בפועל אינה מספקת יתרונות streaming. לכן מומלץ לעבור ל-[`renderToPipeableStream`](/reference/react-dom/server/renderToPipeableStream).

* ה-stream המוחזר הוא byte stream בקידוד utf-8. אם צריך stream בקידוד אחר, אפשר לבדוק פרויקט כמו [iconv-lite](https://www.npmjs.com/package/iconv-lite), שמספק transform streams להמרת קידוד טקסט.

---

## שימוש {/*usage*/}

### רינדור עץ React כ-HTML ל-Node.js Readable Stream {/*rendering-a-react-tree-as-html-to-a-nodejs-readable-stream*/}

קראו ל-`renderToNodeStream` כדי לקבל [Node.js Readable Stream](https://nodejs.org/api/stream.html#readable-streams) שאפשר לבצע לו pipe לתגובת השרת:

```js {5-6}
import { renderToNodeStream } from 'react-dom/server';

// The route handler syntax depends on your backend framework
app.use('/', (request, response) => {
  const stream = renderToNodeStream(<App />);
  stream.pipe(response);
});
```

ה-stream יפיק את פלט ה-HTML הראשוני הלא אינטראקטיבי של קומפוננטות React שלכם. בצד הלקוח תצטרכו לקרוא ל-[`hydrateRoot`](/reference/react-dom/client/hydrateRoot) כדי לבצע *hydration* ל-HTML שנוצר בשרת ולהפוך אותו לאינטראקטיבי.
