---
title: Server React DOM APIs
---

<Intro>

ה-APIs של `react-dom/server` מאפשרים לרנדר קומפוננטות React ל-HTML בצד שרת. ה-APIs האלה משמשים רק בצד שרת, ברמה העליונה של האפליקציה, כדי לייצר את ה-HTML הראשוני. [Framework](/learn/start-a-new-react-project#production-grade-react-frameworks) יכול לקרוא להם עבורכם. רוב הקומפוננטות שלכם לא צריכות לייבא או להשתמש בהם.

</Intro>

---

## Server APIs עבור Node.js Streams {/*server-apis-for-nodejs-streams*/}

המתודות האלה זמינות רק בסביבות עם [Node.js Streams:](https://nodejs.org/api/stream.html)

* [`renderToPipeableStream`](/reference/react-dom/server/renderToPipeableStream) מרנדר עץ React ל-[Node.js Stream](https://nodejs.org/api/stream.html) שניתן לבצע לו piping.
* [`renderToStaticNodeStream`](/reference/react-dom/server/renderToStaticNodeStream) מרנדר עץ React לא אינטראקטיבי ל-[Node.js Readable Stream.](https://nodejs.org/api/stream.html#readable-streams)

---

## Server APIs עבור Web Streams {/*server-apis-for-web-streams*/}

המתודות האלה זמינות רק בסביבות עם [Web Streams](https://developer.mozilla.org/en-US/docs/Web/API/Streams_API), כולל דפדפנים, Deno, וחלק מסביבות edge מודרניות:

* [`renderToReadableStream`](/reference/react-dom/server/renderToReadableStream) מרנדר עץ React ל-[Readable Web Stream.](https://developer.mozilla.org/en-US/docs/Web/API/ReadableStream)

---

## Server APIs עבור סביבות ללא streaming {/*server-apis-for-non-streaming-environments*/}

אפשר להשתמש במתודות האלה בסביבות שלא תומכות ב-streams:

* [`renderToString`](/reference/react-dom/server/renderToString) מרנדר עץ React למחרוזת.
* [`renderToStaticMarkup`](/reference/react-dom/server/renderToStaticMarkup) מרנדר עץ React לא אינטראקטיבי למחרוזת.

יש להן פונקציונליות מוגבלת בהשוואה ל-APIs של streaming.

---

## Server APIs שהוצאו משימוש {/*deprecated-server-apis*/}

<Deprecated>

ה-APIs האלה יוסרו בגרסה ראשית עתידית של React.

</Deprecated>

* [`renderToNodeStream`](/reference/react-dom/server/renderToNodeStream) מרנדר עץ React ל-[Node.js Readable stream.](https://nodejs.org/api/stream.html#readable-streams) (הוצא משימוש.)
