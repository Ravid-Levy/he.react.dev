---
title: experimental_taintUniqueValue
---

<Wip>

**ה-API הזה ניסיוני ועדיין לא זמין בגרסה יציבה של React.**

אפשר לנסות אותו על ידי שדרוג חבילות React לגרסה הניסיונית העדכנית ביותר:

- `react@experimental`
- `react-dom@experimental`
- `eslint-plugin-react-hooks@experimental`

גרסאות ניסיוניות של React עשויות להכיל באגים. אל תשתמשו בהן ב-production.

ה-API הזה זמין רק בתוך [React Server Components](/reference/react/use-client).

</Wip>


<Intro>

`taintUniqueValue` מאפשרת למנוע העברה של ערכים ייחודיים ל-Client Components, כמו סיסמאות, מפתחות או tokens.

```js
taintUniqueValue(errMessage, lifetime, value)
```

כדי למנוע העברה של אובייקט שמכיל מידע רגיש, ראו [`taintObjectReference`](/reference/react/experimental_taintObjectReference).

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `taintUniqueValue(message, lifetime, value)` {/*taintuniquevalue*/}

קראו ל-`taintUniqueValue` עם סיסמה, token, מפתח או hash כדי לרשום אותם ב-React כמשהו שאסור להעביר ללקוח כפי שהוא:

```js
import {experimental_taintUniqueValue} from 'react';

experimental_taintUniqueValue(
  'Do not pass secret keys to the client.',
  process,
  process.env.SECRET_KEY
);
```

[ראו דוגמאות נוספות בהמשך.](#usage)

#### Parameters {/*parameters*/}

* `message`: ההודעה שתרצו להציג אם `value` מועבר ל-Client Component. הודעה זו תוצג כחלק מה-Error שיושלך אם `value` מועבר ל-Client Component.

* `lifetime`: כל אובייקט שמציין כמה זמן `value` צריך להיות מסומן כ-tainted. `value` ייחסם משליחה לכל Client Component כל עוד האובייקט הזה עדיין קיים. לדוגמה, העברת `globalThis` חוסמת את הערך לאורך כל חיי האפליקציה. לרוב `lifetime` הוא אובייקט שהמאפיינים שלו מכילים את `value`.

* `value`: מחרוזת, bigint או TypedArray. `value` חייב להיות רצף ייחודי של תווים או בתים עם אנטרופיה גבוהה, כמו token קריפטוגרפי, מפתח פרטי, hash או סיסמה ארוכה. `value` ייחסם משליחה לכל Client Component.

#### Returns {/*returns*/}

`experimental_taintUniqueValue` מחזירה `undefined`.

#### Caveats {/*caveats*/}

* גזירת ערכים חדשים מערכים tainted עלולה לפגוע בהגנת tainting. ערכים חדשים שנוצרים מהמרת אותיות לגדולות, שרשור מחרוזות tainted למחרוזת גדולה יותר, המרה ל-base64, חיתוך תת-מחרוזת מערכים tainted וטרנספורמציות דומות, אינם tainted אלא אם קוראים במפורש ל-`taintUniqueValue` גם על הערכים החדשים.
* אל תשתמשו ב-`taintUniqueValue` להגנה על ערכים בעלי אנטרופיה נמוכה כמו קודי PIN או מספרי טלפון. אם ערך כלשהו בבקשה נשלט על ידי תוקף, הוא עלול להסיק איזה ערך מסומן כ-tainted על ידי מעבר על כל הערכים האפשריים של הסוד.

---

## שימוש {/*usage*/}

### מניעת העברת token ל-Client Components {/*prevent-a-token-from-being-passed-to-client-components*/}

כדי לוודא שמידע רגיש כמו סיסמאות, session tokens או ערכים ייחודיים אחרים לא מועבר בטעות ל-Client Components, הפונקציה `taintUniqueValue` מספקת שכבת הגנה. כשערך מסומן כ-tainted, כל ניסיון להעביר אותו ל-Client Component יגרום לשגיאה.

הארגומנט `lifetime` מגדיר את משך הזמן שבו הערך נשאר tainted. לערכים שצריכים להישאר tainted ללא הגבלת זמן, אובייקטים כמו [`globalThis`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/globalThis) או `process` יכולים לשמש כ-`lifetime`. לאובייקטים האלה יש אורך חיים שחופף לכל משך הרצת האפליקציה.

```js
import {experimental_taintUniqueValue} from 'react';

experimental_taintUniqueValue(
  'Do not pass a user password to the client.',
  globalThis,
  process.env.SECRET_KEY
);
```

אם אורך החיים של הערך המסומן קשור לאובייקט מסוים, `lifetime` צריך להיות האובייקט שעוטף את הערך. כך מובטח שהערך המסומן יישאר מוגן לאורך חיי האובייקט העוטף.

```js
import {experimental_taintUniqueValue} from 'react';

export async function getUser(id) {
  const user = await db`SELECT * FROM users WHERE id = ${id}`;
  experimental_taintUniqueValue(
    'Do not pass a user session token to the client.',
    user,
    user.session.token
  );
  return user;
}
```

בדוגמה הזו, האובייקט `user` משמש כארגומנט `lifetime`. אם האובייקט הזה נשמר ב-global cache או נגיש מבקשה אחרת, ה-session token נשאר tainted.

<Pitfall>

**אל תסתמכו רק על tainting לאבטחה.** סימון ערך כ-tainted לא חוסם כל ערך נגזר אפשרי. לדוגמה, יצירת ערך חדש מהמרת מחרוזת tainted לאותיות גדולות לא תסמן את הערך החדש.


```js
import {experimental_taintUniqueValue} from 'react';

const password = 'correct horse battery staple';

experimental_taintUniqueValue(
  'Do not pass the password to the client.',
  globalThis,
  password
);

const uppercasePassword = password.toUpperCase() // `uppercasePassword` is not tainted
```

בדוגמה זו, הקבוע `password` מסומן כ-tainted. לאחר מכן משתמשים ב-`password` ליצירת ערך חדש `uppercasePassword` על ידי קריאה ל-`toUpperCase`. הערך החדש `uppercasePassword` אינו tainted.

שיטות דומות נוספות לגזירת ערכים חדשים מערכים tainted, כמו שרשור למחרוזת גדולה יותר, המרה ל-base64 או החזרת תת-מחרוזת, יוצרות ערכים לא מסומנים.

Tainting מגינה רק מפני טעויות פשוטות כמו העברה מפורשת של ערכים סודיים ללקוח. טעויות בשימוש ב-`taintUniqueValue`, כמו שימוש ב-global store מחוץ ל-React בלי אובייקט lifetime תואם, עלולות לגרום לכך שהערך המסומן יאבד את הסימון. Tainting היא שכבת הגנה אחת; אפליקציה מאובטחת תכלול כמה שכבות הגנה, APIs מתוכננים היטב ודפוסי בידוד.

</Pitfall>

<DeepDive>

#### שימוש ב-`server-only` ו-`taintUniqueValue` כדי למנוע דליפת סודות {/*using-server-only-and-taintuniquevalue-to-prevent-leaking-secrets*/}

אם אתם מריצים סביבת Server Components שיש לה גישה למפתחות פרטיים או סיסמאות, כמו סיסמת מסד נתונים, צריך להיזהר לא להעביר את זה ל-Client Component.

```js
export async function Dashboard(props) {
  // DO NOT DO THIS
  return <Overview password={process.env.API_PASSWORD} />;
}
```

```js
"use client";

import {useEffect} from '...'

export async function Overview({ password }) {
  useEffect(() => {
    const headers = { Authorization: password };
    fetch(url, { headers }).then(...);
  }, [password]);
  ...
}
```

הדוגמה הזו תדליף את סוד ה-API token ללקוח. אם ה-token הזה יכול לשמש לגישה לנתונים שלמשתמש הזה לא אמורה להיות גישה אליהם, זה עלול להוביל לדליפת מידע.

[comment]: <> (TODO: Link to `server-only` docs once they are written)

אידיאלית, סודות כאלה יופשטו לקובץ helper יחיד שאפשר לייבא רק מתוך utilities מהימנים בצד השרת. אפשר אפילו לתייג את ה-helper עם [`server-only`](https://www.npmjs.com/package/server-only) כדי להבטיח שהקובץ הזה לא מיובא ללקוח.

```js
import "server-only";

export function fetchAPI(url) {
  const headers = { Authorization: process.env.API_PASSWORD };
  return fetch(url, { headers });
}
```

לפעמים קורות טעויות במהלך refactoring ולא כל חברי הצוות מודעים לכך.
כדי להתגונן מפני טעויות כאלה בהמשך, אפשר "לסמן" את הסיסמה עצמה:

```js
import "server-only";
import {experimental_taintUniqueValue} from 'react';

experimental_taintUniqueValue(
  'Do not pass the API token password to the client. ' +
    'Instead do all fetches on the server.'
  process,
  process.env.API_PASSWORD
);
```

כעת, בכל פעם שמישהו ינסה להעביר את הסיסמה הזו ל-Client Component, או לשלוח אותה ל-Client Component דרך Server Action, תיזרק שגיאה עם ההודעה שהגדרתם בקריאה ל-`taintUniqueValue`.

</DeepDive>

---
