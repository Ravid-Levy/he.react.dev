---
title: preconnect
canary: true
---

<Canary>

הפונקציה `preconnect` זמינה כרגע רק בערוצי Canary ו-experimental של React. מידע נוסף ב-[ערוצי השחרור של React](/community/versioning-policy#all-release-channels).

</Canary>

<Intro>

`preconnect` מאפשרת להתחבר מראש לשרת שאתם מצפים לטעון ממנו משאבים.

```js
preconnect("https://example.com");
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `preconnect(href)` {/*preconnect*/}

כדי לבצע preconnect ל-host, קראו לפונקציה `preconnect` מתוך `react-dom`.

```js
import { preconnect } from 'react-dom';

function AppRoot() {
  preconnect("https://example.com");
  // ...
}

```

[ראו דוגמאות נוספות בהמשך.](#usage)

הפונקציה `preconnect` מספקת לדפדפן רמז שכדאי לפתוח חיבור לשרת הנתון. אם הדפדפן בוחר לעשות זאת, זה יכול להאיץ טעינה של משאבים מהשרת הזה.

#### Parameters {/*parameters*/}

* `href`: מחרוזת. ה-URL של השרת שאליו רוצים להתחבר.


#### Returns {/*returns*/}

`preconnect` לא מחזירה דבר.

#### Caveats {/*caveats*/}

* כמה קריאות ל-`preconnect` עם אותו שרת משפיעות כמו קריאה אחת.
* בדפדפן אפשר לקרוא ל-`preconnect` בכל מצב: בזמן רינדור קומפוננטה, בתוך effect, בתוך event handler, וכן הלאה.
* ברינדור צד שרת או ברינדור Server Components, ל-`preconnect` יש השפעה רק אם קוראים לה בזמן רינדור קומפוננטה או בהקשר async שמקורו ברינדור קומפוננטה. קריאות אחרות ייחסמו.
* אם אתם יודעים אילו משאבים ספציפיים תצטרכו, אפשר לקרוא [לפונקציות אחרות](/reference/react-dom/#resource-preloading-apis) שמתחילות לטעון את המשאבים מיד.
* אין תועלת ב-preconnect לאותו שרת שעליו מתארח דף הווב עצמו, כי החיבור אליו כבר פתוח עד לרגע שבו היה ניתן הרמז.

---

## שימוש {/*usage*/}

### Preconnect בזמן רינדור {/*preconnecting-when-rendering*/}

קראו ל-`preconnect` בזמן רינדור קומפוננטה אם אתם יודעים שהילדים שלה יטענו משאבים חיצוניים מאותו host.

```js
import { preconnect } from 'react-dom';

function AppRoot() {
  preconnect("https://example.com");
  return ...;
}
```

### Preconnect בתוך event handler {/*preconnecting-in-an-event-handler*/}

קראו ל-`preconnect` בתוך event handler לפני מעבר לעמוד או מצב שבהם יידרשו משאבים חיצוניים. כך התהליך מתחיל מוקדם יותר לעומת קריאה בזמן רינדור העמוד או המצב החדש.

```js
import { preconnect } from 'react-dom';

function CallToAction() {
  const onClick = () => {
    preconnect('http://example.com');
    startWizard();
  }
  return (
    <button onClick={onClick}>Start Wizard</button>
  );
}
```
