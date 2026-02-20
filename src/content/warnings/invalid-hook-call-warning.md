---
title: כללי Hooks
---

כנראה הגעתם לכאן כי קיבלתם את הודעת השגיאה הבאה:

<ConsoleBlock level="error">

Hooks can only be called inside the body of a function component.

</ConsoleBlock>

יש שלוש סיבות נפוצות לכך:

1. ייתכן שאתם **מפרים את כללי Hooks**.
2. ייתכן שיש לכם **גרסאות לא תואמות** של React ו-React DOM.
3. ייתכן שיש לכם **יותר מעותק אחד של React** באותה אפליקציה.

בואו נעבור על כל אחד מהמקרים.

## הפרת כללי Hooks {/*breaking-rules-of-hooks*/}

פונקציות שהשם שלהן מתחיל ב-`use` נקראות [*Hooks*](/reference/react) ב-React.

**אל תקראו ל-Hooks בתוך לולאות, תנאים או פונקציות מקוננות.** במקום זאת, תמיד השתמשו ב-Hooks ברמה העליונה של פונקציית React שלכם, לפני כל early return. אפשר לקרוא ל-Hooks רק בזמן ש-React מרנדר function component:

* ✅ קראו להם ברמה העליונה בגוף של [function component](/learn/your-first-component).
* ✅ קראו להם ברמה העליונה בגוף של [custom Hook](/learn/reusing-logic-with-custom-hooks).

```js{2-3,8-9}
function Counter() {
  // ✅ Good: top-level in a function component
  const [count, setCount] = useState(0);
  // ...
}

function useWindowWidth() {
  // ✅ Good: top-level in a custom Hook
  const [width, setWidth] = useState(window.innerWidth);
  // ...
}
```

**לא** נתמך לקרוא ל-Hooks (פונקציות שמתחילות ב-`use`) במקרים אחרים, לדוגמה:

* 🔴 אל תקראו ל-Hooks בתוך תנאים או לולאות.
* 🔴 אל תקראו ל-Hooks אחרי משפט `return` מותנה.
* 🔴 אל תקראו ל-Hooks בתוך event handlers.
* 🔴 אל תקראו ל-Hooks ב-class components.
* 🔴 אל תקראו ל-Hooks בתוך פונקציות שמועברות ל-`useMemo`, `useReducer`, או `useEffect`.

אם תפרו את הכללים האלה, ייתכן שתראו את השגיאה הזו.

```js{3-4,11-12,20-21}
function Bad({ cond }) {
  if (cond) {
    // 🔴 Bad: inside a condition (to fix, move it outside!)
    const theme = useContext(ThemeContext);
  }
  // ...
}

function Bad() {
  for (let i = 0; i < 10; i++) {
    // 🔴 Bad: inside a loop (to fix, move it outside!)
    const theme = useContext(ThemeContext);
  }
  // ...
}

function Bad({ cond }) {
  if (cond) {
    return;
  }
  // 🔴 Bad: after a conditional return (to fix, move it before the return!)
  const theme = useContext(ThemeContext);
  // ...
}

function Bad() {
  function handleClick() {
    // 🔴 Bad: inside an event handler (to fix, move it outside!)
    const theme = useContext(ThemeContext);
  }
  // ...
}

function Bad() {
  const style = useMemo(() => {
    // 🔴 Bad: inside useMemo (to fix, move it outside!)
    const theme = useContext(ThemeContext);
    return createStyle(theme);
  });
  // ...
}

class Bad extends React.Component {
  render() {
    // 🔴 Bad: inside a class component (to fix, write a function component instead of a class!)
    useEffect(() => {})
    // ...
  }
}
```

אפשר להשתמש ב-[`eslint-plugin-react-hooks` plugin](https://www.npmjs.com/package/eslint-plugin-react-hooks) כדי לזהות טעויות כאלה.

<Note>

[Custom Hooks](/learn/reusing-logic-with-custom-hooks) *יכולים* לקרוא ל-Hooks אחרים (זו כל המטרה שלהם). זה עובד כי גם custom Hooks אמורים להיקרא רק בזמן ש-function component מרונדר.

</Note>

## גרסאות לא תואמות של React ו-React DOM {/*mismatching-versions-of-react-and-react-dom*/}

ייתכן שאתם משתמשים בגרסה של `react-dom` (פחות מ-16.8.0) או `react-native` (פחות מ-0.59) שעוד לא תומכת ב-Hooks. אפשר להריץ `npm ls react-dom` או `npm ls react-native` בתיקיית האפליקציה כדי לבדוק באיזו גרסה אתם משתמשים. אם אתם מוצאים יותר מאחת, זה עלול ליצור בעיות נוספות (הסבר בהמשך).

## React כפול {/*duplicate-react*/}

כדי ש-Hooks יעבדו, ה-`react` import מהקוד של האפליקציה צריך להיפתר לאותו מודול כמו ה-`react` import מתוך החבילה `react-dom`.

אם שני ה-imports האלה של `react` נפתרים לשני export objects שונים, תראו את האזהרה הזאת. זה יכול לקרות אם **בטעות יש לכם שני עותקים** של החבילה `react`.

אם אתם משתמשים ב-Node לניהול חבילות, אפשר להריץ את הבדיקה הבאה בתיקיית הפרויקט:

<TerminalBlock>

npm ls react

</TerminalBlock>

אם מופיעים יותר מעותק אחד של React, תצטרכו להבין למה זה קורה ולתקן את dependency tree. לדוגמה, ייתכן שספרייה שאתם משתמשים בה הגדירה את `react` כתלות רגילה במקום peer dependency. עד שהספרייה תתוקן, [Yarn resolutions](https://yarnpkg.com/lang/en/docs/selective-version-resolutions/) היא דרך אפשרית לעקיפה.

אפשר גם לנסות לדבג את הבעיה הזו על ידי הוספת לוגים והפעלה מחדש של שרת הפיתוח:

```js
// Add this in node_modules/react-dom/index.js
window.React1 = require('react');

// Add this in your component file
require('react-dom');
window.React2 = require('react');
console.log(window.React1 === window.React2);
```

אם יודפס `false`, כנראה שיש לכם שני עותקים של React וצריך לברר למה זה קרה. ב-[issue הזה](https://github.com/facebook/react/issues/13991) יש סיבות נפוצות שהקהילה נתקלה בהן.

הבעיה הזו יכולה להופיע גם כשמשתמשים ב-`npm link` או מקבילה שלו. במצב כזה ה-bundler עשוי "לראות" שני עותקים של React - אחד בתיקיית האפליקציה ואחד בתיקיית הספרייה. בהנחה ש-`myapp` ו-`mylib` הן תיקיות אחיות, פתרון אפשרי הוא להריץ `npm link ../myapp/node_modules/react` מתוך `mylib`. זה אמור לגרום לספרייה להשתמש בעותק React של האפליקציה.

<Note>

באופן כללי, React תומכת בכמה עותקים עצמאיים באותו עמוד (לדוגמה, אפליקציה ו-widget צד שלישי שמשתמשים בה במקביל). זה נשבר רק אם `require('react')` נפתר אחרת בין הקומפוננטה לבין העותק של `react-dom` שאיתו היא רונדרה.

</Note>

## סיבות נוספות {/*other-causes*/}

אם שום דבר מזה לא עזר, אנא הוסיפו תגובה ב-[issue הזה](https://github.com/facebook/react/issues/13991) וננסה לעזור. מומלץ ליצור דוגמת שחזור קטנה - הרבה פעמים מגלים את הבעיה כבר בתהליך יצירת הדוגמה.
