---
title: preloadModule
canary: true
---

<Canary>

הפונקציה `preloadModule` זמינה כרגע רק בערוצי Canary ו-experimental של React. מידע נוסף ב-[ערוצי השחרור של React](/community/versioning-policy#all-release-channels).

</Canary>

<Note>

[Frameworks מבוססי React](/learn/start-a-new-react-project) מטפלים לעיתים קרובות בטעינת משאבים בשבילכם, אז ייתכן שלא תצטרכו לקרוא ל-API הזה בעצמכם. לפרטים, עיינו בתיעוד של ה-framework שלכם.

</Note>

<Intro>

`preloadModule` מאפשרת להביא מראש מודול ESM שאתם מצפים להשתמש בו.

```js
preloadModule("https://example.com/module.js", {as: "script"});
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `preloadModule(href, options)` {/*preloadmodule*/}

כדי לבצע preload למודול ESM, קראו לפונקציה `preloadModule` מתוך `react-dom`.

```js
import { preloadModule } from 'react-dom';

function AppRoot() {
  preloadModule("https://example.com/module.js", {as: "script"});
  // ...
}

```

[ראו דוגמאות נוספות בהמשך.](#usage)

הפונקציה `preloadModule` מספקת לדפדפן רמז שכדאי להתחיל להוריד את המודול הנתון, מה שיכול לחסוך זמן.

#### Parameters {/*parameters*/}

* `href`: מחרוזת. ה-URL של המודול שברצונכם להוריד.
* `options`: אובייקט. כולל את המאפיינים הבאים:
  *  `as`: מחרוזת חובה. חייב להיות `'script'`.
  *  `crossOrigin`: מחרוזת. [מדיניות CORS](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/crossorigin) לשימוש. הערכים האפשריים: `anonymous` ו-`use-credentials`.
  *  `integrity`: מחרוזת. hash קריפטוגרפי של המודול לצורך [אימות אותנטיות](https://developer.mozilla.org/en-US/docs/Web/Security/Subresource_Integrity).
  *  `nonce`: מחרוזת. [nonce](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/nonce) קריפטוגרפי שמאפשר את המודול כשמשתמשים ב-Content Security Policy קשוחה.


#### Returns {/*returns*/}

`preloadModule` לא מחזירה דבר.

#### Caveats {/*caveats*/}

* כמה קריאות ל-`preloadModule` עם אותו `href` משפיעות כמו קריאה אחת.
* בדפדפן אפשר לקרוא ל-`preloadModule` בכל מצב: בזמן רינדור קומפוננטה, בתוך effect, בתוך event handler, וכן הלאה.
* ברינדור צד שרת או ברינדור Server Components, ל-`preloadModule` יש השפעה רק אם קוראים לה בזמן רינדור קומפוננטה או בהקשר async שמקורו ברינדור קומפוננטה. קריאות אחרות ייחסמו.

---

## שימוש {/*usage*/}

### Preloading בזמן רינדור {/*preloading-when-rendering*/}

קראו ל-`preloadModule` בזמן רינדור קומפוננטה אם אתם יודעים שהיא או הילדים שלה ישתמשו במודול ספציפי.

```js
import { preloadModule } from 'react-dom';

function AppRoot() {
  preloadModule("https://example.com/module.js", {as: "script"});
  return ...;
}
```

אם אתם רוצים שהדפדפן יתחיל גם להריץ את המודול מיד (ולא רק להוריד אותו), השתמשו ב-[`preinitModule`](/reference/react-dom/preinitModule) במקום. אם אתם רוצים לטעון סקריפט שאינו מודול ESM, השתמשו ב-[`preload`](/reference/react-dom/preload).

### Preloading בתוך event handler {/*preloading-in-an-event-handler*/}

קראו ל-`preloadModule` בתוך event handler לפני מעבר לעמוד או מצב שבהם המודול יידרש. כך התהליך מתחיל מוקדם יותר לעומת קריאה בזמן רינדור העמוד או המצב החדש.

```js
import { preloadModule } from 'react-dom';

function CallToAction() {
  const onClick = () => {
    preloadModule("https://example.com/module.js", {as: "script"});
    startWizard();
  }
  return (
    <button onClick={onClick}>Start Wizard</button>
  );
}
```
