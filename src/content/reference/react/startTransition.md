---
title: startTransition
---

<Intro>

`startTransition` מאפשר לעדכן state בלי לחסום את ה-UI.

```js
startTransition(scope)
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `startTransition(scope)` {/*starttransitionscope*/}

הפונקציה `startTransition` מאפשרת לסמן עדכון state כ-transition.

```js {7,9}
import { startTransition } from 'react';

function TabContainer() {
  const [tab, setTab] = useState('about');

  function selectTab(nextTab) {
    startTransition(() => {
      setTab(nextTab);
    });
  }
  // ...
}
```

[ראו דוגמאות נוספות בהמשך.](#usage)

#### Parameters {/*parameters*/}

* `scope`: פונקציה שמעדכנת state על ידי קריאה לפונקציית `set` אחת או יותר.[`set` functions.](/reference/react/useState#setstate) React קוראת ל-`scope` מיד ללא ארגומנטים, ומסמנת כל עדכון state שמתוזמן באופן סינכרוני במהלך הקריאה ל-`scope` כ-transition. העדכונים יהיו [לא-חוסמים](/reference/react/useTransition#marking-a-state-update-as-a-non-blocking-transition) ו-[לא יציגו מחווני טעינה לא רצויים.](/reference/react/useTransition#preventing-unwanted-loading-indicators)

#### Returns {/*returns*/}

`startTransition` לא מחזירה דבר.

#### Caveats {/*caveats*/}

* `startTransition` לא מספקת דרך לעקוב אם transition ממתין. כדי להציג אינדיקציית pending בזמן שה-transition מתבצע, צריך להשתמש ב-[`useTransition`](/reference/react/useTransition).

* אפשר לעטוף עדכון בתוך transition רק אם יש לכם גישה לפונקציית ה-`set` של אותו state. אם רוצים להתחיל transition בתגובה ל-prop או לערך שמוחזר מ-custom Hook, נסו להשתמש ב-[`useDeferredValue`](/reference/react/useDeferredValue).

* הפונקציה שמעבירים ל-`startTransition` חייבת להיות סינכרונית. React מבצעת אותה מיד ומסמנת כ-transition את כל עדכוני ה-state שמתרחשים בזמן הביצוע. אם תנסו לבצע עדכוני state נוספים מאוחר יותר (למשל ב-timeout), הם לא יסומנו כ-transition.

* עדכון state שסומן כ-transition יופרע על ידי עדכוני state אחרים. לדוגמה, אם מעדכנים קומפוננטת גרף בתוך transition ואז מתחילים להקליד בשדה קלט בזמן שהגרף באמצע רינדור מחדש, React תתחיל מחדש את עבודת הרינדור על הגרף אחרי טיפול בעדכון ה-state של הקלט.

* אי אפשר להשתמש בעדכוני transition כדי לשלוט בשדות קלט טקסט.

* אם יש כמה transitions בו-זמנית, React כרגע מאגדת אותם יחד. זו מגבלה שככל הנראה תוסר בגרסה עתידית.

---

## שימוש {/*usage*/}

### סימון עדכון state כ-transition לא חוסם {/*marking-a-state-update-as-a-non-blocking-transition*/}

אפשר לסמן עדכון state כ-*transition* על ידי עטיפה שלו בקריאה ל-`startTransition`:

```js {7,9}
import { startTransition } from 'react';

function TabContainer() {
  const [tab, setTab] = useState('about');

  function selectTab(nextTab) {
    startTransition(() => {
      setTab(nextTab);
    });
  }
  // ...
}
```

Transitions מאפשרים לשמור על תגובתיות עדכוני ממשק המשתמש גם במכשירים איטיים.

עם transition, ה-UI נשאר תגובתי גם בזמן רינדור מחדש. למשל, אם משתמש לוחץ על טאב ואז משנה את דעתו ולוחץ על טאב אחר, הוא יכול לעשות זאת בלי להמתין שהרינדור מחדש הראשון יסתיים.

<Note>

`startTransition` מאוד דומה ל-[`useTransition`](/reference/react/useTransition), חוץ מזה שהיא לא מספקת את הדגל `isPending` למעקב אחרי transition שמתבצע. אפשר לקרוא ל-`startTransition` כש-`useTransition` לא זמין. לדוגמה, `startTransition` עובדת מחוץ לקומפוננטות, כמו מתוך ספריית נתונים.

[למדו על transitions וראו דוגמאות בעמוד של `useTransition`.](/reference/react/useTransition)

</Note>
