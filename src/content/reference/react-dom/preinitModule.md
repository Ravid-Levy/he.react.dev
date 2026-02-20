---
title: preinitModule
canary: true
---

<Canary>

הפונקציה `preinitModule` זמינה כרגע רק בערוצי Canary ו-experimental של React. מידע נוסף ב-[ערוצי השחרור של React](/community/versioning-policy#all-release-channels).

</Canary>

<Note>

[Frameworks מבוססי React](/learn/start-a-new-react-project) מטפלים לעיתים קרובות בטעינת משאבים בשבילכם, אז ייתכן שלא תצטרכו לקרוא ל-API הזה בעצמכם. לפרטים, עיינו בתיעוד של ה-framework שלכם.

</Note>

<Intro>

`preinitModule` מאפשרת להביא מראש מודול ESM ולהעריך אותו.

```js
preinitModule("https://example.com/module.js", {as: "script"});
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `preinitModule(href, options)` {/*preinitmodule*/}

כדי לבצע preinit למודול ESM, קראו לפונקציה `preinitModule` מתוך `react-dom`.

```js
import { preinitModule } from 'react-dom';

function AppRoot() {
  preinitModule("https://example.com/module.js", {as: "script"});
  // ...
}

```

[ראו דוגמאות נוספות בהמשך.](#usage)

הפונקציה `preinitModule` מספקת לדפדפן רמז שכדאי להתחיל להוריד ולהריץ את המודול הנתון, מה שיכול לחסוך זמן. מודולים שמבצעים להם `preinit` יורצו כשיסיימו לרדת.

#### Parameters {/*parameters*/}

* `href`: מחרוזת. ה-URL של המודול שברצונכם להוריד ולהריץ.
* `options`: אובייקט. כולל את המאפיינים הבאים:
  *  `as`: מחרוזת חובה. חייב להיות `'script'`.
  *  `crossOrigin`: מחרוזת. [מדיניות CORS](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/crossorigin) לשימוש. הערכים האפשריים: `anonymous` ו-`use-credentials`.
  *  `integrity`: מחרוזת. hash קריפטוגרפי של המודול לצורך [אימות אותנטיות](https://developer.mozilla.org/en-US/docs/Web/Security/Subresource_Integrity).
  *  `nonce`: מחרוזת. [nonce](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/nonce) קריפטוגרפי שמאפשר את המודול כשמשתמשים ב-Content Security Policy קשוחה.

#### Returns {/*returns*/}

`preinitModule` לא מחזירה דבר.

#### Caveats {/*caveats*/}

* כמה קריאות ל-`preinitModule` עם אותו `href` משפיעות כמו קריאה אחת.
* בדפדפן אפשר לקרוא ל-`preinitModule` בכל מצב: בזמן רינדור קומפוננטה, בתוך effect, בתוך event handler, וכן הלאה.
* ברינדור צד שרת או ברינדור Server Components, ל-`preinitModule` יש השפעה רק אם קוראים לה בזמן רינדור קומפוננטה או בהקשר async שמקורו ברינדור קומפוננטה. קריאות אחרות ייחסמו.

---

## שימוש {/*usage*/}

### Preloading בזמן רינדור {/*preloading-when-rendering*/}

קראו ל-`preinitModule` בזמן רינדור קומפוננטה אם אתם יודעים שהיא או הילדים שלה ישתמשו במודול ספציפי, ואם מקובל עליכם שהמודול יוערך וייכנס לפעולה מיד כשהורדתו מסתיימת.

```js
import { preinitModule } from 'react-dom';

function AppRoot() {
  preinitModule("https://example.com/module.js", {as: "script"});
  return ...;
}
```

אם אתם רוצים שהדפדפן יוריד את המודול אבל לא יריץ אותו מייד, השתמשו ב-[`preloadModule`](/reference/react-dom/preloadModule) במקום. אם אתם רוצים לבצע preinit לסקריפט שאינו מודול ESM, השתמשו ב-[`preinit`](/reference/react-dom/preinit).

### Preloading בתוך event handler {/*preloading-in-an-event-handler*/}

קראו ל-`preinitModule` בתוך event handler לפני מעבר לעמוד או מצב שבהם המודול יידרש. כך התהליך מתחיל מוקדם יותר לעומת קריאה בזמן רינדור העמוד או המצב החדש.

```js
import { preinitModule } from 'react-dom';

function CallToAction() {
  const onClick = () => {
    preinitModule("https://example.com/module.js", {as: "script"});
    startWizard();
  }
  return (
    <button onClick={onClick}>Start Wizard</button>
  );
}
```
