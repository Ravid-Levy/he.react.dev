---
title: lazy
---

<Intro>

`lazy` מאפשרת לדחות טעינה של קוד קומפוננטה עד לרינדור הראשון שלה.

```js
const SomeComponent = lazy(load)
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `lazy(load)` {/*lazy*/}

קראו ל-`lazy` מחוץ לקומפוננטות שלכם כדי להצהיר על קומפוננטת React בטעינה עצלה:

```js
import { lazy } from 'react';

const MarkdownPreview = lazy(() => import('./MarkdownPreview.js'));
```

[ראו דוגמאות נוספות בהמשך.](#usage)

#### Parameters {/*parameters*/}

* `load`: פונקציה שמחזירה [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) או *thenable* אחר (אובייקט דמוי Promise עם מתודת `then`). React לא תקרא ל-`load` עד הפעם הראשונה שבה תנסו לרנדר את הקומפוננטה המוחזרת. אחרי ש-React קוראת ל-`load` בפעם הראשונה, היא תחכה שהפונקציה תיפתר, ואז תרנדר את `.default` של הערך שנפתר כקומפוננטת React. גם ה-Promise המוחזר וגם הערך שנפתר ממנו יישמרו במטמון, כך ש-React לא תקרא ל-`load` יותר מפעם אחת. אם ה-Promise נדחה, React תבצע `throw` לסיבת הדחייה עבור Error Boundary הקרוב כדי שיטפל בה.

#### Returns {/*returns*/}

`lazy` מחזירה קומפוננטת React שאפשר לרנדר בעץ שלכם. בזמן שקוד הקומפוננטה העצלה עדיין נטען, ניסיון לרנדר אותה יגרום ל-*suspend*. השתמשו ב-[`<Suspense>`](/reference/react/Suspense) כדי להציג אינדיקציית טעינה בזמן שהיא נטענת.

---

### פונקציית `load` {/*load*/}

#### Parameters {/*load-parameters*/}

`load` לא מקבלת פרמטרים.

#### Returns {/*load-returns*/}

צריך להחזיר [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) או *thenable* אחר (אובייקט דמוי Promise עם מתודת `then`). בסופו של דבר היא צריכה להיפתר לאובייקט שהמאפיין `.default` שלו הוא סוג קומפוננטת React תקין, כמו פונקציה, קומפוננטת [`memo`](/reference/react/memo), או קומפוננטת [`forwardRef`](/reference/react/forwardRef).

---

## שימוש {/*usage*/}

### טעינת קומפוננטות בעצלות עם Suspense {/*suspense-for-code-splitting*/}

בדרך כלל מייבאים קומפוננטות באמצעות הצהרת [`import`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) סטטית:

```js
import MarkdownPreview from './MarkdownPreview.js';
```

כדי לדחות טעינה של קוד הקומפוננטה הזו עד לרינדור הראשון שלה, החליפו את הייבוא הזה ב:

```js
import { lazy } from 'react';

const MarkdownPreview = lazy(() => import('./MarkdownPreview.js'));
```

הקוד הזה נשען על [`import()` דינמי](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import), שעשוי לדרוש תמיכה מה-bundler או מה-framework שלכם. שימוש בדפוס הזה דורש שהקומפוננטה העצלה שאתם מייבאים יוצאה כ-`default` export.

עכשיו, כשהקוד של הקומפוננטה נטען לפי דרישה, צריך גם לציין מה יוצג בזמן הטעינה. אפשר לעשות זאת על ידי עטיפת הקומפוננטה העצלה או אחד מההורים שלה בתוך גבול [`<Suspense>`](/reference/react/Suspense):

```js {1,4}
<Suspense fallback={<Loading />}>
  <h2>Preview</h2>
  <MarkdownPreview />
 </Suspense>
```

בדוגמה הזו, הקוד של `MarkdownPreview` לא ייטען עד שתנסו לרנדר אותו. אם `MarkdownPreview` עדיין לא נטען, `Loading` יוצג במקומו. נסו לסמן את תיבת הסימון:

<Sandpack>

```js src/App.js
import { useState, Suspense, lazy } from 'react';
import Loading from './Loading.js';

const MarkdownPreview = lazy(() => delayForDemo(import('./MarkdownPreview.js')));

export default function MarkdownEditor() {
  const [showPreview, setShowPreview] = useState(false);
  const [markdown, setMarkdown] = useState('Hello, **world**!');
  return (
    <>
      <textarea value={markdown} onChange={e => setMarkdown(e.target.value)} />
      <label>
        <input type="checkbox" checked={showPreview} onChange={e => setShowPreview(e.target.checked)} />
        Show preview
      </label>
      <hr />
      {showPreview && (
        <Suspense fallback={<Loading />}>
          <h2>Preview</h2>
          <MarkdownPreview markdown={markdown} />
        </Suspense>
      )}
    </>
  );
}

// Add a fixed delay so you can see the loading state
function delayForDemo(promise) {
  return new Promise(resolve => {
    setTimeout(resolve, 2000);
  }).then(() => promise);
}
```

```js src/Loading.js
export default function Loading() {
  return <p><i>Loading...</i></p>;
}
```

```js src/MarkdownPreview.js
import { Remarkable } from 'remarkable';

const md = new Remarkable();

export default function MarkdownPreview({ markdown }) {
  return (
    <div
      className="content"
      dangerouslySetInnerHTML={{__html: md.render(markdown)}}
    />
  );
}
```

```json package.json hidden
{
  "dependencies": {
    "immer": "1.7.3",
    "react": "latest",
    "react-dom": "latest",
    "react-scripts": "latest",
    "remarkable": "2.0.1"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject"
  }
}
```

```css
label {
  display: block;
}

input, textarea {
  margin-bottom: 10px;
}

body {
  min-height: 200px;
}
```

</Sandpack>

הדמו הזה נטען עם השהיה מלאכותית. בפעם הבאה שתבטלו ותסמנו שוב את תיבת הסימון, `Preview` כבר יהיה במטמון, ולכן לא יהיה מצב טעינה. כדי לראות שוב את מצב הטעינה, לחצו על "Reset" ב-sandbox.

[קראו עוד על ניהול מצבי טעינה עם Suspense.](/reference/react/Suspense)

---

## פתרון תקלות {/*troubleshooting*/}

### ה-state של קומפוננטת `lazy` מתאפס באופן לא צפוי {/*my-lazy-components-state-gets-reset-unexpectedly*/}

אל תצהירו על קומפוננטות `lazy` *בתוך* קומפוננטות אחרות:

```js {4-5}
import { lazy } from 'react';

function Editor() {
  // 🔴 Bad: This will cause all state to be reset on re-renders
  const MarkdownPreview = lazy(() => import('./MarkdownPreview.js'));
  // ...
}
```

במקום זאת, תמיד הצהירו עליהן ברמה העליונה של המודול:

```js {3-4}
import { lazy } from 'react';

// ✅ Good: Declare lazy components outside of your components
const MarkdownPreview = lazy(() => import('./MarkdownPreview.js'));

function Editor() {
  // ...
}
```
