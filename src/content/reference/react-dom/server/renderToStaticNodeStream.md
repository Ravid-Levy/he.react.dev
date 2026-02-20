---
title: renderToStaticNodeStream
---

<Intro>

`renderToStaticNodeStream` מרנדר עץ React לא אינטראקטיבי ל-[Node.js Readable Stream.](https://nodejs.org/api/stream.html#readable-streams)

```js
const stream = renderToStaticNodeStream(reactNode, options?)
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `renderToStaticNodeStream(reactNode, options?)` {/*rendertostaticnodestream*/}

בשרת, קראו ל-`renderToStaticNodeStream` כדי לקבל [Node.js Readable Stream](https://nodejs.org/api/stream.html#readable-streams).

```js
import { renderToStaticNodeStream } from 'react-dom/server';

const stream = renderToStaticNodeStream(<Page />);
stream.pipe(response);
```

[ראו דוגמאות נוספות בהמשך.](#usage)

ה-stream יפיק פלט HTML לא אינטראקטיבי של קומפוננטות React שלכם.

#### Parameters {/*parameters*/}

* `reactNode`: React node שברצונכם לרנדר ל-HTML. לדוגמה, JSX element כמו `<Page />`.

* **אופציונלי** `options`: אובייקט עבור רינדור שרת.
  * **אופציונלי** `identifierPrefix`: מחרוזת קידומת ש-React משתמשת בה עבור מזהים שנוצרים על ידי [`useId`.](/reference/react/useId) שימושי למניעת התנגשויות כשמשתמשים בכמה roots באותו עמוד.

#### Returns {/*returns*/}

[Node.js Readable Stream](https://nodejs.org/api/stream.html#readable-streams) שמפיק מחרוזת HTML. אי אפשר לבצע hydration ל-HTML שמתקבל.

#### Caveats {/*caveats*/}

* אי אפשר לבצע hydration לפלט של `renderToStaticNodeStream`.

* המתודה הזו תחכה שכל [גבולות Suspense](/reference/react/Suspense) יסתיימו לפני החזרת פלט.

* החל מ-React 18, המתודה הזו מאחסנת את כל הפלט בבאפר, ולכן בפועל אינה מספקת יתרונות streaming.

* ה-stream המוחזר הוא byte stream בקידוד utf-8. אם צריך stream בקידוד אחר, אפשר לבדוק פרויקט כמו [iconv-lite](https://www.npmjs.com/package/iconv-lite), שמספק transform streams להמרת קידוד טקסט.

---

## שימוש {/*usage*/}

### רינדור עץ React כ-HTML סטטי ל-Node.js Readable Stream {/*rendering-a-react-tree-as-static-html-to-a-nodejs-readable-stream*/}

קראו ל-`renderToStaticNodeStream` כדי לקבל [Node.js Readable Stream](https://nodejs.org/api/stream.html#readable-streams) שאפשר לבצע לו pipe לתגובת השרת:

```js {5-6}
import { renderToStaticNodeStream } from 'react-dom/server';

// The route handler syntax depends on your backend framework
app.use('/', (request, response) => {
  const stream = renderToStaticNodeStream(<Page />);
  stream.pipe(response);
});
```

ה-stream יפיק את פלט ה-HTML הראשוני הלא אינטראקטיבי של קומפוננטות React שלכם.

<Pitfall>

המתודה הזו מרנדרת **HTML לא אינטראקטיבי שאי אפשר לבצע לו hydration.** זה שימושי אם רוצים להשתמש ב-React כמחולל עמודים סטטיים פשוט, או אם מרנדרים תוכן סטטי לחלוטין כמו אימיילים.

אפליקציות אינטראקטיביות צריכות להשתמש ב-[`renderToPipeableStream`](/reference/react-dom/server/renderToPipeableStream) בצד השרת וב-[`hydrateRoot`](/reference/react-dom/client/hydrateRoot) בצד הלקוח.

</Pitfall>
