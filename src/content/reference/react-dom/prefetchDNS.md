---
title: prefetchDNS
canary: true
---

<Canary>

הפונקציה `prefetchDNS` זמינה כרגע רק בערוצי Canary ו-experimental של React. מידע נוסף ב-[ערוצי השחרור של React](/community/versioning-policy#all-release-channels).

</Canary>

<Intro>

`prefetchDNS` מאפשרת לבצע חיפוש מוקדם של כתובת ה-IP של שרת שאתם מצפים לטעון ממנו משאבים.

```js
prefetchDNS("https://example.com");
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `prefetchDNS(href)` {/*prefetchdns*/}

כדי לבצע lookup ל-host, קראו לפונקציה `prefetchDNS` מתוך `react-dom`.

```js
import { prefetchDNS } from 'react-dom';

function AppRoot() {
  prefetchDNS("https://example.com");
  // ...
}

```

[ראו דוגמאות נוספות בהמשך.](#usage)

הפונקציה `prefetchDNS` מספקת לדפדפן רמז שכדאי לו לבצע lookup לכתובת ה-IP של שרת נתון. אם הדפדפן בוחר לעשות זאת, זה יכול להאיץ טעינה של משאבים מהשרת הזה.

#### Parameters {/*parameters*/}

* `href`: מחרוזת. ה-URL של השרת שאליו רוצים להתחבר.

#### Returns {/*returns*/}

`prefetchDNS` לא מחזירה דבר.

#### Caveats {/*caveats*/}

* כמה קריאות ל-`prefetchDNS` עם אותו שרת משפיעות כמו קריאה אחת.
* בדפדפן אפשר לקרוא ל-`prefetchDNS` בכל מצב: בזמן רינדור קומפוננטה, בתוך effect, בתוך event handler, וכן הלאה.
* ברינדור צד שרת או ברינדור Server Components, ל-`prefetchDNS` יש השפעה רק אם קוראים לה בזמן רינדור קומפוננטה או בהקשר async שמקורו ברינדור קומפוננטה. קריאות אחרות ייחסמו.
* אם אתם יודעים אילו משאבים ספציפיים תצטרכו, אפשר לקרוא [לפונקציות אחרות](/reference/react-dom/#resource-preloading-apis) שמתחילות לטעון את המשאבים מיד.
* אין תועלת ב-prefetch לאותו שרת שעליו מתארח דף הווב עצמו, כי lookup אליו כבר בוצע עד לזמן שבו היה ניתן הרמז.
* בהשוואה ל-[`preconnect`](/reference/react-dom/preconnect), ייתכן ש-`prefetchDNS` עדיף אם אתם מתחברים ספקולטיבית למספר גדול של דומיינים, מצב שבו העלות של preconnections עלולה לעלות על התועלת.

---

## שימוש {/*usage*/}

### DNS Prefetch בזמן רינדור {/*prefetching-dns-when-rendering*/}

קראו ל-`prefetchDNS` בזמן רינדור קומפוננטה אם אתם יודעים שהילדים שלה יטענו משאבים חיצוניים מאותו host.

```js
import { prefetchDNS } from 'react-dom';

function AppRoot() {
  prefetchDNS("https://example.com");
  return ...;
}
```

### DNS Prefetch בתוך event handler {/*prefetching-dns-in-an-event-handler*/}

קראו ל-`prefetchDNS` בתוך event handler לפני מעבר לעמוד או מצב שבהם יידרשו משאבים חיצוניים. כך התהליך מתחיל מוקדם יותר לעומת קריאה בזמן רינדור העמוד או המצב החדש.

```js
import { prefetchDNS } from 'react-dom';

function CallToAction() {
  const onClick = () => {
    prefetchDNS('http://example.com');
    startWizard();
  }
  return (
    <button onClick={onClick}>Start Wizard</button>
  );
}
```
