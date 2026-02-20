---
title: "שימוש ב-TypeScript"
re: https://github.com/reactjs/react.dev/issues/5960
---

<Intro>

TypeScript היא דרך פופולרית להוסיף הגדרות סוג לבסיסי קוד של JavaScript. מהקופסה, TypeScript [תומך ב-JSX](/learn/writing-markup-with-jsx) ותוכל לקבל תמיכה מלאה באינטרנט של React על ידי הוספת [`@types/react`](https://www.npmjs.com/package/@types/react) ו-[`@types/react-dom`](j/https://www.com/pm) your pack פרויקט.

</Intro>

<YouWillLearn>

* [TypeScript עם רכיבי React](/learn/typescript#typescript-with-react-components)
* [דוגמאות של הקלדה עם Hooks](/learn/typescript#example-hooks)
* [סוגים נפוצים מ-`@types/react`](/learn/typescript/#useful-types)
* [מיקומי למידה נוספים](/learn/typescript/#further-learning)

</YouWillLearn>

## התקנה {/*התקנה*/}

כל [מסגרות React בדרגת יצירה](/learn/start-a-new-react-project#production-grade-react-frameworks) מציעות תמיכה לשימוש ב-TypeScript. עקוב אחר המדריך הספציפי למסגרת להתקנה:

- [Next.js](https://nextjs.org/docs/pages/building-your-application/configuring/typescript)
- [רמיקס](https://remix.run/docs/en/1.19.2/guides/typescript)
- [גטסבי](https://www.gatsbyjs.com/docs/how-to/custom-configuration/typescript/)
- [אקספו](https://docs.expo.dev/guides/typescript/)

### הוספת TypeScript לפרויקט React קיים {/*adding-typescript-to-an-existing-react-project*/}

כדי להתקין את הגרסה העדכנית ביותר של הגדרות הסוג של React:

<TerminalBlock>
npm להתקין את @types/react @types/react-dom
</TerminalBlock>

יש להגדיר את אפשרויות המהדר הבאות ב-`tsconfig.json` שלך:

1. יש לכלול `dom` ב-[`lib`](https://www.typescriptlang.org/tsconfig/#lib) (הערה: אם לא צוינה אפשרות `lib`, `dom` נכלל כברירת מחדל).
1. יש להגדיר את [`jsx`](https://www.typescriptlang.org/tsconfig/#jsx) לאחת מהאפשרויות החוקיות. `שמר` אמור להספיק לרוב היישומים.
  אם אתה מפרסם ספריה, עיין בתיעוד [`jsx`](https://www.typescriptlang.org/tsconfig/#jsx) על איזה ערך לבחור.

## TypeScript עם רכיבי React {/*typescript-with-react-components*/}

<Note>

כל קובץ המכיל JSX חייב להשתמש בסיומת הקובץ `.tsx`. זוהי הרחבה ספציפית ל-TypeScript שאומרת ל-TypeScript שהקובץ הזה מכיל JSX.

</Note>

כתיבת TypeScript עם React דומה מאוד לכתיבת JavaScript עם React. ההבדל העיקרי בעבודה עם רכיב הוא שאתה יכול לספק סוגים עבור הprops של הרכיב שלך. ניתן להשתמש בסוגים אלה לבדיקת נכונות ולמתן תיעוד מוטבע בעורכים.

אם ניקח את הרכיב [`MyButton`](/learn#components) מהמדריך [התחלה מהירה](/learn), נוכל להוסיף סוג המתאר את ה`כותרת` עבור הכפתור:

<Sandpack>

```tsx src/App.tsx active
function MyButton({ title }: { title: string }) {
  return (
    <button>{title}</button>
  );
}

export default function MyApp() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton title="I'm a button" />
    </div>
  );
}
```

```js src/App.js hidden
import AppTSX from "./App.tsx";
export default App = AppTSX;
```
</Sandpack>

 <Note>

ארגזי חול אלו יכולים להתמודד עם קוד TypeScript, אך הם לא מריצים את בודק הסוג. זה אומר שאתה יכול לשנות את ארגזי החול של TypeScript כדי ללמוד, אבל לא תקבל שגיאות סוג או אזהרות. כדי לבצע בדיקת סוגים, אתה יכול להשתמש ב-[TypeScript Playground](https://www.typescriptlang.org/play) או להשתמש בארגז חול מקוון מלא יותר.

</Note>

תחביר מוטבע זה הוא הדרך הפשוטה ביותר לספק טיפוסים עבור רכיב, אם כי ברגע שמתחילים לקבל כמה שדות לתאר, זה יכול להפוך למסורבל. במקום זאת, אתה יכול להשתמש ב'ממשק' או 'סוג' כדי לתאר את הprops של הרכיב:

<Sandpack>

```tsx src/App.tsx active
interface MyButtonProps {
  /** The text to display inside the button */
  title: string;
  /** Whether the button can be interacted with */
  disabled: boolean;
}

function MyButton({ title, disabled }: MyButtonProps) {
  return (
    <button disabled={disabled}>{title}</button>
  );
}

export default function MyApp() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton title="I'm a disabled button" disabled={true}/>
    </div>
  );
}
```

```js src/App.js hidden
import AppTSX from "./App.tsx";
export default App = AppTSX;
```

</Sandpack>

הסוג המתאר את הprops של הרכיב שלך יכול להיות פשוט או מורכב ככל שאתה צריך, אם כי הם צריכים להיות סוג אובייקט המתואר עם 'סוג' או 'ממשק'. אתה יכול ללמוד כיצד TypeScript מתאר אובייקטים ב-[Object Types](https://www.typescriptlang.org/docs/handbook/2/objects.html), אך ייתכן שתהיה מעוניין גם בשימוש ב-[Union Types](https://www.typescriptlang.org/docs/handbook/2/everyday-types.html#union-types) כדי לתאר סוג אחד של [סוגים שונים וסוגים שונים] סוגים](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html) מדריך למקרי שימוש מתקדמים יותר.


## ווים לדוגמה {/*example-hooks*/}

הגדרות הסוג מ- `@types/react` כוללות טיפוסים עבור ה-Hooks המובנים, כך שתוכל להשתמש בהם ברכיבים שלך ללא כל הגדרה נוספת. הם בנויים כדי לקחת בחשבון את הקוד שאתה כותב ברכיב שלך, כך שתקבל [טיפוסים משוערים](https://www.typescriptlang.org/docs/handbook/type-inference.html) הרבה מהזמן ובאופן אידיאלי לא תצטרך להתמודד עם הפרטים הקטנים של אספקת הסוגים. 

עם זאת, אנו יכולים להסתכל על כמה דוגמאות כיצד לספק סוגים עבור Hooks.

### `useState` {/*typing-usestate*/}

ה-[`useState` Hook](/reference/react/useState) יעשה שימוש חוזר בערך המועבר כstate ההתחלתי כדי לקבוע מה סוג הערך צריך להיות. לְדוּגמָה:

```ts
// Infer the type as "boolean"
const [enabled, setEnabled] = useState(false);
```

יקצה את הסוג 'בוליאני' ל-'enabled', ו-'setEnabled' תהיה פונקציה שמקבלת ארגומנט 'בוליאני', או פונקציה שמחזירה 'בוליאני'. אם אתה רוצה לספק במפורש סוג עבור הstate, אתה יכול לעשות זאת על ידי מתן ארגומנט סוג לקריאה 'useState':

```ts 
// Explicitly set the type to "boolean"
const [enabled, setEnabled] = useState<boolean>(false);
```

זה לא מאוד שימושי במקרה זה, אבל מקרה נפוץ שבו אולי תרצה לספק סוג הוא כאשר יש לך סוג איגוד. לדוגמה, 'סטטוס' כאן יכול להיות אחת מכמה מחרוזות שונות:

```ts
type Status = "idle" | "loading" | "success" | "error";

const [status, setStatus] = useState<Status>("idle");
```

לחלופין, כפי שהומלץ ב-[עקרונות לstate מבנה](/למד/בחירת-ה-מצב-מבנה#עקרונות-ל-מצב-מבנה), אתה יכול לקבץ מצב קשור כאובייקט ולתאר את האפשרויות השונות באמצעות סוגי אובייקטים:

```ts
type RequestState =
  | { status: 'idle' }
  | { status: 'loading' }
  | { status: 'success', data: any }
  | { status: 'error', error: Error };

const [requestState, setRequestState] = useState<RequestState>({ status: 'idle' });
```

### `useReducer` {/*הקלדה-usereducer*/}

ה-[`useReducer` Hook](/reference/react/useReducer) הוא Hook מורכב יותר שלוקח פונקציית reducer וstate התחלתי. הסוגים של פונקציית הreducer נגזרים מstate ההתחלתי. באפשרותך לספק ארגומנט סוג לקריאה 'useReducer' כדי לספק סוג עבור הstate, אך לרוב עדיף להגדיר את הסוג בstate ההתחלתי במקום זאת:

<Sandpack>

```tsx src/App.tsx active
import {useReducer} from 'react';

interface State {
   count: number 
};

type CounterAction =
  | { type: "reset" }
  | { type: "setCount"; value: State["count"] }

const initialState: State = { count: 0 };

function stateReducer(state: State, action: CounterAction): State {
  switch (action.type) {
    case "reset":
      return initialState;
    case "setCount":
      return { ...state, count: action.value };
    default:
      throw new Error("Unknown action");
  }
}

export default function App() {
  const [state, dispatch] = useReducer(stateReducer, initialState);

  const addFive = () => dispatch({ type: "setCount", value: state.count + 5 });
  const reset = () => dispatch({ type: "reset" });

  return (
    <div>
      <h1>Welcome to my counter</h1>

      <p>Count: {state.count}</p>
      <button onClick={addFive}>Add 5</button>
      <button onClick={reset}>Reset</button>
    </div>
  );
}

```

```js src/App.js hidden
import AppTSX from "./App.tsx";
export default App = AppTSX;
```

</Sandpack>


אנו משתמשים ב-TypeScript בכמה מקומות מרכזיים:

 - `מצב ממשק` מתאר את צורת מצב הreducer.
 - `סוג CounterAction` מתאר את הפעולות השונות שניתן לשלוח לreducer.
 - `const initialState: State` מספק סוג עבור הstate ההתחלתי, וגם את הסוג שבו משתמשים `useReducer` כברירת מחדל.
 - `stateReducer(state: State, action: CounterAction): State` מגדיר את סוגי הארגומנטים וערך ההחזרה של פונקציית הreducer.

חלופה מפורשת יותר להגדרת הסוג ב-'initialState' היא לספק ארגומנט סוג ל-'useReducer':

```ts
import { stateReducer, State } from './your-reducer-implementation';

const initialState = { count: 0 };

export default function App() {
  const [state, dispatch] = useReducer<State>(stateReducer, initialState);
}
```

### `useContext` {/*הקלדה-usecontext*/}

ה-[`useContext` Hook](/reference/react/useContext) היא טכניקה להעברת נתונים בעץ הרכיבים ללא צורך להעביר props דרך רכיבים. הוא משמש על ידי יצירת רכיב ספק ולעתים קרובות על ידי יצירת Hook כדי לצרוך את הערך ברכיב צאצא.

סוג הערך שסופק על ידי ההקשר מוסק מהערך המועבר לקריאה 'createContext':

<Sandpack>

```tsx src/App.tsx active
import { createContext, useContext, useState } from 'react';

type Theme = "light" | "dark" | "system";
const ThemeContext = createContext<Theme>("system");

const useGetTheme = () => useContext(ThemeContext);

export default function MyApp() {
  const [theme, setTheme] = useState<Theme>('light');

  return (
    <ThemeContext.Provider value={theme}>
      <MyComponent />
    </ThemeContext.Provider>
  )
}

function MyComponent() {
  const theme = useGetTheme();

  return (
    <div>
      <p>Current theme: {theme}</p>
    </div>
  )
}
```

```js src/App.js hidden
import AppTSX from "./App.tsx";
export default App = AppTSX;
```

</Sandpack>

הטכניקה הזו עובדת כשיש לך ערך ברירת מחדל שזה הגיוני - אבל לפעמים יש מקרים שבהם אין לך, ובמקרים האלה `null` יכול להרגיש הגיוני כערך ברירת מחדל. עם זאת, כדי לאפשר למערכת הטיפוסים להבין את הקוד שלך, עליך להגדיר במפורש `ContextShape | null' ב-'createContext'. 

זה גורם לבעיה שאתה צריך כדי לבטל את ה- `| null` בסוג עבור צרכני הקשר. ההמלצה שלנו היא לבקש מה-Hook לבצע בדיקת זמן ריצה לקיומו ולזרוק שגיאה כאשר אינו קיים:

```js {5, 16-20}
import { createContext, useContext, useState, useMemo } from 'react';

// This is a simpler example, but you can imagine a more complex object here
type ComplexObject = {
  kind: string
};

// The context is created with `| null` in the type, to accurately reflect the default value.
const Context = createContext<ComplexObject | null>(null);

// The `| null` will be removed via the check in the Hook.
const useGetComplexObject = () => {
  const object = useContext(Context);
  if (!object) { throw new Error("useGetComplexObject must be used within a Provider") }
  return object;
}

export default function MyApp() {
  const object = useMemo(() => ({ kind: "complex" }), []);

  return (
    <Context.Provider value={object}>
      <MyComponent />
    </Context.Provider>
  )
}

function MyComponent() {
  const object = useGetComplexObject();

  return (
    <div>
      <p>Current object: {object.kind}</p>
    </div>
  )
}
```

### `useMemo` {/*הקלדה-usememo*/}

ה-[`useMemo`](/reference/react/useMemo) Hooks ייצרו/ייגשו מחדש לערך שנשנן מקריאה לפונקציה, ויפעילו מחדש את הפונקציה רק ​​כאשר התלות שעברו כפרמטר השני משתנות. התוצאה של קריאת ה-Hook מוסקת מערך ההחזרה מהפונקציה בפרמטר הראשון. אתה יכול להיות מפורש יותר על ידי מתן ארגומנט סוג ל-Hook.

```ts
// The type of visibleTodos is inferred from the return value of filterTodos
const visibleTodos = useMemo(() => filterTodos(todos, tab), [todos, tab]);
```


### `useCallback` {/*הקלדה-usecallback*/}

ה-[`useCallback`](/reference/react/useCallback) מספק הפניה יציבה לפונקציה כל עוד התלות שהועברו לפרמטר השני זהות. כמו `useMemo`, סוג הפונקציה נגזר מערך ההחזרה של הפונקציה בפרמטר הראשון, ותוכל להיות מפורש יותר על ידי מתן ארגומנט type ל-Hook.


```ts
const handleClick = useCallback(() => {
  // ...
}, [todos]);
```

כשעובדים ב-TypeScript מצב קפדני `useCallback` דורש הוספת סוגים עבור הפרמטרים בהתקשרות חזרה. הסיבה לכך היא שסוג ההתקשרות חוזרת מוסק מערך ההחזרה של הפונקציה, וללא פרמטרים לא ניתן להבין את הסוג במלואו.

בהתאם להעדפות סגנון הקוד שלך, תוכל להשתמש בפונקציות `*EventHandler` מסוגי React כדי לספק את הסוג למטפל באירועים במקביל להגדרת ההתקשרות חזרה: 

```ts
import { useState, useCallback } from 'react';

export default function Form() {
  const [value, setValue] = useState("Change me");

  const handleChange = useCallback<React.ChangeEventHandler<HTMLInputElement>>((event) => {
    setValue(event.currentTarget.value);
  }, [setValue])
  
  return (
    <>
      <input value={value} onChange={handleChange} />
      <p>Value: {value}</p>
    </>
  );
}
```

## סוגים שימושיים {/*סוגים-שימושיים*/}

יש קבוצה די נרחבת של טיפוסים שמגיעים מחבילת `@types/react`, כדאי לקרוא כאשר אתה מרגיש בנוח עם האופן שבו React ו-TypeScript מתקשרים. אתה יכול למצוא אותם [בתיקיה של React ב-DefinitelyTyped](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/master/types/react/index.d.ts). נסקור כאן כמה מהסוגים הנפוצים יותר.

### אירועי DOM {/*טיינג-דום-אירועים*/}

כאשר עובדים עם אירועי DOM ב-React, לעתים קרובות ניתן להסיק מהמטפל באירוע את סוג האירוע. עם זאת, כאשר אתה רוצה לחלץ פונקציה שתועבר למטפל באירועים, תצטרך להגדיר במפורש את סוג האירוע.

<Sandpack>

```tsx src/App.tsx active
import { useState } from 'react';

export default function Form() {
  const [value, setValue] = useState("Change me");

  function handleChange(event: React.ChangeEvent<HTMLInputElement>) {
    setValue(event.currentTarget.value);
  }

  return (
    <>
      <input value={value} onChange={handleChange} />
      <p>Value: {value}</p>
    </>
  );
}
```

```js src/App.js hidden
import AppTSX from "./App.tsx";
export default App = AppTSX;
```

</Sandpack>

ישנם סוגים רבים של אירועים מסופקים בסוגי React - ניתן למצוא את הרשימה המלאה [כאן](https://github.com/DefinitelyTyped/DefinitelyTyped/blob/b580df54c0819ec9df62b0835a315dd48b8594a9/types/react.4C31/index) שמבוססת על [האירועים הפופולריים ביותר מה-DOM](https://developer.mozilla.org/en-US/docs/Web/Events).

בעת קביעת הסוג שאתה מחפש אתה יכול קודם כל להסתכל על מידע הרחף עבור מטפל האירוע שבו אתה משתמש, אשר יציג את סוג האירוע.

אם אתה צריך להשתמש באירוע שאינו כלול ברשימה זו, אתה יכול להשתמש בסוג `React.SyntheticEvent`, שהוא סוג הבסיס לכל האירועים.

### ילדים {/*הקלדה-ילדים*/}

ישנם שני נתיבים נפוצים לתיאור הילדים של רכיב. הראשון הוא להשתמש בסוג `React.ReactNode`, שהוא איחוד של כל הסוגים האפשריים שניתן להעביר כילדים ב-JSX:

```ts
interface ModalRendererProps {
  title: string;
  children: React.ReactNode;
}
```

זוהי הגדרה מאוד רחבה של ילדים. השני הוא להשתמש בסוג 'React.ReactElement', שהוא רק רכיבי JSX ולא פרימיטיביים של JavaScript כמו מחרוזות או מספרים:

```ts
interface ModalRendererProps {
  title: string;
  children: React.ReactElement;
}
```

שימו לב, אינכם יכולים להשתמש ב-TypeScript כדי לתאר שהילדים הם סוג מסוים של רכיבי JSX, ולכן אינכם יכולים להשתמש במערכת-type כדי לתאר רכיב שמקבל רק ילדים ``<li>``. 

אתה יכול לראות את כל הדוגמאות של 'React.ReactNode' וגם של 'React.ReactElement' עם בודק הסוג ב-[This TypeScript מגרש משחקים](https://www.typescriptlang.org/play?#code/JYWwDg9gTgLgBAJQKYEMDG8BmUIjgIilQ3wChSB 6CxYmAOmXRgDkIATJOdNJMGAZzgwAFpxAR+8YADswAVwGkZMJFEzpOjDKw4AFHGEEBvUnDhphwADZsi0gFw0mDWjqQBuUg F9yaCNMlENzgAXjgACjADfkctFnYkfQhDAEpQgD44AB42YAA3dKMo5P46C2tbJGkvLIpcgt9-QLi3AEEwMFCItJDMrPTTb IQ3dKywdIB5aU4kKyQQKpha8drhhIGzLLWODbNs3b3s8YAxKBQAcwXpAThMaGWDvbH0gFloGbmrgQfBzYpd1YjQZbEYARk B6zMwO2SHSAAlZlYIBCdtCRkZpHIrFYahQYQD8UYYFA5EhcfjyGYqHAXnJAsIUHlOOUbHYhMIIHJzsI0Qk4P9SLUBuRqXE XEwAKKfRZcNA8PiCfxWACEcAAUgBlAAacFm80W-CU11U6h4TgwUv11yShjgJjMLMqDnN9Dilq+nh8pD8AXgCHdMrCkWisV oAet0R6fXqhWKhjKllZVVxMcavpd4Zg7U6Qaj+2hmdG4zeRF10uu-Aeq0LBfLMEe-V+T2L7zLVu+FBWLdLeq+lc7DYFf39 deFVOotMCACNOCh1dq219a+30uC8YWoZsRyuEdjkevR8uvoVMdjyTWt4WiSSydXD4NqZP4AymeZE072ZzuUeZQKheQgA).

### props סגנון {/*הקלדה-סגנון-props*/}

בעת שימוש בסגנונות מוטבעים ב-React, אתה יכול להשתמש ב-'React.CSSProperties' כדי לתאר את האובייקט המועבר ל-'סגנון'. סוג זה הוא איחוד של כל מאפייני ה-CSS האפשריים, ומהווה דרך טובה להבטיח שאתה מעביר מאפייני CSS חוקיים ל-'סגנון', ולהשלמת השלמה אוטומטית בעורך שלך.

```ts
interface MyComponentProps {
  style: React.CSSProperties;
}
```

## למידה נוספת {/*למידה-נוספת*/}

מדריך זה כיסה את היסודות של השימוש ב-TypeScript עם React, אבל יש עוד הרבה מה ללמוד.
דפי API בודדים במסמכים עשויים להכיל תיעוד מעמיק יותר כיצד להשתמש בהם עם TypeScript.

אנו ממליצים על המשאבים הבאים:

 - [המדריך של TypeScript](https://www.typescriptlang.org/docs/handbook/) הוא התיעוד הרשמי של TypeScript, ומכסה את רוב תכונות השפה העיקריות.

 - [הערות השחרור של TypeScript](https://devblogs.microsoft.com/typescript/) מכסים כל תכונות חדשות לעומק.

 - [React TypeScript Cheatsheet](https://react-typescript-cheatsheet.netlify.app/) הוא דף צ'יטים המתוחזק על ידי קהילה לשימוש ב-TypeScript עם React, המכסה הרבה מקרי קצה שימושיים ומספק רוחב רחבה יותר מהמסמך הזה.

 - [TypeScript Community Discord](https://discord.com/invite/typescript) הוא מקום מצוין לשאול שאלות ולקבל עזרה עם בעיות TypeScript ו-React.

