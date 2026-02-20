---
meta: "<meta>"
canary: true
---

<Canary>

ההרחבות של React ל-`<meta>` זמינות כרגע רק בערוצי canary ו-experimental של React. בגרסאות יציבות של React, `<meta>` פועל רק כ-[רכיב HTML מובנה של הדפדפן](https://react.dev/reference/react-dom/components#all-html-components). מידע נוסף ב-[ערוצי השחרור של React](/community/versioning-policy#all-release-channels).

</Canary>


<Intro>

רכיב הדפדפן המובנה [`<meta>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta) מאפשר להוסיף metadata למסמך.

```js
<meta name="keywords" content="React, JavaScript, semantic markup, html" />
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `<meta>` {/*meta*/}

כדי להוסיף metadata למסמך, רנדרו את רכיב הדפדפן המובנה [`<meta>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta). אפשר לרנדר `<meta>` מכל קומפוננטה ו-React תמיד תמקם את אלמנט ה-DOM המתאים בתוך ה-document head.

```js
<meta name="keywords" content="React, JavaScript, semantic markup, html" />
```

[ראו דוגמאות נוספות בהמשך.](#usage)

#### Props {/*props*/}

`<meta>` תומך בכל [מאפייני האלמנט הנפוצים.](/reference/react-dom/components/common#props)

הוא צריך לקבל *בדיוק אחד* מה-props הבאים: `name`, `httpEquiv`, `charset`, `itemProp`. רכיב `<meta>` פועל אחרת בהתאם ל-prop שמצוין.

* `name`: מחרוזת. מציינת את [סוג ה-metadata](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta/name) שיצורף למסמך.
* `charset`: מחרוזת. מציינת את קידוד התווים של המסמך. הערך התקין היחיד הוא `"utf-8"`.
* `httpEquiv`: מחרוזת. מציינת directive לעיבוד המסמך.
* `itemProp`: מחרוזת. מציינת metadata על פריט מסוים בתוך המסמך, ולא על המסמך כולו.
* `content`: מחרוזת. מציינת את ה-metadata לצירוף כשמשתמשים ב-`name` או `itemProp`, או את התנהגות ה-directive כשמשתמשים ב-`httpEquiv`.

#### התנהגות רינדור מיוחדת {/*special-rendering-behavior*/}

React תמיד תמקם את אלמנט ה-DOM המתאים ל-`<meta>` בתוך `<head>` של המסמך, בלי קשר למקום שבו הוא מרונדר בעץ React. `<head>` הוא המקום החוקי היחיד ל-`<meta>` בתוך ה-DOM, ובכל זאת זה נוח ושומר על קומפוזביליות אם קומפוננטה שמייצגת עמוד מסוים יכולה לרנדר בעצמה רכיבי `<meta>`.

יש חריג אחד לכך: אם ל-`<meta>` יש prop מסוג [`itemProp`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/itemprop), אין התנהגות מיוחדת, כי במקרה הזה הוא לא מייצג metadata על המסמך אלא metadata על חלק מסוים של העמוד.

---

## שימוש {/*usage*/}

### הוספת metadata למסמך {/*annotating-the-document-with-metadata*/}

אפשר להוסיף למסמך metadata כמו מילות מפתח, תקציר או שם המחבר. React תמקם את ה-metadata הזו בתוך `<head>` של המסמך, בלי קשר למקום שבו הוא מרונדר בעץ React.

```html
<meta name="author" content="John Smith" />
<meta name="keywords" content="React, JavaScript, semantic markup, html" />
<meta name="description" content="API reference for the <meta> component in React DOM" />
```

אפשר לרנדר את רכיב `<meta>` מכל קומפוננטה. React תשים DOM node של `<meta>` בתוך `<head>` של המסמך.

<SandpackWithHTMLOutput>

```js src/App.js active
import ShowRenderedHTML from './ShowRenderedHTML.js';

export default function SiteMapPage() {
  return (
    <ShowRenderedHTML>
      <meta name="keywords" content="React" />
      <meta name="description" content="A site map for the React website" />
      <h1>Site Map</h1>
      <p>...</p>
    </ShowRenderedHTML>
  );
}
```

</SandpackWithHTMLOutput>

### הוספת metadata לפריטים ספציפיים במסמך {/*annotating-specific-items-within-the-document-with-metadata*/}

אפשר להשתמש ברכיב `<meta>` עם prop מסוג `itemProp` כדי להוסיף metadata לפריטים ספציפיים בתוך המסמך. במקרה כזה, React *לא* תמקם את ההערות האלה בתוך `<head>` של המסמך, אלא תמקם אותן כמו כל קומפוננטת React אחרת.

```js
<section itemScope>
  <h3>Annotating specific items</h3>
  <meta itemProp="description" content="API reference for using <meta> with itemProp" />
  <p>...</p>
</section>
```
