---
title: "'use server'"
titleForTitleTag: "'use server' directive"
canary: true
---

<Canary>

`'use server'` נדרש רק אם אתם [משתמשים ב-React Server Components](/learn/start-a-new-react-project#bleeding-edge-react-frameworks) או בונים ספרייה שתואמת אליהם.

</Canary>


<Intro>

`'use server'` מסמן פונקציות בצד השרת שאפשר לקרוא להן מקוד בצד הלקוח.

</Intro>

<InlineToc />

---

## עיון ב-API {/*reference*/}

### `'use server'` {/*use-server*/}

הוסיפו `'use server'` בתחילת גוף של פונקציה אסינכרונית כדי לסמן אותה כפונקציה שהלקוח יכול לקרוא לה. לפונקציות האלו אנחנו קוראים _Server Actions_.

```js {2}
async function addToCart(data) {
  'use server';
  // ...
}
```

כשקוראים ל-Server Action מהלקוח, מתבצעת בקשת רשת לשרת שכוללת עותק מסודר (serialized) של כל הארגומנטים שהועברו. אם ה-Server Action מחזיר ערך, הערך הזה יעבור serialization ויוחזר ללקוח.

במקום לסמן כל פונקציה בנפרד עם `'use server'`, אפשר להוסיף את ההנחיה בראש קובץ כדי לסמן שכל ה-exports בקובץ הם Server Actions שאפשר להשתמש בהם מכל מקום, כולל ייבוא מתוך קוד לקוח.

#### הבהרות {/*caveats*/}
* `'use server'` חייב להופיע ממש בתחילת הפונקציה או המודול, לפני כל קוד אחר כולל imports (מותרות הערות מעל ההנחיה). יש לכתוב אותו במרכאות יחידות או כפולות, לא עם backticks.
* אפשר להשתמש ב-`'use server'` רק בקבצים שרצים בצד השרת. את ה-Server Actions שנוצרים אפשר להעביר ל-Client Components דרך props. ראו [types נתמכים ל-serialization](#serializable-parameters-and-return-values).
* כדי לייבא Server Action מתוך [קוד לקוח](/reference/react/use-client), ההנחיה חייבת להיות ברמת מודול.
* מכיוון שקריאות הרשת מתחת למכסה המנוע הן תמיד אסינכרוניות, אפשר להשתמש ב-`'use server'` רק על פונקציות async.
* התייחסו תמיד לארגומנטים של Server Actions כקלט לא מהימן ובצעו הרשאה לכל שינוי מצב. ראו [שיקולי אבטחה](#security).
* מומלץ לקרוא ל-Server Actions בתוך [transition](/reference/react/useTransition). Server Actions שמועברים ל-[`<form action>`](/reference/react-dom/components/form#props) או ל-[`formAction`](/reference/react-dom/components/input#props) ירוצו אוטומטית בתוך transition.
* Server Actions מיועדים למוטציות שמעדכנות מצב בצד השרת; הם לא מומלצים לשליפת נתונים. בהתאם לכך, frameworks שמממשים Server Actions לרוב מעבדים פעולה אחת בכל פעם ואין להם דרך למטמן את ערך החזרה.

### שיקולי אבטחה {/*security*/}

הארגומנטים ל-Server Actions נשלטים לגמרי על ידי הלקוח. מטעמי אבטחה, התייחסו אליהם תמיד כקלט לא מהימן, ודאו שאתם מאמתים ומבצעים escaping לארגומנטים לפי הצורך.

בכל Server Action, ודאו שהמשתמש המחובר מורשה לבצע את הפעולה.

<Wip>

כדי למנוע שליחה של מידע רגיש מתוך Server Action, קיימים APIs ניסיוניים מסוג taint שמונעים העברת ערכים ייחודיים ואובייקטים לקוד לקוח.

ראו [experimental_taintUniqueValue](/reference/react/experimental_taintUniqueValue) ו-[experimental_taintObjectReference](/reference/react/experimental_taintObjectReference).

</Wip>

### ארגומנטים וערכי החזרה שניתנים ל-serialization {/*serializable-parameters-and-return-values*/}

מכיוון שקוד לקוח קורא ל-Server Action דרך הרשת, כל הארגומנטים שמועברים חייבים להיות ניתנים ל-serialization.

אלה הסוגים הנתמכים לארגומנטים של Server Action:

* פרימיטיביים
	* [string](https://developer.mozilla.org/en-US/docs/Glossary/String)
	* [number](https://developer.mozilla.org/en-US/docs/Glossary/Number)
	* [bigint](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/BigInt)
	* [boolean](https://developer.mozilla.org/en-US/docs/Glossary/Boolean)
	* [undefined](https://developer.mozilla.org/en-US/docs/Glossary/Undefined)
	* [null](https://developer.mozilla.org/en-US/docs/Glossary/Null)
	* [symbol](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol), רק symbols שנרשמו ב-global Symbol registry דרך [`Symbol.for`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Symbol/for)
* איטרטורים שמכילים ערכים שניתנים ל-serialization
	* [String](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String)
	* [Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array)
	* [Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)
	* [Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)
	* [TypedArray](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray) ו-[ArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer)
* [Date](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Date)
* מופעי [FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData)
* [objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object) פשוטים: כאלה שנוצרו עם [object initializers](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer), עם מאפיינים שניתנים ל-serialization
* פונקציות שהן Server Actions
* [Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)

לעומת זאת, אלה אינם נתמכים:
* React elements או [JSX](/learn/writing-markup-with-jsx)
* פונקציות, כולל פונקציות קומפוננטה או כל פונקציה אחרת שאינה Server Action
* [Classes](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Objects/Classes_in_JavaScript)
* אובייקטים שהם מופעים של כל class (מלבד ה-built-ins שהוזכרו) או אובייקטים עם [null prototype](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object#null-prototype_objects)
* Symbols שלא נרשמו גלובלית, לדוגמה: `Symbol('my new symbol')`


ערכי חזרה נתמכים ב-serialization זהים ל-[serializable props](/reference/react/use-client#passing-props-from-server-to-client-components) עבור Client Component בגבול.


## שימוש {/*usage*/}

### Server Actions בתוך טפסים {/*server-actions-in-forms*/}

מקרה השימוש הנפוץ ביותר ל-Server Actions הוא קריאה לפונקציות שרת שמבצעות מוטציה בנתונים. בדפדפן, אלמנט [HTML form](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/form) הוא הדרך המסורתית לשלוח מוטציה מצד המשתמש. עם React Server Components, React מוסיף תמיכה מדרגה ראשונה ב-Server Actions בתוך [forms](/reference/react-dom/components/form).

הנה טופס שמאפשר למשתמש לבקש שם משתמש.

```js [[1, 3, "formData"]]
// App.js

async function requestUsername(formData) {
  'use server';
  const username = formData.get('username');
  // ...
}

export default function App() {
  return (
    <form action={requestUsername}>
      <input type="text" name="username" />
      <button type="submit">Request</button>
    </form>
  );
}
```

בדוגמה הזו `requestUsername` הוא Server Action שמועבר ל-`<form>`. כשמשתמש שולח את הטופס, מתבצעת בקשת רשת לפונקציית השרת `requestUsername`. כשקוראים ל-Server Action מתוך טופס, React יעביר את <CodeStep step={1}>[FormData](https://developer.mozilla.org/en-US/docs/Web/API/FormData)</CodeStep> של הטופס כארגומנט ראשון ל-Server Action.

על ידי העברת Server Action ל-`action` של הטופס, React יכול [לבצע progressive enhancement](https://developer.mozilla.org/en-US/docs/Glossary/Progressive_Enhancement) לטופס. המשמעות היא שאפשר לשלוח טפסים גם לפני שחבילת JavaScript נטענה.

#### טיפול בערכי חזרה בטפסים {/*handling-return-values*/}

בטופס בקשת שם משתמש ייתכן שהשם לא זמין. `requestUsername` צריך להחזיר לנו אם הפעולה הצליחה או נכשלה.

כדי לעדכן את ה-UI לפי תוצאת Server Action תוך תמיכה ב-progressive enhancement, השתמשו ב-[`useFormState`](/reference/react-dom/hooks/useFormState).

```js
// requestUsername.js
'use server';

export default async function requestUsername(formData) {
  const username = formData.get('username');
  if (canRequest(username)) {
    // ...
    return 'successful';
  }
  return 'failed';
}
```

```js {4,8}, [[2, 2, "'use client'"]]
// UsernameForm.js
'use client';

import { useFormState } from 'react-dom';
import requestUsername from './requestUsername';

function UsernameForm() {
  const [returnValue, action] = useFormState(requestUsername, 'n/a');

  return (
    <>
      <form action={action}>
        <input type="text" name="username" />
        <button type="submit">Request</button>
      </form>
      <p>Last submission request returned: {returnValue}</p>
    </>
  );
}
```

שימו לב שכמו רוב ה-Hooks, `useFormState` יכול להיקרא רק מתוך <CodeStep step={1}>[קוד לקוח](/reference/react/use-client)</CodeStep>.

### קריאה ל-Server Action מחוץ ל-`<form>` {/*calling-a-server-action-outside-of-form*/}

Server Actions הם endpoints בשרת, ואפשר לקרוא להם מכל מקום בקוד לקוח.

כשמשתמשים ב-Server Action מחוץ ל-[form](/reference/react-dom/components/form), קראו לו בתוך [transition](/reference/react/useTransition), כדי שתוכלו להציג אינדיקציית טעינה, להראות [optimistic state updates](/reference/react/useOptimistic), ולטפל בשגיאות לא צפויות. טפסים עוטפים Server Actions אוטומטית בתוך transitions.

```js {9-12}
import incrementLike from './actions';
import { useState, useTransition } from 'react';

function LikeButton() {
  const [isPending, startTransition] = useTransition();
  const [likeCount, setLikeCount] = useState(0);

  const onClick = () => {
    startTransition(async () => {
      const currentCount = await incrementLike();
      setLikeCount(currentCount);
    });
  };

  return (
    <>
      <p>Total Likes: {likeCount}</p>
      <button onClick={onClick} disabled={isPending}>Like</button>;
    </>
  );
}
```

```js
// actions.js
'use server';

let likeCount = 0;
export default async function incrementLike() {
  likeCount++;
  return likeCount;
}
```

כדי לקרוא את ערך החזרה של Server Action צריך לבצע `await` ל-promise שמוחזר.
