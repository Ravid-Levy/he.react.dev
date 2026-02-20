---
title: "חשיבה ב-React"
---

<Intro>

React יכול לשנות את האופן שבו אתה חושב על העיצובים שאתה מסתכל עליהם ועל האפליקציות שאתה בונה. כאשר אתה בונה ממשק משתמש עם React, תחילה תפרק אותו לחתיכות שנקראות *רכיבים*. לאחר מכן, תתאר את הstates החזותיים השונים עבור כל אחד מהרכיבים שלך. לבסוף, תחבר את הרכיבים שלך יחד כך שהנתונים יזרמו דרכם. במדריך זה, נדריך אותך בתהליך החשיבה של בניית טבלת נתוני מוצרים הניתנת לחיפוש עם React.

</Intro>

## התחל עם הדגם {/*התחל-עם-הדגם*/}

תאר לעצמך שכבר יש לך JSON API ודגם ממעצב.

ה-API של JSON מחזיר כמה נתונים שנראים כך:

```json
[
  { category: "Fruits", price: "$1", stocked: true, name: "Apple" },
  { category: "Fruits", price: "$1", stocked: true, name: "Dragonfruit" },
  { category: "Fruits", price: "$2", stocked: false, name: "Passionfruit" },
  { category: "Vegetables", price: "$2", stocked: true, name: "Spinach" },
  { category: "Vegetables", price: "$4", stocked: false, name: "Pumpkin" },
  { category: "Vegetables", price: "$1", stocked: true, name: "Peas" }
]
```

הדגם נראה כך:

<img src="/images/docs/s_thinking-in-react_ui.png" width="300" style={{margin: '0 auto'}} />

כדי ליישם ממשק משתמש ב-React, בדרך כלל תבצע את אותם חמשת שלבים.

## שלב 1: חלק את ממשק המשתמש להיררכיית רכיבים {/*step-1-break-the-ui-to-a-component-hierarchy*/}

התחל על ידי ציור תיבות סביב כל רכיב ותת רכיב בדגם ותן להם שם. אם אתה עובד עם מעצב, ייתכן שהוא כבר קרא לרכיבים האלה בכלי העיצוב שלהם. תשאל אותם!

בהתאם לרקע שלך, אתה יכול לחשוב על פיצול עיצוב לרכיבים בדרכים שונות:

* **תכנות**--השתמש באותן טכניקות כדי להחליט אם עליך ליצור פונקציה או אובייקט חדש. טכניקה אחת כזו היא [עקרון האחריות היחידה](https://en.wikipedia.org/wiki/Single_responsibility_principle), כלומר, רכיב צריך לעשות רק דבר אחד. אם בסופו של דבר הוא גדל, יש לפרק אותו לתת-רכיבים קטנים יותר. 
* **CSS**--שקול בשביל מה היית עושה בוררי כיתות. (עם זאת, הרכיבים קצת פחות גרגירים.)
* **עיצוב**--שקול כיצד היית מארגן את שכבות העיצוב.

אם ה-JSON שלך מובנה היטב, לעתים קרובות תגלה שהוא ממפה באופן טבעי למבנה הרכיבים של ממשק המשתמש שלך. הסיבה לכך היא כי למודלים של ממשק משתמש ולנתונים יש לעתים קרובות את אותה ארכיטקטורת מידע - כלומר, אותה צורה. הפרד את ממשק המשתמש שלך לרכיבים, כאשר כל רכיב תואם חלק אחד של מודל הנתונים שלך.

ישנם חמישה רכיבים במסך זה:

<FullWidth>

<CodeDiagram flip>

<img src="/images/docs/s_thinking-in-react_ui_outline.png" width="500" style={{margin: '0 auto'}} />

1. `FilterableProductTable` (אפור) מכיל את כל האפליקציה.
2. 'סרגל החיפוש' (כחול) מקבל את קלט המשתמש.
3. `ProductTable` (לבנדר) מציג ומסנן את הרשימה בהתאם לקלט המשתמש.
4. `ProductCategoryRow` (ירוק) מציג כותרת עבור כל קטגוריה.
5. `ProductRow` (צהוב) מציג שורה עבור כל מוצר.

</CodeDiagram>

</FullWidth>

אם תסתכל על `ProductTable` (לבנדר), תראה שכותרת הטבלה (המכילה את התוויות "שם" ו"מחיר") אינה רכיב משלה. זה עניין של העדפה, ואתה יכול ללכת לכל כיוון. עבור דוגמה זו, הוא חלק מ'ProductTable' מכיוון שהוא מופיע בתוך הרשימה של 'ProductTable'. עם זאת, אם הכותרת הזו גדלה להיות מורכבת (למשל, אם תוסיף מיון), תוכל להעביר אותה לרכיב 'ProductTableHeader' משלה.

כעת לאחר שזיהית את הרכיבים בדגם, סדר אותם בהיררכיה. רכיבים המופיעים בתוך רכיב אחר בדגם צריכים להופיע בתור ילד בהיררכיה:

* 'TableProductTable'
    * 'סרגל החיפוש'
    * `ProductTable`
        * `ProductCategoryRow`
        * `ProductRow`

## שלב 2: בנה גרסה סטטית ב-React {/*step-2-build-a-static-version-in-react*/}

עכשיו כשיש לך את היררכיית הרכיבים שלך, הגיע הזמן ליישם את האפליקציה שלך. הגישה הפשוטה ביותר היא לבנות גרסה שמציגה את ממשק המשתמש ממודל הנתונים שלך מבלי להוסיף שום אינטראקטיביות... עדיין! לעתים קרובות קל יותר לבנות תחילה את הגרסה הסטטית ולהוסיף אינטראקטיביות מאוחר יותר. בניית גרסה סטטית דורשת הרבה הקלדה וללא חשיבה, אבל הוספת אינטראקטיביות דורשת הרבה חשיבה ולא הרבה הקלדה.

כדי לבנות גרסה סטטית של האפליקציה שלך שמציגה את מודל הנתונים שלך, תרצה לבנות [components](/learn/your-first-component) שעושים שימוש חוזר ברכיבים אחרים ומעבירים נתונים באמצעות [props.](/learn/passing-props-to-a-component) props הם דרך להעביר נתונים מהורה לילד. (אם אתה מכיר את המושג [state](/learn/state-a-components-memory), אל תשתמש כלל ב-state כדי לבנות את הגרסה הסטטית הזו. מצב שמור רק לאינטראקטיביות, כלומר, נתונים המשתנים עם הזמן. מכיוון שזו גרסה סטטית של האפליקציה, אינך צריך אותה.)

אתה יכול לבנות "מלמעלה למטה" על ידי התחלת בבניית הרכיבים גבוה יותר בהיררכיה (כמו `FilterableProductTable`) או "מלמטה למעלה" על ידי עבודה מרכיבים למטה (כמו `ProductRow`). בדוגמאות פשוטות יותר, בדרך כלל קל יותר ללכת מלמעלה למטה, ובפרויקטים גדולים יותר, קל יותר ללכת מלמטה למעלה.

<Sandpack>

```jsx src/App.js
function ProductCategoryRow({ category }) {
  return (
    <tr>
      <th colSpan="2">
        {category}
      </th>
    </tr>
  );
}

function ProductRow({ product }) {
  const name = product.stocked ? product.name :
    <span style={{ color: 'red' }}>
      {product.name}
    </span>;

  return (
    <tr>
      <td>{name}</td>
      <td>{product.price}</td>
    </tr>
  );
}

function ProductTable({ products }) {
  const rows = [];
  let lastCategory = null;

  products.forEach((product) => {
    if (product.category !== lastCategory) {
      rows.push(
        <ProductCategoryRow
          category={product.category}
          key={product.category} />
      );
    }
    rows.push(
      <ProductRow
        product={product}
        key={product.name} />
    );
    lastCategory = product.category;
  });

  return (
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>{rows}</tbody>
    </table>
  );
}

function SearchBar() {
  return (
    <form>
      <input type="text" placeholder="Search..." />
      <label>
        <input type="checkbox" />
        {' '}
        Only show products in stock
      </label>
    </form>
  );
}

function FilterableProductTable({ products }) {
  return (
    <div>
      <SearchBar />
      <ProductTable products={products} />
    </div>
  );
}

const PRODUCTS = [
  {category: "Fruits", price: "$1", stocked: true, name: "Apple"},
  {category: "Fruits", price: "$1", stocked: true, name: "Dragonfruit"},
  {category: "Fruits", price: "$2", stocked: false, name: "Passionfruit"},
  {category: "Vegetables", price: "$2", stocked: true, name: "Spinach"},
  {category: "Vegetables", price: "$4", stocked: false, name: "Pumpkin"},
  {category: "Vegetables", price: "$1", stocked: true, name: "Peas"}
];

export default function App() {
  return <FilterableProductTable products={PRODUCTS} />;
}
```

```css
body {
  padding: 5px
}
label {
  display: block;
  margin-top: 5px;
  margin-bottom: 5px;
}
th {
  padding-top: 10px;
}
td {
  padding: 2px;
  padding-right: 40px;
}
```

</Sandpack>

(אם הקוד הזה נראה מאיים, עברו תחילה על [ההתחלה מהירה](/learn/)!)

לאחר בניית הרכיבים שלך, תהיה לך ספרייה של רכיבים לשימוש חוזר המציגים את מודל הנתונים שלך. מכיוון שמדובר באפליקציה סטטית, הרכיבים יחזירו רק JSX. הרכיב בראש ההיררכיה (`FilterableProductTable`) ייקח את מודל הנתונים שלך כprops. זה נקרא _זרימת נתונים חד כיוונית_ מכיוון שהנתונים זורמים למטה מהרכיב ברמה העליונה לאלה שבתחתית העץ.

<Pitfall>

בשלב זה, אינך אמור להשתמש בשום ערכי מצב. זה לשלב הבא!

</Pitfall>

## שלב 3: מצא את הייצוג המינימלי אך המלא של מצב ממשק המשתמש {/*step-3-find-the-minimal-but-complete-representation-of-ui-state*/}

כדי להפוך את ממשק המשתמש לאינטראקטיבי, עליך לאפשר למשתמשים לשנות את מודל הנתונים הבסיסי שלך. אתה תשתמש ב-*state* בשביל זה.

חשבו על הstate כעל קבוצת הנתונים המשתנים המינימלית שהאפליקציה שלכם צריכה לזכור. העיקרון החשוב ביותר למבנה מצב הוא לשמור אותו [יבש (אל תחזור על עצמך).](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself) גלה את הייצוג המינימלי המוחלט של הstate שהיישום שלך צריך וחשב את כל השאר לפי דרישה. לדוגמה, אם אתה בונה רשימת קניות, אתה יכול לאחסן את הפריטים כמערך בstate. אם ברצונך להציג גם את מספר הפריטים ברשימה, אל תשמור את מספר הפריטים כערך מצב אחר - במקום זאת, קרא את אורך המערך שלך.

עכשיו תחשוב על כל פיסות הנתונים ביישום לדוגמה זה:

1. רשימת המוצרים המקורית
2. טקסט החיפוש שהמשתמש הזין
3. הערך של תיבת הסימון
4. רשימת המוצרים המסוננת

אילו מאלה הם state? זהה את אלה שאינם:

* האם זה **נשאר ללא שינוי** לאורך זמן? אם כן, זה לא state.
* האם זה **מועבר מהורה** באמצעות props? אם כן, זה לא state.
* **האם אתה יכול לחשב את זה** על סמך הstate הקיים או הprops ברכיב שלך? אם כן, זה *בהחלט* לא state!

מה שנשאר זה כנראה הstate.

בואו נעבור עליהם אחד אחד שוב:

1. רשימת המוצרים המקורית **מועברת כprops, כך שהיא לא רשומה.** 
2. נראה שטקסט החיפוש הוא מצב מכיוון שהוא משתנה עם הזמן ולא ניתן לחשב אותו מכלום.
3. נראה שהערך של תיבת הסימון הוא מצב מכיוון שהיא משתנה עם הזמן ואי אפשר לחשב אותה מכלום.
4. רשימת המוצרים המסוננת **לא מוגדרת כי ניתן לחשב אותה** על ידי נטילת רשימת המוצרים המקורית וסינונה בהתאם לטקסט החיפוש ולערך של תיבת הסימון.

זה אומר שרק טקסט החיפוש והערך של תיבת הסימון הם מצב! יפה!

<DeepDive>

#### props נגד state {/*props-vs-state*/}

ישנם שני סוגים של נתוני "מודל" ב-React: props וstate. השניים מאוד שונים:

* [**props** הם כמו ארגומנטים שאתה מעביר](/learn/sending-props-to-a-component) לפונקציה. הם מאפשרים לרכיב אב להעביר נתונים לרכיב צאצא ולהתאים אישית את המראה שלו. לדוגמה, 'טופס' יכול להעביר props 'צבע' ל'כפתור'.
* [**מצב** הוא כמו זיכרון של רכיב.](/learn/state-a-components-memory) הוא מאפשר לרכיב לעקוב אחר מידע מסוים ולשנות אותו בתגובה לאינטראקציות. לדוגמה, `לחצן` עשוי לעקוב אחר מצב `isHovered`.

props וstate שונים, אבל הם עובדים יחד. רכיב הורה ישמור לעתים קרובות מידע מסויים בstate (כדי שיוכל לשנות אותו), ו*יעביר אותו* לרכיבי ילדים בתור הprops שלו. זה בסדר אם ההבדל עדיין מרגיש מעורפל בקריאה הראשונה. צריך קצת תרגול כדי שזה באמת יידבק!

</DeepDive>

## שלב 4: זהה היכן הstate שלך צריכה לגור {/*step-4-identify-where-your-state-should-live*/}

לאחר זיהוי נתוני הstate המינימלי של האפליקציה שלך, עליך לזהות איזה רכיב אחראי לשינוי הstate הזה, או *הבעלים* של הstate. זכור: React משתמשת בזרימת נתונים חד-כיוונית, ומעבירה נתונים במורד היררכיית הרכיבים מהורה לרכיב צאצא. ייתכן שלא ברור מיידית איזה רכיב צריך להיות הבעלים של איזו state. זה יכול להיות מאתגר אם אתה חדש בקונספט הזה, אבל אתה יכול להבין את זה על ידי ביצוע השלבים הבאים!

עבור כל פיסת state בבקשה שלך:

1. זהה *כל* רכיב שמציג משהו על סמך הstate הזה.
2. מצא את רכיב האב המשותף הקרוב ביותר שלהם - רכיב מעל כולם בהיררכיה.
3. להחליט היכן הstate צריכה לגור:
    1. לעתים קרובות, אתה יכול להכניס את הstate ישירות להורה המשותף שלהם.
    2. אתה יכול גם להכניס את הstate לרכיב כלשהו מעל ההורה המשותף שלהם.
    3. אם אינך מוצא רכיב שבו הגיוני להחזיק בstate, צור רכיב חדש אך ורק עבור החזקת הstate והוסף אותו איפשהו בהיררכיה מעל רכיב האב המשותף.

בשלב הקודם, מצאת שני חלקי state ביישום זה: טקסט קלט החיפוש והערך של תיבת הסימון. בדוגמה זו, הם תמיד מופיעים יחד, אז זה הגיוני לשים אותם באותו מקום.

עכשיו בואו נעבור על האסטרטגיה שלנו עבורם:

1. **זהה רכיבים שמשתמשים בstate:**
    * `ProductTable` צריך לסנן את רשימת המוצרים על סמך הstate הזה (טקסט חיפוש וערך תיבת סימון). 
    * 'סרגל החיפוש' צריך להציג את הstate הזה (טקסט חיפוש וערך תיבת סימון).
1. **מצא את האב המשותף שלהם:** רכיב האב הראשון ששני הרכיבים חולקים הוא `FilterableProductTable`.
2. **החלט היכן הstate גרה**: נשמור את טקסט המסנן ואת ערכי הstate המסומנים ב-'FilterableProductTable'.

אז ערכי הstate יחיו ב-'FilterableProductTable'. 

הוסף מצב לרכיב עם [`useState()` Hook.](/reference/react/useState) Hooks הם פונקציות מיוחדות המאפשרות לך "להתחבר" ל-React. הוסף שני משתני מצב בחלק העליון של 'FilterableProductTable' וציין את הstate ההתחלתי שלהם:

```js
function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('');
  const [inStockOnly, setInStockOnly] = useState(false);  
```

לאחר מכן, העבר את 'filterText' ו-'inStockOnly' אל 'ProductTable' ו-'SearchBar' בתור props:

```js
<div>
  <SearchBar 
    filterText={filterText} 
    inStockOnly={inStockOnly} />
  <ProductTable 
    products={products}
    filterText={filterText}
    inStockOnly={inStockOnly} />
</div>
```

אתה יכול להתחיל לראות איך האפליקציה שלך תתנהג. ערוך את הערך ההתחלתי של `filterText` מ`useState('')` ל`useState('fruit')` בקוד ארגז החול למטה. תראה גם את טקסט קלט החיפוש וגם את עדכון הטבלה:

<Sandpack>

```jsx src/App.js
import { useState } from 'react';

function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('');
  const [inStockOnly, setInStockOnly] = useState(false);

  return (
    <div>
      <SearchBar 
        filterText={filterText} 
        inStockOnly={inStockOnly} />
      <ProductTable 
        products={products}
        filterText={filterText}
        inStockOnly={inStockOnly} />
    </div>
  );
}

function ProductCategoryRow({ category }) {
  return (
    <tr>
      <th colSpan="2">
        {category}
      </th>
    </tr>
  );
}

function ProductRow({ product }) {
  const name = product.stocked ? product.name :
    <span style={{ color: 'red' }}>
      {product.name}
    </span>;

  return (
    <tr>
      <td>{name}</td>
      <td>{product.price}</td>
    </tr>
  );
}

function ProductTable({ products, filterText, inStockOnly }) {
  const rows = [];
  let lastCategory = null;

  products.forEach((product) => {
    if (
      product.name.toLowerCase().indexOf(
        filterText.toLowerCase()
      ) === -1
    ) {
      return;
    }
    if (inStockOnly && !product.stocked) {
      return;
    }
    if (product.category !== lastCategory) {
      rows.push(
        <ProductCategoryRow
          category={product.category}
          key={product.category} />
      );
    }
    rows.push(
      <ProductRow
        product={product}
        key={product.name} />
    );
    lastCategory = product.category;
  });

  return (
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>{rows}</tbody>
    </table>
  );
}

function SearchBar({ filterText, inStockOnly }) {
  return (
    <form>
      <input 
        type="text" 
        value={filterText} 
        placeholder="Search..."/>
      <label>
        <input 
          type="checkbox" 
          checked={inStockOnly} />
        {' '}
        Only show products in stock
      </label>
    </form>
  );
}

const PRODUCTS = [
  {category: "Fruits", price: "$1", stocked: true, name: "Apple"},
  {category: "Fruits", price: "$1", stocked: true, name: "Dragonfruit"},
  {category: "Fruits", price: "$2", stocked: false, name: "Passionfruit"},
  {category: "Vegetables", price: "$2", stocked: true, name: "Spinach"},
  {category: "Vegetables", price: "$4", stocked: false, name: "Pumpkin"},
  {category: "Vegetables", price: "$1", stocked: true, name: "Peas"}
];

export default function App() {
  return <FilterableProductTable products={PRODUCTS} />;
}
```

```css
body {
  padding: 5px
}
label {
  display: block;
  margin-top: 5px;
  margin-bottom: 5px;
}
th {
  padding-top: 5px;
}
td {
  padding: 2px;
}
```

</Sandpack>

שימו לב שעריכת הטופס עדיין לא עובדת. יש שגיאת קונסולה בארגז החול למעלה שמסבירה מדוע:

<ConsoleBlock level="error">

סיפקת props \`value\` לשדה טופס ללא מטפל \`onChange\`. זה יציג שדה לקריאה בלבד.

</ConsoleBlock>

בארגז החול שלמעלה, `ProductTable` ו`SearchBar` קראו את props `filterText` ו-`inStockOnly` כדי להציג את הטבלה, הקלט ותיבה. לדוגמה, כך 'סרגל החיפוש' מאכלס את ערך הקלט:

```js {1,6}
function SearchBar({ filterText, inStockOnly }) {
  return (
    <form>
      <input 
        type="text" 
        value={filterText} 
        placeholder="Search..."/>
```

עם זאת, עדיין לא הוספת קוד כדי להגיב לפעולות המשתמש כמו הקלדה. זה יהיה הצעד האחרון שלך.


## שלב 5: הוסף זרימת נתונים הפוכה {/*step-5-add-inverse-data-flow*/}

נכון לעכשיו, האפליקציה שלך מוצגת בצורה נכונה עם props וstate זורמים במורד ההיררכיה. אבל כדי לשנות את הstate בהתאם לקלט המשתמש, תצטרך לתמוך בנתונים הזורמים לכיוון השני: רכיבי הטופס עמוק בהיררכיה צריכים לעדכן את הstate ב-'FilterableProductTable'. 

React הופך את זרימת הנתונים הזו למפורשת, אבל היא דורשת קצת יותר הקלדה מאשר איגוד נתונים דו-כיווני. אם תנסה להקליד או לסמן את התיבה בדוגמה למעלה, תראה ש-React מתעלם מהקלט שלך. זה מכוון. על ידי כתיבת `<input value={filterText} />`, הגדרת את הפרופס של `value` של `input` להיות תמיד שווה לstate `filterText` המועבר מ`FilterableProductTable`. מכיוון שstate 'filterText' לעולם אינו מוגדר, הקלט לעולם לא משתנה.

אתה רוצה להפוך את זה כך שבכל פעם שהמשתמש משנה את קלט הטופס, הstate מתעדכן כדי לשקף את השינויים האלה. הstate נמצאת בבעלות `FilterableProductTable`, כך שרק היא יכולה לקרוא ל`setFilterText` ו-`setInStockOnly`. כדי לאפשר ל-SearchBar לעדכן את מצב ה-FilterableProductTable, עליך להעביר את הפונקציות האלה ל-SearchBar:

```js {2,3,10,11}
function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('');
  const [inStockOnly, setInStockOnly] = useState(false);

  return (
    <div>
      <SearchBar 
        filterText={filterText} 
        inStockOnly={inStockOnly}
        onFilterTextChange={setFilterText}
        onInStockOnlyChange={setInStockOnly} />
```

בתוך 'סרגל החיפוש', תוסיף את מטפלי האירועים 'onChange' ותגדיר מהם את מצב האב:

```js {4,5,13,19}
function SearchBar({
  filterText,
  inStockOnly,
  onFilterTextChange,
  onInStockOnlyChange
}) {
  return (
    <form>
      <input
        type="text"
        value={filterText}
        placeholder="Search..."
        onChange={(e) => onFilterTextChange(e.target.value)}
      />
      <label>
        <input
          type="checkbox"
          checked={inStockOnly}
          onChange={(e) => onInStockOnlyChange(e.target.checked)}
```

עכשיו האפליקציה עובדת במלואה!

<Sandpack>

```jsx src/App.js
import { useState } from 'react';

function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('');
  const [inStockOnly, setInStockOnly] = useState(false);

  return (
    <div>
      <SearchBar 
        filterText={filterText} 
        inStockOnly={inStockOnly} 
        onFilterTextChange={setFilterText} 
        onInStockOnlyChange={setInStockOnly} />
      <ProductTable 
        products={products} 
        filterText={filterText}
        inStockOnly={inStockOnly} />
    </div>
  );
}

function ProductCategoryRow({ category }) {
  return (
    <tr>
      <th colSpan="2">
        {category}
      </th>
    </tr>
  );
}

function ProductRow({ product }) {
  const name = product.stocked ? product.name :
    <span style={{ color: 'red' }}>
      {product.name}
    </span>;

  return (
    <tr>
      <td>{name}</td>
      <td>{product.price}</td>
    </tr>
  );
}

function ProductTable({ products, filterText, inStockOnly }) {
  const rows = [];
  let lastCategory = null;

  products.forEach((product) => {
    if (
      product.name.toLowerCase().indexOf(
        filterText.toLowerCase()
      ) === -1
    ) {
      return;
    }
    if (inStockOnly && !product.stocked) {
      return;
    }
    if (product.category !== lastCategory) {
      rows.push(
        <ProductCategoryRow
          category={product.category}
          key={product.category} />
      );
    }
    rows.push(
      <ProductRow
        product={product}
        key={product.name} />
    );
    lastCategory = product.category;
  });

  return (
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>{rows}</tbody>
    </table>
  );
}

function SearchBar({
  filterText,
  inStockOnly,
  onFilterTextChange,
  onInStockOnlyChange
}) {
  return (
    <form>
      <input 
        type="text" 
        value={filterText} placeholder="Search..." 
        onChange={(e) => onFilterTextChange(e.target.value)} />
      <label>
        <input 
          type="checkbox" 
          checked={inStockOnly} 
          onChange={(e) => onInStockOnlyChange(e.target.checked)} />
        {' '}
        Only show products in stock
      </label>
    </form>
  );
}

const PRODUCTS = [
  {category: "Fruits", price: "$1", stocked: true, name: "Apple"},
  {category: "Fruits", price: "$1", stocked: true, name: "Dragonfruit"},
  {category: "Fruits", price: "$2", stocked: false, name: "Passionfruit"},
  {category: "Vegetables", price: "$2", stocked: true, name: "Spinach"},
  {category: "Vegetables", price: "$4", stocked: false, name: "Pumpkin"},
  {category: "Vegetables", price: "$1", stocked: true, name: "Peas"}
];

export default function App() {
  return <FilterableProductTable products={PRODUCTS} />;
}
```

```css
body {
  padding: 5px
}
label {
  display: block;
  margin-top: 5px;
  margin-bottom: 5px;
}
th {
  padding: 4px;
}
td {
  padding: 2px;
}
```

</Sandpack>

תוכל ללמוד הכל על טיפול באירועים וstate עדכון בקטע [הוספת אינטראקטיביות](/learn/adding-interactivity).

## לאן ללכת מכאן {/*לאן-ללכת-מכאן*/}

זה היה הקדמה קצרה מאוד כיצד לחשוב על בניית רכיבים ויישומים עם React. אתה יכול [להתחיל פרויקט React](/learn/installation) עכשיו או [לצלול עמוק יותר על כל התחביר](/learn/describing-the-ui) בשימוש במדריך זה.

