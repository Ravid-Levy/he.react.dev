---
title: createContext
---

<Intro>

`createContext` מאפשרת ליצור [context](/learn/passing-data-deeply-with-context) שקומפוננטות יכולות לספק או לקרוא.

```js
const SomeContext = createContext(defaultValue)
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `createContext(defaultValue)` {/*createcontext*/}

קראו ל-`createContext` מחוץ לכל קומפוננטה כדי ליצור context.

```js
import { createContext } from 'react';

const ThemeContext = createContext('light');
```

[ראו דוגמאות נוספות בהמשך.](#usage)

#### Parameters {/*parameters*/}

* `defaultValue`: הערך שתרצו של-context יהיה כשאין context provider תואם בעץ מעל הקומפוננטה שקוראת את ה-context. אם אין לכם ערך ברירת מחדל משמעותי, ציינו `null`. ערך ברירת המחדל מיועד כ-fallback של "מוצא אחרון". הוא סטטי ולעולם לא משתנה לאורך הזמן.

#### Returns {/*returns*/}

`createContext` מחזירה אובייקט context.

**אובייקט ה-context עצמו לא מחזיק מידע.** הוא מייצג *איזה* context קומפוננטות אחרות קוראות או מספקות. בדרך כלל תשתמשו ב-[`SomeContext.Provider`](#provider) בקומפוננטות למעלה כדי לציין את ערך ה-context, ותקראו ל-[`useContext(SomeContext)`](/reference/react/useContext) בקומפוננטות למטה כדי לקרוא אותו. לאובייקט ה-context יש כמה מאפיינים:

* `SomeContext.Provider` מאפשר לספק את ערך ה-context לקומפוננטות.
* `SomeContext.Consumer` הוא דרך חלופית ונדירה לקרוא את ערך ה-context.

---

### `SomeContext.Provider` {/*provider*/}

עטפו את הקומפוננטות שלכם ב-context provider כדי לציין את ערך ה-context הזה לכל הקומפוננטות בתוכו:

```js
function App() {
  const [theme, setTheme] = useState('light');
  // ...
  return (
    <ThemeContext.Provider value={theme}>
      <Page />
    </ThemeContext.Provider>
  );
}
```

#### Props {/*provider-props*/}

* `value`: הערך שברצונכם להעביר לכל הקומפוננטות שקוראות את ה-context הזה בתוך ה-provider הזה, לא משנה כמה עמוק. ערך ה-context יכול להיות מכל סוג. קומפוננטה שקוראת ל-[`useContext(SomeContext)`](/reference/react/useContext) בתוך ה-provider תקבל את ה-`value` של ה-context provider התואם הפנימי ביותר שמעליה.

---

### `SomeContext.Consumer` {/*consumer*/}

לפני ש-`useContext` הייתה קיימת, הייתה דרך ישנה יותר לקרוא context:

```js
function Button() {
  // 🟡 Legacy way (not recommended)
  return (
    <ThemeContext.Consumer>
      {theme => (
        <button className={theme} />
      )}
    </ThemeContext.Consumer>
  );
}
```

למרות שהדרך הישנה הזו עדיין עובדת, **קוד חדש צריך לקרוא context בעזרת [`useContext()`](/reference/react/useContext) במקום:**

```js
function Button() {
  // ✅ Recommended way
  const theme = useContext(ThemeContext);
  return <button className={theme} />;
}
```

#### Props {/*consumer-props*/}

* `children`: פונקציה. React תקרא לפונקציה שתעבירו עם ערך ה-context הנוכחי שנקבע על ידי אותו אלגוריתם שבו משתמשת [`useContext()`](/reference/react/useContext), ותרנדר את התוצאה שתחזירו מהפונקציה הזו. React גם תריץ את הפונקציה הזו שוב ותעדכן את ה-UI בכל פעם שה-context מהקומפוננטות ההורה משתנה.

---

## שימוש {/*usage*/}

### יצירת context {/*creating-context*/}

Context מאפשר לקומפוננטות [להעביר מידע עמוק יותר בעץ](/learn/passing-data-deeply-with-context) בלי להעביר props במפורש.

קראו ל-`createContext` מחוץ לכל קומפוננטה כדי ליצור context אחד או יותר.

```js [[1, 3, "ThemeContext"], [1, 4, "AuthContext"], [3, 3, "'light'"], [3, 4, "null"]]
import { createContext } from 'react';

const ThemeContext = createContext('light');
const AuthContext = createContext(null);
```

`createContext` מחזירה <CodeStep step={1}>אובייקט context</CodeStep>. קומפוננטות יכולות לקרוא context על ידי העברתו ל-[`useContext()`](/reference/react/useContext):

```js [[1, 2, "ThemeContext"], [1, 7, "AuthContext"]]
function Button() {
  const theme = useContext(ThemeContext);
  // ...
}

function Profile() {
  const currentUser = useContext(AuthContext);
  // ...
}
```

כברירת מחדל, הערכים שהן יקבלו יהיו <CodeStep step={3}>ערכי ברירת המחדל</CodeStep> שציינתם בעת יצירת ה-context. אבל בפני עצמו זה לא שימושי, כי ערכי ברירת המחדל לעולם לא משתנים.

Context שימושי כי אפשר **לספק ערכים אחרים, דינמיים, מתוך הקומפוננטות שלכם:**

```js {8-9,11-12}
function App() {
  const [theme, setTheme] = useState('dark');
  const [currentUser, setCurrentUser] = useState({ name: 'Taylor' });

  // ...

  return (
    <ThemeContext.Provider value={theme}>
      <AuthContext.Provider value={currentUser}>
        <Page />
      </AuthContext.Provider>
    </ThemeContext.Provider>
  );
}
```

עכשיו קומפוננטת `Page` וכל קומפוננטה בתוכה, לא משנה כמה עמוק, "יראו" את ערכי ה-context שהועברו. אם ערכי ה-context משתנים, React תרנדר מחדש גם את הקומפוננטות שקוראות את ה-context.

[קראו עוד על קריאה וסיפוק context וראו דוגמאות.](/reference/react/useContext)

---

### ייבוא וייצוא context מקובץ {/*importing-and-exporting-context-from-a-file*/}

לעיתים קרובות קומפוננטות בקבצים שונים צריכות גישה לאותו context. לכן מקובל להצהיר על contexts בקובץ נפרד. לאחר מכן אפשר להשתמש ב-[`export` statement](https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/export) כדי להפוך את ה-context לזמין לקבצים אחרים:

```js {4-5}
// Contexts.js
import { createContext } from 'react';

export const ThemeContext = createContext('light');
export const AuthContext = createContext(null);
```

קומפוננטות שמוצהרות בקבצים אחרים יכולות להשתמש ב-[`import`](https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/import) כדי לקרוא או לספק את ה-context הזה:

```js {2}
// Button.js
import { ThemeContext } from './Contexts.js';

function Button() {
  const theme = useContext(ThemeContext);
  // ...
}
```

```js {2}
// App.js
import { ThemeContext, AuthContext } from './Contexts.js';

function App() {
  // ...
  return (
    <ThemeContext.Provider value={theme}>
      <AuthContext.Provider value={currentUser}>
        <Page />
      </AuthContext.Provider>
    </ThemeContext.Provider>
  );
}
```

זה עובד בדומה ל-[ייבוא וייצוא קומפוננטות.](/learn/importing-and-exporting-components)

---

## פתרון תקלות {/*troubleshooting*/}

### אני לא מוצא דרך לשנות את ערך ה-context {/*i-cant-find-a-way-to-change-the-context-value*/}


קוד כזה מציין את ערך ה-context *ברירת המחדל*:

```js
const ThemeContext = createContext('light');
```

הערך הזה לעולם לא משתנה. React משתמשת בו רק כ-fallback אם היא לא מוצאת provider תואם מעל.

כדי לגרום ל-context להשתנות לאורך הזמן, [הוסיפו state ועטפו קומפוננטות ב-context provider.](/reference/react/useContext#updating-data-passed-via-context)
