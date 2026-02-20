---
title: preinit
canary: true
---

<Canary>

הפונקציה `preinit` זמינה כרגע רק בערוצי Canary ו-experimental של React. מידע נוסף ב-[ערוצי השחרור של React](/community/versioning-policy#all-release-channels).

</Canary>

<Note>

[Frameworks מבוססי React](/learn/start-a-new-react-project) מטפלים לעיתים קרובות בטעינת משאבים בשבילכם, אז ייתכן שלא תצטרכו לקרוא ל-API הזה בעצמכם. לפרטים, עיינו בתיעוד של ה-framework שלכם.

</Note>

<Intro>

`preinit` מאפשרת להביא מראש ולהעריך stylesheet או סקריפט חיצוני.

```js
preinit("https://example.com/script.js", {as: "style"});
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `preinit(href, options)` {/*preinit*/}

כדי לבצע preinit לסקריפט או stylesheet, קראו לפונקציה `preinit` מתוך `react-dom`.

```js
import { preinit } from 'react-dom';

function AppRoot() {
  preinit("https://example.com/script.js", {as: "script"});
  // ...
}

```

[ראו דוגמאות נוספות בהמשך.](#usage)

הפונקציה `preinit` מספקת לדפדפן רמז שכדאי להתחיל להוריד ולהריץ את המשאב הנתון, מה שיכול לחסוך זמן. סקריפטים שמבצעים להם `preinit` יורצו כשהורדתם תסתיים. Stylesheets שמבצעים להם preinit יוכנסו למסמך וייכנסו לפעולה מיד.

#### Parameters {/*parameters*/}

* `href`: מחרוזת. ה-URL של המשאב שברצונכם להוריד ולהריץ.
* `options`: אובייקט. כולל את המאפיינים הבאים:
  *  `as`: מחרוזת חובה. סוג המשאב. הערכים האפשריים: `script` ו-`style`.
  * `precedence`: מחרוזת. חובה עבור stylesheets. מציינת איפה להכניס את ה-stylesheet ביחס לאחרים. stylesheets עם קדימות גבוהה יותר יכולים לעקוף כאלה עם קדימות נמוכה יותר. הערכים האפשריים: `reset`, `low`, `medium`, `high`.
  *  `crossOrigin`: מחרוזת. [מדיניות CORS](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/crossorigin) לשימוש. הערכים האפשריים: `anonymous` ו-`use-credentials`. חובה כשהערך של `as` הוא `"fetch"`.
  *  `integrity`: מחרוזת. hash קריפטוגרפי של המשאב לצורך [אימות אותנטיות](https://developer.mozilla.org/en-US/docs/Web/Security/Subresource_Integrity).
  *  `nonce`: מחרוזת. [nonce](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/nonce) קריפטוגרפי שמאפשר את המשאב כשמשתמשים ב-Content Security Policy קשוחה.
  *  `fetchPriority`: מחרוזת. מציעה עדיפות יחסית לטעינת המשאב. הערכים האפשריים: `auto` (ברירת מחדל), `high`, ו-`low`.

#### Returns {/*returns*/}

`preinit` לא מחזירה דבר.

#### Caveats {/*caveats*/}

* כמה קריאות ל-`preinit` עם אותו `href` משפיעות כמו קריאה אחת.
* בדפדפן אפשר לקרוא ל-`preinit` בכל מצב: בזמן רינדור קומפוננטה, בתוך effect, בתוך event handler, וכן הלאה.
* ברינדור צד שרת או ברינדור Server Components, ל-`preinit` יש השפעה רק אם קוראים לה בזמן רינדור קומפוננטה או בהקשר async שמקורו ברינדור קומפוננטה. קריאות אחרות ייחסמו.

---

## שימוש {/*usage*/}

### Preinit בזמן רינדור {/*preiniting-when-rendering*/}

קראו ל-`preinit` בזמן רינדור קומפוננטה אם אתם יודעים שהיא או הילדים שלה ישתמשו במשאב ספציפי, ואם מקובל עליכם שהמשאב יוערך וייכנס לפעולה מיד כשהורדתו מסתיימת.

<Recipes titleText="Examples of preiniting">

#### Preinit לסקריפט חיצוני {/*preiniting-an-external-script*/}

```js
import { preinit } from 'react-dom';

function AppRoot() {
  preinit("https://example.com/script.js", {as: "script"});
  return ...;
}
```

אם אתם רוצים שהדפדפן יוריד את הסקריפט אבל לא יריץ אותו מיד, השתמשו ב-[`preload`](/reference/react-dom/preload) במקום. אם רוצים לטעון מודול ESM, השתמשו ב-[`preinitModule`](/reference/react-dom/preinitModule).

<Solution />

#### Preinit ל-stylesheet {/*preiniting-a-stylesheet*/}

```js
import { preinit } from 'react-dom';

function AppRoot() {
  preinit("https://example.com/style.css", {as: "style", precedence: "medium"});
  return ...;
}
```

האפשרות `precedence`, שהיא חובה, מאפשרת לשלוט בסדר של stylesheets בתוך המסמך. stylesheets עם קדימות גבוהה יותר יכולים לעקוף כאלה עם קדימות נמוכה יותר.

אם אתם רוצים להוריד את ה-stylesheet אבל לא להכניס אותו למסמך מיד, השתמשו ב-[`preload`](/reference/react-dom/preload) במקום.

<Solution />

</Recipes>

### Preinit בתוך event handler {/*preiniting-in-an-event-handler*/}

קראו ל-`preinit` בתוך event handler לפני מעבר לעמוד או מצב שבהם יידרשו משאבים חיצוניים. כך התהליך מתחיל מוקדם יותר לעומת קריאה בזמן רינדור העמוד או המצב החדש.

```js
import { preinit } from 'react-dom';

function CallToAction() {
  const onClick = () => {
    preinit("https://example.com/wizardStyles.css", {as: "style"});
    startWizard();
  }
  return (
    <button onClick={onClick}>Start Wizard</button>
  );
}
```
