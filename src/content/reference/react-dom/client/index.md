---
title: Client React DOM APIs
---

<Intro>

ה-APIs של `react-dom/client` מאפשרים לרנדר קומפוננטות React בצד לקוח (בדפדפן). בדרך כלל משתמשים ב-APIs האלה ברמה העליונה של האפליקציה כדי לאתחל את עץ ה-React. [Framework](/learn/start-a-new-react-project#production-grade-react-frameworks) יכול לקרוא להם עבורכם. רוב הקומפוננטות שלכם לא צריכות לייבא או להשתמש בהם.

</Intro>

---

## Client APIs {/*client-apis*/}

* [`createRoot`](/reference/react-dom/client/createRoot) מאפשר ליצור root להצגת קומפוננטות React בתוך DOM node בדפדפן.
* [`hydrateRoot`](/reference/react-dom/client/hydrateRoot) מאפשר להציג קומפוננטות React בתוך DOM node בדפדפן, כאשר תוכן ה-HTML שלו נוצר קודם על ידי [`react-dom/server`.](/reference/react-dom/server)

---

## תמיכה בדפדפנים {/*browser-support*/}

React תומכת בכל הדפדפנים הנפוצים, כולל Internet Explorer 9 ומעלה. עבור דפדפנים ישנים יותר כמו IE 9 ו-IE 10 נדרשים polyfills מסוימים.
