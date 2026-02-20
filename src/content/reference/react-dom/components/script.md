---
script: "<script>"
canary: true
---

<Canary>

ההרחבות של React ל-`<script>` זמינות כרגע רק בערוצי canary ו-experimental של React. בגרסאות יציבות של React, `<script>` פועל רק כ-[רכיב HTML מובנה של הדפדפן](https://react.dev/reference/react-dom/components#all-html-components). מידע נוסף ב-[ערוצי השחרור של React](/community/versioning-policy#all-release-channels).

</Canary>

<Intro>

רכיב הדפדפן המובנה [`<script>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script) מאפשר להוסיף סקריפט למסמך.

```js
<script> alert("hi!") </script>
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `<script>` {/*script*/}

כדי להוסיף סקריפטים inline או חיצוניים למסמך, רנדרו את רכיב הדפדפן המובנה [`<script>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script). אפשר לרנדר `<script>` מכל קומפוננטה, ו-React [במקרים מסוימים](#special-rendering-behavior) תמקם את אלמנט ה-DOM המתאים ב-document head ותמנע כפילות של סקריפטים זהים.

```js
<script> alert("hi!") </script>
<script src="script.js" />
```

[ראו דוגמאות נוספות בהמשך.](#usage)

#### Props {/*props*/}

`<script>` תומך בכל [מאפייני האלמנט הנפוצים.](/reference/react-dom/components/common#props)

הוא צריך לכלול *או* `children` *או* prop בשם `src`.

* `children`: מחרוזת. קוד המקור של סקריפט inline.
* `src`: מחרוזת. ה-URL של סקריפט חיצוני.

Props נתמכים נוספים:

* `async`: ערך בוליאני. מאפשר לדפדפן לדחות את הרצת הסקריפט עד ששאר המסמך עובד — זו ההתנהגות המועדפת לביצועים.
*  `crossOrigin`: מחרוזת. [מדיניות CORS](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/crossorigin) לשימוש. הערכים האפשריים: `anonymous` ו-`use-credentials`.
* `fetchPriority`: מחרוזת. מאפשר לדפדפן לדרג עדיפות לסקריפטים בזמן טעינה של כמה סקריפטים יחד. יכול להיות `"high"`, `"low"`, או `"auto"` (ברירת מחדל).
* `integrity`: מחרוזת. hash קריפטוגרפי של הסקריפט לצורך [אימות אותנטיות](https://developer.mozilla.org/en-US/docs/Web/Security/Subresource_Integrity).
* `noModule`: ערך בוליאני. מבטל את הסקריפט בדפדפנים שתומכים ב-ES modules — ומאפשר סקריפט חלופי לדפדפנים שלא.
* `nonce`: מחרוזת. [nonce](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/nonce) קריפטוגרפי שמאפשר את המשאב כשמשתמשים ב-Content Security Policy קשוחה.
* `referrer`: מחרוזת. מציינת [איזו כותרת Referer לשלוח](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script#referrerpolicy) בעת טעינת הסקריפט וכל משאב שהסקריפט טוען לאחר מכן.
* `type`: מחרוזת. מציינת אם הסקריפט הוא [classic script, ES module, או import map](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script/type).

Props שמבטלים את [הטיפול המיוחד של React בסקריפטים](#special-rendering-behavior):

* `onError`: פונקציה. נקראת כשהסקריפט נכשל בטעינה.
* `onLoad`: פונקציה. נקראת כשהסקריפט סיים להיטען.

Props ש-**לא מומלץ** להשתמש בהם עם React:

* `blocking`: מחרוזת. אם מוגדר ל-`"render"`, מורה לדפדפן לא לרנדר את העמוד עד שה-scriptsheet נטען. React מספקת שליטה מדויקת יותר דרך Suspense.
* `defer`: מחרוזת. מונע מהדפדפן להריץ את הסקריפט עד שהמסמך סיים להיטען. לא תואם ל-streaming של קומפוננטות מרונדרות שרת. השתמשו ב-prop `async` במקום.

#### התנהגות רינדור מיוחדת {/*special-rendering-behavior*/}

React יכולה להזיז רכיבי `<script>` ל-`<head>` של המסמך, למנוע כפילות של סקריפטים זהים, ולבצע [suspend](/reference/react/Suspense) בזמן שהסקריפט נטען.

כדי להפעיל את ההתנהגות הזו, ספקו את ה-props `src` ו-`async={true}`. React תמנע כפילות של סקריפטים אם יש להם אותו `src`. ‏`async` חייב להיות true כדי שיהיה בטוח להזיז סקריפטים.

אם מספקים אחד מה-props `onLoad` או `onError`, אין התנהגות מיוחדת, כי props אלה מציינים שאתם מנהלים ידנית את טעינת הסקריפט בתוך הקומפוננטה.

לטיפול המיוחד הזה יש שתי הסתייגויות:

* React תתעלם משינויים ב-props אחרי שהסקריפט רונדר. (React תציג אזהרה בזמן פיתוח אם זה קורה.)
* React עשויה להשאיר את הסקריפט ב-DOM גם אחרי שהקומפוננטה שרנדרה אותו הוסרה. (אין לזה השפעה כי סקריפטים מורצים רק פעם אחת כשהם מוכנסים ל-DOM.)

---

## שימוש {/*usage*/}

### רינדור סקריפט חיצוני {/*rendering-an-external-script*/}

אם קומפוננטה תלויה בסקריפטים מסוימים כדי להיות מוצגת נכון, אפשר לרנדר `<script>` בתוך הקומפוננטה.

אם תספקו props של `src` ו-`async`, הקומפוננטה שלכם תבצע suspend בזמן שהסקריפט נטען. React תמנע כפילות של סקריפטים עם אותו `src`, ותכניס רק אחד מהם ל-DOM גם אם כמה קומפוננטות מרנדרות אותו.

<SandpackWithHTMLOutput>

```js src/App.js active
import ShowRenderedHTML from './ShowRenderedHTML.js';

function Map({lat, long}) {
  return (
    <>
      <script async src="map-api.js" />
      <div id="map" data-lat={lat} data-long={long} />
    </>
  );
}

export default function Page() {
  return (
    <ShowRenderedHTML>
      <Map />
    </ShowRenderedHTML>
  );
}
```

</SandpackWithHTMLOutput>

<Note>
כשאתם רוצים להשתמש בסקריפט, יכול להיות מועיל לקרוא לפונקציה [preinit](/reference/react-dom/preinit). קריאה לפונקציה הזו עשויה לאפשר לדפדפן להתחיל להביא את הסקריפט מוקדם יותר מאשר ברינדור רגיל של `<script>`, למשל באמצעות [HTTP Early Hints response](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/103).
</Note>

### רינדור סקריפט inline {/*rendering-an-inline-script*/}

כדי לכלול סקריפט inline, רנדרו את רכיב `<script>` עם קוד המקור של הסקריפט כ-children שלו. סקריפטים inline אינם עוברים de-duplication ואינם מועברים ל-`<head>` של המסמך, ומכיוון שהם לא טוענים משאבים חיצוניים הם לא יגרמו לקומפוננטה לבצע suspend.

<SandpackWithHTMLOutput>

```js src/App.js active
import ShowRenderedHTML from './ShowRenderedHTML.js';

function Tracking() {
  return (
    <script>
      ga('send', 'pageview');
    </script>
  );
}

export default function Page() {
  return (
    <ShowRenderedHTML>
      <h1>My Website</h1>
      <Tracking />
      <p>Welcome</p>
    </ShowRenderedHTML>
  );
}
```

</SandpackWithHTMLOutput>
