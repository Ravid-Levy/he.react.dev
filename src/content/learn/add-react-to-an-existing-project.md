---
title: "הוספת React לפרויקט קיים"
---

<Intro>

אם אתם רוצים להוסיף אינטראקטיביות לפרויקט קיים, אין צורך לכתוב אותו מחדש ב-React. הוסיפו React ל-stack הקיים שלכם, ורנדרו קומפוננטות React אינטראקטיביות בכל מקום.

</Intro>

<Note>

**צריך להתקין [Node.js](https://nodejs.org/en/) לפיתוח מקומי.** למרות שאפשר [לנסות React](/learn/installation#try-react) אונליין או עם דף HTML פשוט, רוב כלי הפיתוח של JavaScript שתרצו להשתמש בהם דורשים Node.js.

</Note>

## שימוש ב-React עבור תת-נתיב (subroute) שלם באתר קיים {/*using-react-for-an-hele-subroute-of-your-existing-site*/}

נניח שיש לכם אפליקציית ווב קיימת ב-`example.com` שבנויה בטכנולוגיית שרת אחרת (למשל Rails), ואתם רוצים שכל הנתיבים שמתחילים ב-`example.com/some-app/` ימומשו במלואם עם React.

כך אנחנו ממליצים להגדיר זאת:

1. **בנו את חלק ה-React באפליקציה** באמצעות אחד מ-[frameworks שמבוססים על React](/learn/start-a-new-react-project).
2. **הגדירו את `/some-app` כ-*base path*** בקונפיגורציית ה-framework (ראו: [Next.js](https://nextjs.org/docs/api-reference/next.config.js/basepath), [Gatsby](https://www.gatsbyjs.com/docs/how-to/previews-deploys-hosting/path-prefix/)).
3. **הגדירו את השרת או proxy** כך שכל הבקשות תחת `/some-app/` יטופלו על ידי אפליקציית React.

כך החלק של React באפליקציה [ייהנה ממיטב הפרקטיקות](/learn/start-a-new-react-project#can-i-use-react-without-a-framework) שמובנות ב-frameworks האלה.

רבים מה-frameworks מבוססי React הם full-stack ומאפשרים לאפליקציית React לנצל יכולות שרת. עם זאת, אפשר להשתמש באותה גישה גם אם אינכם יכולים או לא רוצים להריץ JavaScript על השרת. במקרה כזה, אפשר להגיש תחת `some-app/` פלט סטטי של HTML/CSS/JS ([פלט של `next export`](https://nextjs.org/docs/advanced-features/static-html-export) ב-Next.js, וברירת המחדל ב-Gatsby).

## שימוש ב-React עבור חלק מעמוד קיים {/*using-react-for-a-part-of-your-existing-page*/}

נניח שיש לכם עמוד קיים שבנוי בטכנולוגיה אחרת (שרתית כמו Rails או בצד לקוח), ואתם רוצים לרנדר קומפוננטות React אינטראקטיביות באזור מסוים בעמוד. למעשה, כך חלק גדול מהשימוש ב-React ב-Meta נראה במשך שנים רבות.

אפשר לעשות זאת בשני שלבים:

1. **הגדירו סביבת JavaScript** שמאפשרת להשתמש ב-[תחביר JSX](/learn/writing-markup-with-jsx), לפצל קוד למודולים עם [`import`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) ו-[`export`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export), ולהשתמש בחבילות מ-[npm](https://www.npmjs.com/), כולל React.
2. **רנדרו קומפוננטות React** במקומות שבהם אתם רוצים שיופיעו בעמוד.

הגישה המדויקת תלויה ב-setup הנוכחי של העמוד, אז נעבור על הפרטים.

### שלב 1: הגדרת סביבת JavaScript מודולרית {/*step-1-set-up-a-modular-javascript-environment*/}

סביבת JavaScript מודולרית מאפשרת לכתוב קומפוננטות React בקבצים נפרדים במקום לרכז הכול בקובץ אחד. בנוסף, היא מאפשרת להשתמש בחבילות שזמינות ב-[npm](https://www.npmjs.com/), כולל React עצמה. איך עושים את זה תלוי ב-setup הקיים שלכם:

* **אם האפליקציה שלכם כבר מפוצלת לקבצים שמשתמשים ב-`import`,** נסו להשתמש ב-setup הקיים. בדקו אם כתיבה של `<div />` בקוד JS גורמת לשגיאת תחביר. אם כן, ייתכן שתצטרכו [להעביר את הקוד דרך Babel](https://babeljs.io/setup) ולהפעיל את [Babel React preset](https://babeljs.io/docs/babel-preset-react) כדי להשתמש ב-JSX.

* **אם לאפליקציה שלכם אין setup לקומפילציה של מודולי JavaScript,** הגדירו זאת עם [Vite](https://vitejs.dev/). קהילת Vite מתחזקת [אינטגרציות רבות עם backends](https://github.com/vitejs/awesome-vite#integrations-with-backends), כולל Rails, Django ו-Laravel. אם ה-framework שלכם לא מופיע שם, [עקבו אחרי המדריך הזה](https://vitejs.dev/guide/backend-integration.html) כדי לשלב ידנית את Vite עם ה-backend.

כדי לבדוק שה-setup עובד, הריצו את הפקודה הזו בתיקיית הפרויקט:

<TerminalBlock>
npm install react react-dom
</TerminalBlock>

לאחר מכן הוסיפו את שורות הקוד האלה בראש קובץ ה-JavaScript הראשי (למשל `index.js` או `main.js`):

<Sandpack>

```html index.html hidden
<!DOCTYPE html>
<html>
  <head><title>My app</title></head>
  <body>
    <!-- Your existing page content (in this example, it gets replaced) -->
  </body>
</html>
```

```js src/index.js active
import { createRoot } from 'react-dom/client';

// Clear the existing HTML content
document.body.innerHTML = '<div id="app"></div>';

// Render your React component instead
const root = createRoot(document.getElementById('app'));
root.render(<h1>Hello, world</h1>);
```

</Sandpack>

אם כל התוכן בעמוד הוחלף ב-"Hello, world!", הכול עבד. המשיכו לקרוא.

<Note>

שילוב סביבת JavaScript מודולרית בפרויקט קיים בפעם הראשונה יכול להרגיש מרתיע, אבל זה שווה את זה. אם נתקעתם, נסו את [משאבי הקהילה](/community) או את [Vite Chat](https://chat.vitejs.dev/).

</Note>

### שלב 2: רינדור קומפוננטות React בכל מקום בעמוד {/*step-2-render-react-components-anywhere-on-the-page*/}

בשלב הקודם שמתם את הקוד הזה בראש הקובץ הראשי:

```js
import { createRoot } from 'react-dom/client';

// Clear the existing HTML content
document.body.innerHTML = '<div id="app"></div>';

// Render your React component instead
const root = createRoot(document.getElementById('app'));
root.render(<h1>Hello, world</h1>);
```

כמובן, אתם לא באמת רוצים למחוק את תוכן ה-HTML הקיים.

מחקו את הקוד הזה.

במקום זאת, סביר שתרצו לרנדר קומפוננטות React בנקודות ספציפיות ב-HTML שלכם. פתחו את עמוד ה-HTML (או את תבניות השרת שמייצרות אותו) והוסיפו מאפיין [`id`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/id) ייחודי לכל תגית יעד, לדוגמה:

```html
<!-- ... somewhere in your html ... -->
<nav id="navigation"></nav>
<!-- ... more html ... -->
```

כך תוכלו למצוא את אלמנט ה-HTML עם [`document.getElementById`](https://developer.mozilla.org/en-US/docs/Web/API/Document/getElementById) ולהעביר אותו ל-[`createRoot`](/reference/react-dom/client/createRoot):

<Sandpack>

```html index.html
<!DOCTYPE html>
<html>
  <head><title>My app</title></head>
  <body>
    <p>This paragraph is a part of HTML.</p>
    <nav id="navigation"></nav>
    <p>This paragraph is also a part of HTML.</p>
  </body>
</html>
```

```js src/index.js active
import { createRoot } from 'react-dom/client';

function NavigationBar() {
  // TODO: Actually implement a navigation bar
  return <h1>Hello from React!</h1>;
}

const domNode = document.getElementById('navigation');
const root = createRoot(domNode);
root.render(<NavigationBar />);
```

</Sandpack>

שימו לב שתוכן ה-HTML המקורי מ-`index.html` נשמר, אבל קומפוננטת React בשם `NavigationBar` מופיעה עכשיו בתוך `<nav id="navigation">`. קראו את [תיעוד השימוש ב-`createRoot`](/reference/react-dom/client/createRoot#rendering-a-page-partially-built-with-react) כדי ללמוד עוד על רינדור קומפוננטות React בתוך עמוד HTML קיים.

כשמאמצים React בפרויקט קיים, נפוץ להתחיל בקומפוננטות אינטראקטיביות קטנות (כמו כפתורים), ואז להתקדם בהדרגה "למעלה" עד שלבסוף כל העמוד בנוי ב-React. אם הגעתם לנקודה הזו, אנחנו ממליצים לעבור ל-[React framework](/learn/start-a-new-react-project) כדי להפיק את המקסימום מ-React.

## שימוש ב-React Native באפליקציית מובייל native קיימת {/*using-react-native-in-an-existing-native-mobile-app*/}

אפשר לשלב [React Native](https://reactnative.dev/) גם באפליקציות native קיימות בצורה הדרגתית. אם יש לכם אפליקציית native קיימת ל-Android (Java או Kotlin) או ל-iOS (Objective-C או Swift), [עקבו אחרי המדריך הזה](https://reactnative.dev/docs/integration-with-existing-apps) כדי להוסיף לה מסך React Native.
