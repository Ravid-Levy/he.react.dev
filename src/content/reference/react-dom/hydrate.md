---
title: hydrate
---

<Deprecated>

ה-API הזה יוסר בגרסה ראשית עתידית של React.

ב-React 18, `hydrate` הוחלפה ב-[`hydrateRoot`.](/reference/react-dom/client/hydrateRoot) שימוש ב-`hydrate` ב-React 18 יציג אזהרה שהאפליקציה שלכם תתנהג כאילו היא רצה על React 17. מידע נוסף [כאן.](/blog/2022/03/08/react-18-upgrade-guide#updates-to-client-rendering-apis)

</Deprecated>

<Intro>

`hydrate` מאפשרת להציג קומפוננטות React בתוך DOM node של דפדפן, כשהתוכן ה-HTML שלו נוצר קודם על ידי [`react-dom/server`](/reference/react-dom/server) ב-React 17 ומטה.

```js
hydrate(reactNode, domNode, callback?)
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `hydrate(reactNode, domNode, callback?)` {/*hydrate*/}

ב-React 17 ומטה, קראו ל-`hydrate` כדי "לחבר" את React ל-HTML קיים שכבר רונדר על ידי React בסביבת שרת.

```js
import { hydrate } from 'react-dom';

hydrate(reactNode, domNode);
```

React תתחבר ל-HTML שקיים בתוך `domNode`, ותיקח שליטה על ניהול ה-DOM שבתוכו. אפליקציה שבנויה לגמרי ב-React תכלול בדרך כלל קריאת `hydrate` אחת בלבד עם קומפוננטת השורש שלה.

[ראו דוגמאות נוספות בהמשך.](#usage)

#### Parameters {/*parameters*/}

* `reactNode`: ‏"React node" ששימש לרינדור ה-HTML הקיים. בדרך כלל זו חתיכת JSX כמו `<App />` שרונדרה עם מתודת `ReactDOM Server` כמו `renderToString(<App />)` ב-React 17.

* `domNode`: [אלמנט DOM](https://developer.mozilla.org/en-US/docs/Web/API/Element) שרונדר כ-root element בשרת.

* **אופציונלי**: `callback`: פונקציה. אם הועברה, React תקרא לה אחרי שהקומפוננטה עוברת hydration.

#### Returns {/*returns*/}

`hydrate` מחזירה null.

#### Caveats {/*caveats*/}
* `hydrate` מצפה שהתוכן המרונדר יהיה זהה לתוכן שרונדר בשרת. React יכולה לתקן הבדלים בתוכן טקסט, אבל צריך להתייחס לחוסר התאמות כבאגים ולתקן אותם.
* במצב פיתוח, React מציגה אזהרות על חוסר התאמות בזמן hydration. אין הבטחה שהבדלי attributes יתוקנו במקרה של חוסר התאמה. זה חשוב מבחינת ביצועים כי ברוב האפליקציות חוסר התאמות נדיר, ולכן אימות מלא של כל ה-markup יהיה יקר מדי.
* סביר שתהיה לכם רק קריאת `hydrate` אחת באפליקציה. אם אתם משתמשים ב-framework, ייתכן שהוא יבצע את הקריאה הזו בשבילכם.
* אם האפליקציה שלכם מרונדרת בלקוח בלבד בלי HTML קיים, שימוש ב-`hydrate()` לא נתמך. השתמשו ב-[render()](/reference/react-dom/render) (עבור React 17 ומטה) או ב-[createRoot()](/reference/react-dom/client/createRoot) (עבור React 18+) במקום.

---

## שימוש {/*usage*/}

קראו ל-`hydrate` כדי לחבר <CodeStep step={1}>קומפוננטת React</CodeStep> לתוך <CodeStep step={2}>DOM node של דפדפן שרונדר בשרת</CodeStep>.

```js [[1, 3, "<App />"], [2, 3, "document.getElementById('root')"]]
import { hydrate } from 'react-dom';

hydrate(<App />, document.getElementById('root'));
```

שימוש ב-`hydrate()` לרינדור אפליקציית לקוח בלבד (אפליקציה ללא HTML מרונדר שרת) לא נתמך. השתמשו ב-[`render()`](/reference/react-dom/render) (ב-React 17 ומטה) או ב-[`createRoot()`](/reference/react-dom/client/createRoot) (ב-React 18+) במקום.

### ביצוע hydration ל-HTML מרונדר שרת {/*hydrating-server-rendered-html*/}

ב-React, "hydration" הוא האופן שבו React "מתחברת" ל-HTML קיים שכבר רונדר על ידי React בסביבת שרת. בזמן hydration, React תנסה לחבר event listeners ל-markup הקיים ולקחת שליטה על רינדור האפליקציה בצד הלקוח.

באפליקציות שבנויות לגמרי עם React, **בדרך כלל תבצעו hydration רק ל"root" אחד, פעם אחת בזמן ההפעלה לכל האפליקציה**.

<Sandpack>

```html public/index.html
<!--
  HTML content inside <div id="root">...</div>
  was generated from App by react-dom/server.
-->
<div id="root"><h1>Hello, world!</h1></div>
```

```js src/index.js active
import './styles.css';
import { hydrate } from 'react-dom';
import App from './App.js';

hydrate(<App />, document.getElementById('root'));
```

```js src/App.js
export default function App() {
  return <h1>Hello, world!</h1>;
}
```

</Sandpack>

בדרך כלל לא צריך לקרוא ל-`hydrate` שוב או במקומות נוספים. מהנקודה הזו React תנהל את ה-DOM של האפליקציה. כדי לעדכן את ה-UI, הקומפוננטות שלכם [ישתמשו ב-state.](/reference/react/useState)

למידע נוסף על hydration, ראו את התיעוד של [`hydrateRoot`.](/reference/react-dom/client/hydrateRoot)

---

### השתקת אזהרות hydration mismatch בלתי נמנעות {/*suppressing-unavoidable-hydration-mismatch-errors*/}

אם attribute של אלמנט בודד או תוכן טקסט שלו שונים בהכרח בין השרת ללקוח (למשל timestamp), אפשר להשתיק את אזהרת ה-hydration mismatch.

כדי להשתיק אזהרות hydration על אלמנט, הוסיפו `suppressHydrationWarning={true}`:

<Sandpack>

```html public/index.html
<!--
  HTML content inside <div id="root">...</div>
  was generated from App by react-dom/server.
-->
<div id="root"><h1>Current Date: 01/01/2020</h1></div>
```

```js src/index.js
import './styles.css';
import { hydrate } from 'react-dom';
import App from './App.js';

hydrate(<App />, document.getElementById('root'));
```

```js src/App.js active
export default function App() {
  return (
    <h1 suppressHydrationWarning={true}>
      Current Date: {new Date().toLocaleDateString()}
    </h1>
  );
}
```

</Sandpack>

זה עובד לעומק של רמה אחת בלבד, ומיועד כ-escape hatch. אל תשתמשו בזה מעבר לנדרש. אלא אם מדובר בתוכן טקסט, React עדיין לא תנסה לתקן אותו, ולכן הוא עלול להישאר לא עקבי עד עדכונים עתידיים.

---

### טיפול בתוכן שונה בין לקוח לשרת {/*handling-different-client-and-server-content*/}

אם אתם צריכים בכוונה לרנדר משהו שונה בשרת ובלקוח, אפשר לבצע רינדור בשני מעברים. קומפוננטות שמרנדרות משהו שונה בלקוח יכולות לקרוא [משתנה state](/reference/react/useState) כמו `isClient`, שאפשר לקבוע ל-`true` בתוך [effect](/reference/react/useEffect):

<Sandpack>

```html public/index.html
<!--
  HTML content inside <div id="root">...</div>
  was generated from App by react-dom/server.
-->
<div id="root"><h1>Is Server</h1></div>
```

```js src/index.js
import './styles.css';
import { hydrate } from 'react-dom';
import App from './App.js';

hydrate(<App />, document.getElementById('root'));
```

```js src/App.js active
import { useState, useEffect } from "react";

export default function App() {
  const [isClient, setIsClient] = useState(false);

  useEffect(() => {
    setIsClient(true);
  }, []);

  return (
    <h1>
      {isClient ? 'Is Client' : 'Is Server'}
    </h1>
  );
}
```

</Sandpack>

כך מעבר הרינדור הראשוני ירנדר את אותו תוכן כמו בשרת וימנע חוסר התאמות, אבל מעבר נוסף יתרחש באופן סינכרוני מיד אחרי hydration.

<Pitfall>

הגישה הזו הופכת hydration לאיטית יותר כי הקומפוננטות צריכות לרנדר פעמיים. שימו לב לחוויית המשתמש בחיבורים איטיים. קוד JavaScript עלול להיטען הרבה אחרי רינדור ה-HTML הראשוני, ולכן רינדור UI שונה מיד אחרי hydration עלול להרגיש קופצני למשתמש.

</Pitfall>
