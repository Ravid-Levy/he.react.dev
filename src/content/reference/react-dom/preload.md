---
title: preload
canary: true
---

<Canary>

הפונקציה `preload` זמינה כרגע רק בערוצי Canary ו-experimental של React. מידע נוסף ב-[ערוצי השחרור של React](/community/versioning-policy#all-release-channels).

</Canary>

<Note>

[Frameworks מבוססי React](/learn/start-a-new-react-project) מטפלים לעיתים קרובות בטעינת משאבים בשבילכם, אז ייתכן שלא תצטרכו לקרוא ל-API הזה בעצמכם. לפרטים, עיינו בתיעוד של ה-framework שלכם.

</Note>

<Intro>

`preload` מאפשרת להביא מראש משאב כמו stylesheet, font, או סקריפט חיצוני שאתם מצפים להשתמש בו.

```js
preload("https://example.com/font.woff2", {as: "font"});
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `preload(href, options)` {/*preload*/}

כדי לבצע preload למשאב, קראו לפונקציה `preload` מתוך `react-dom`.

```js
import { preload } from 'react-dom';

function AppRoot() {
  preload("https://example.com/font.woff2", {as: "font"});
  // ...
}

```

[ראו דוגמאות נוספות בהמשך.](#usage)

הפונקציה `preload` מספקת לדפדפן רמז שכדאי להתחיל להוריד את המשאב הנתון, מה שיכול לחסוך זמן.

#### Parameters {/*parameters*/}

* `href`: מחרוזת. ה-URL של המשאב שברצונכם להוריד.
* `options`: אובייקט. כולל את המאפיינים הבאים:
  *  `as`: מחרוזת חובה. סוג המשאב. [הערכים האפשריים](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link#as): `audio`, `document`, `embed`, `fetch`, `font`, `image`, `object`, `script`, `style`, `track`, `video`, `worker`.
  *  `crossOrigin`: מחרוזת. [מדיניות CORS](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/crossorigin) לשימוש. הערכים האפשריים: `anonymous` ו-`use-credentials`. חובה כשהערך של `as` הוא `"fetch"`.
  *  `referrerPolicy`: מחרוזת. [כותרת Referrer](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link#referrerpolicy) שתישלח בזמן הטעינה. הערכים האפשריים: `no-referrer-when-downgrade` (ברירת מחדל), `no-referrer`, `origin`, `origin-when-cross-origin`, ו-`unsafe-url`.
  *  `integrity`: מחרוזת. hash קריפטוגרפי של המשאב לצורך [אימות אותנטיות](https://developer.mozilla.org/en-US/docs/Web/Security/Subresource_Integrity).
  *  `type`: מחרוזת. סוג ה-MIME של המשאב.
  *  `nonce`: מחרוזת. [nonce](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/nonce) קריפטוגרפי שמאפשר את המשאב כשמשתמשים ב-Content Security Policy קשוחה.
  *  `fetchPriority`: מחרוזת. מציעה עדיפות יחסית לטעינת המשאב. הערכים האפשריים: `auto` (ברירת מחדל), `high`, ו-`low`.
  *  `imageSrcSet`: מחרוזת. לשימוש רק עם `as: "image"`. מציינת את [source set של התמונה](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images).
  *  `imageSizes`: מחרוזת. לשימוש רק עם `as: "image"`. מציינת את [הגדלים של התמונה](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images).

#### Returns {/*returns*/}

`preload` לא מחזירה דבר.

#### Caveats {/*caveats*/}

* כמה קריאות שקולות ל-`preload` משפיעות כמו קריאה אחת. קריאות ל-`preload` נחשבות שקולות לפי הכללים האלה:
  * שתי קריאות שקולות אם יש להן אותו `href`, חוץ מהמקרה הבא:
  * אם `as` מוגדר כ-`image`, שתי קריאות שקולות אם יש להן אותו `href`, `imageSrcSet`, ו-`imageSizes`.
* בדפדפן אפשר לקרוא ל-`preload` בכל מצב: בזמן רינדור קומפוננטה, בתוך effect, בתוך event handler, וכן הלאה.
* ברינדור צד שרת או ברינדור Server Components, ל-`preload` יש השפעה רק אם קוראים לה בזמן רינדור קומפוננטה או בהקשר async שמקורו ברינדור קומפוננטה. קריאות אחרות ייחסמו.

---

## שימוש {/*usage*/}

### Preloading בזמן רינדור {/*preloading-when-rendering*/}

קראו ל-`preload` בזמן רינדור קומפוננטה אם אתם יודעים שהיא או הילדים שלה ישתמשו במשאב ספציפי.

<Recipes titleText="Examples of preloading">

#### Preloading לסקריפט חיצוני {/*preloading-an-external-script*/}

```js
import { preload } from 'react-dom';

function AppRoot() {
  preload("https://example.com/script.js", {as: "script"});
  return ...;
}
```

אם אתם רוצים שהדפדפן יתחיל להריץ את הסקריפט מיד (ולא רק להוריד אותו), השתמשו ב-[`preinit`](/reference/react-dom/preinit) במקום. אם רוצים לטעון מודול ESM, השתמשו ב-[`preloadModule`](/reference/react-dom/preloadModule).

<Solution />

#### Preloading ל-stylesheet {/*preloading-a-stylesheet*/}

```js
import { preload } from 'react-dom';

function AppRoot() {
  preload("https://example.com/style.css", {as: "style"});
  return ...;
}
```

אם אתם רוצים שה-stylesheet יוכנס למסמך מיד (כלומר שהדפדפן יתחיל לפרש אותו מייד ולא רק להוריד אותו), השתמשו ב-[`preinit`](/reference/react-dom/preinit) במקום.

<Solution />

#### Preloading לפונט {/*preloading-a-font*/}

```js
import { preload } from 'react-dom';

function AppRoot() {
  preload("https://example.com/style.css", {as: "style"});
  preload("https://example.com/font.woff2", {as: "font"});
  return ...;
}
```

אם אתם מבצעים preload ל-stylesheet, חכם לבצע preload גם לפונטים שה-stylesheet מפנה אליהם. כך הדפדפן יכול להתחיל להוריד את הפונט עוד לפני שה-stylesheet יורד ומפורש.

<Solution />

#### Preloading לתמונה {/*preloading-an-image*/}

```js
import { preload } from 'react-dom';

function AppRoot() {
  preload("/banner.png", {
    as: "image",
    imageSrcSet: "/banner512.png 512w, /banner1024.png 1024w",
    imageSizes: "(max-width: 512px) 512px, 1024px",
  });
  return ...;
}
```

כשמבצעים preload לתמונה, האפשרויות `imageSrcSet` ו-`imageSizes` עוזרות לדפדפן [להביא את התמונה בגודל הנכון לגודל המסך](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images).

<Solution />

</Recipes>

### Preloading בתוך event handler {/*preloading-in-an-event-handler*/}

קראו ל-`preload` בתוך event handler לפני מעבר לעמוד או מצב שבהם יידרשו משאבים חיצוניים. כך התהליך מתחיל מוקדם יותר לעומת קריאה בזמן רינדור העמוד או המצב החדש.

```js
import { preload } from 'react-dom';

function CallToAction() {
  const onClick = () => {
    preload("https://example.com/wizardStyles.css", {as: "style"});
    startWizard();
  }
  return (
    <button onClick={onClick}>Start Wizard</button>
  );
}
```
