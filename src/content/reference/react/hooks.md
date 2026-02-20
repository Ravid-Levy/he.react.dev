---
title: "React Hooks מובנים"
---

<Intro>

*Hooks* מאפשרים להשתמש ביכולות שונות של React מתוך קומפוננטות. אפשר להשתמש ב-Hooks המובנים, או לשלב ביניהם כדי לבנות Hooks משלכם. העמוד הזה מציג את כל ה-Hooks המובנים ב-React.

</Intro>

---

## State Hooks {/*state-hooks*/}

*State* מאפשר לקומפוננטה ["לזכור" מידע כמו קלט מהמשתמש.](/learn/state-a-components-memory) לדוגמה, קומפוננטת טופס יכולה להשתמש ב-state כדי לשמור את ערך הקלט, בעוד קומפוננטת גלריית תמונות יכולה להשתמש ב-state כדי לשמור את אינדקס התמונה שנבחרה.

כדי להוסיף state לקומפוננטה, השתמשו באחד מה-Hooks הבאים:

* [`useState`](/reference/react/useState) מצהיר על משתנה state שאפשר לעדכן ישירות.
* [`useReducer`](/reference/react/useReducer) מצהיר על משתנה state עם לוגיקת העדכון בתוך [פונקציית reducer.](/learn/extracting-state-logic-into-a-reducer)

```js
function ImageGallery() {
  const [index, setIndex] = useState(0);
  // ...
```

---

## Context Hooks {/*context-hooks*/}

*Context* מאפשר לקומפוננטה [לקבל מידע מהורים רחוקים בלי להעביר אותו כ-props.](/learn/passing-props-to-a-component) לדוגמה, קומפוננטת העליונה של האפליקציה יכולה להעביר את ערכת הנושא הנוכחית לכל הקומפוננטות מתחתיה, לא משנה כמה עמוק הן נמצאות.

* [`useContext`](/reference/react/useContext) קורא ל-context ונרשם אליו.

```js
function Button() {
  const theme = useContext(ThemeContext);
  // ...
```

---

## Ref Hooks {/*ref-hooks*/}

*Refs* מאפשרים לקומפוננטה [להחזיק מידע שלא משמש לרינדור,](/learn/referencing-values-with-refs) כמו DOM node או מזהה timeout. בניגוד ל-state, עדכון של ref לא מרנדר מחדש את הקומפוננטה. Refs הם "escape hatch" מהפרדיגמה של React. הם שימושיים כשצריך לעבוד עם מערכות שאינן React, כמו APIs מובנים של הדפדפן.

* [`useRef`](/reference/react/useRef) מצהיר על ref. אפשר לשמור בו כל ערך, אבל לרוב משתמשים בו כדי לשמור DOM node.
* [`useImperativeHandle`](/reference/react/useImperativeHandle) מאפשר להתאים אישית את ה-ref שהקומפוננטה חושפת. זה בשימוש נדיר.

```js
function Form() {
  const inputRef = useRef(null);
  // ...
```

---

## Effect Hooks {/*effect-hooks*/}

*Effects* מאפשרים לקומפוננטה [להתחבר ולהסתנכרן עם מערכות חיצוניות.](/learn/synchronizing-with-effects) זה כולל עבודה עם רשת, DOM של הדפדפן, אנימציות, ווידג'טים שנכתבו בספריית UI אחרת, וקוד נוסף שאינו React.

* [`useEffect`](/reference/react/useEffect) מחבר קומפוננטה למערכת חיצונית.

```js
function ChatRoom({ roomId }) {
  useEffect(() => {
    const connection = createConnection(roomId);
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]);
  // ...
```

Effects הם "escape hatch" מהפרדיגמה של React. אל תשתמשו ב-Effects כדי לתזמר את זרימת הנתונים של האפליקציה. אם אתם לא מקיימים אינטראקציה עם מערכת חיצונית, [יכול להיות שאין צורך ב-Effect.](/learn/you-might-not-need-an-effect)

יש שתי וריאציות נדירות יותר של `useEffect` עם הבדלים בתזמון:

* [`useLayoutEffect`](/reference/react/useLayoutEffect) מופעל לפני שהדפדפן מצייר מחדש את המסך. אפשר למדוד layout כאן.
* [`useInsertionEffect`](/reference/react/useInsertionEffect) מופעל לפני ש-React מבצעת שינויים ב-DOM. ספריות יכולות להכניס כאן CSS דינמי.

---

## Performance Hooks {/*performance-hooks*/}

דרך נפוצה לאופטימיזציה של ביצועי רינדור מחדש היא לדלג על עבודה מיותרת. לדוגמה, אפשר לבקש מ-React להשתמש מחדש בחישוב שמור או לדלג על רינדור חוזר אם הנתונים לא השתנו מאז הרינדור הקודם.

כדי לדלג על חישובים ורינדורים מיותרים, השתמשו באחד מה-Hooks הבאים:

- [`useMemo`](/reference/react/useMemo) מאפשר לשמור במטמון תוצאה של חישוב יקר.
- [`useCallback`](/reference/react/useCallback) מאפשר לשמור במטמון הגדרת פונקציה לפני שמעבירים אותה לקומפוננטה ממוטבת.

```js
function TodoList({ todos, tab, theme }) {
  const visibleTodos = useMemo(() => filterTodos(todos, tab), [todos, tab]);
  // ...
}
```

לפעמים אי אפשר לדלג על רינדור מחדש כי המסך באמת צריך להתעדכן. במקרה כזה אפשר לשפר ביצועים על ידי הפרדה בין עדכונים חוסמים שחייבים להיות סינכרוניים (כמו הקלדה בשדה קלט) לבין עדכונים לא-חוסמים שלא צריכים לחסום את ממשק המשתמש (כמו עדכון גרף).

כדי לתעדף רינדור, השתמשו באחד מה-Hooks הבאים:

- [`useTransition`](/reference/react/useTransition) מאפשר לסמן מעבר state כלא-חוסם ולאפשר לעדכונים אחרים להפריע לו.
- [`useDeferredValue`](/reference/react/useDeferredValue) מאפשר לדחות עדכון של חלק לא-קריטי ב-UI ולאפשר לחלקים אחרים להתעדכן קודם.

---

## Resource Hooks {/*resource-hooks*/}

*Resources* ניתנים לגישה מתוך קומפוננטה גם בלי להיות חלק מה-state שלה. לדוגמה, קומפוננטה יכולה לקרוא הודעה מתוך Promise או מידע עיצובי מתוך context.

כדי לקרוא ערך ממשאב, השתמשו ב-Hook הבא:

- [`use`](/reference/react/use) מאפשר לקרוא ערך ממשאב כמו [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) או [context](/learn/passing-data-deeply-with-context).

```js
function MessageComponent({ messagePromise }) {
  const message = use(messagePromise);
  const theme = use(ThemeContext);
  // ...
}
```

---

## Hooks נוספים {/*other-hooks*/}

ה-Hooks האלה שימושיים בעיקר למחברי ספריות, ולא בשימוש נפוץ בקוד אפליקטיבי.

- [`useDebugValue`](/reference/react/useDebugValue) מאפשר להתאים את התווית ש-React DevTools מציג עבור custom Hook.
- [`useId`](/reference/react/useId) מאפשר לקומפוננטה לשייך לעצמה מזהה ייחודי. לרוב בשילוב עם APIs של נגישות.
- [`useSyncExternalStore`](/reference/react/useSyncExternalStore) מאפשר לקומפוננטה להירשם לחנות חיצונית.

---

## Hooks משלכם {/*your-own-hooks*/}

אפשר גם [להגדיר custom Hooks משלכם](/learn/reusing-logic-with-custom-hooks#extracting-your-own-custom-hook-from-a-component) כפונקציות JavaScript.
