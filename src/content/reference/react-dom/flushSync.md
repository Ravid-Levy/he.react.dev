---
title: flushSync
---

<Pitfall>

שימוש ב-`flushSync` אינו נפוץ ועלול לפגוע בביצועים של האפליקציה.

</Pitfall>

<Intro>

`flushSync` מאפשרת לאלץ את React לבצע flush לכל עדכון בתוך callback שסופק באופן סינכרוני. כך מובטח שה-DOM מתעדכן מיידית.

```js
flushSync(callback)
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `flushSync(callback)` {/*flushsync*/}

קראו ל-`flushSync` כדי לאלץ את React לבצע flush לעבודה ממתינה ולעדכן את ה-DOM באופן סינכרוני.

```js
import { flushSync } from 'react-dom';

flushSync(() => {
  setSomething(123);
});
```

ברוב המקרים אפשר להימנע מ-`flushSync`. השתמשו בה כמוצא אחרון.

[ראו דוגמאות נוספות בהמשך.](#usage)

#### Parameters {/*parameters*/}


* `callback`: פונקציה. React תקרא ל-callback הזה מיד ותבצע flush סינכרוני לכל עדכון שהוא מכיל. ייתכן שיבוצע flush גם לעדכונים ממתינים, ל-Effects, או לעדכונים מתוך Effects. אם עדכון מבצע suspend כתוצאה מקריאת `flushSync`, ייתכן שה-fallbacks יוצגו שוב.

#### Returns {/*returns*/}

`flushSync` מחזירה `undefined`.

#### Caveats {/*caveats*/}

* `flushSync` יכולה לפגוע משמעותית בביצועים. להשתמש במשורה.
* `flushSync` עשויה לאלץ גבולות Suspense ממתינים להציג את מצב ה-`fallback` שלהם.
* `flushSync` עשויה להריץ effects ממתינים ולהחיל באופן סינכרוני כל עדכון שהם מכילים לפני החזרה.
* `flushSync` עשויה לבצע flush לעדכונים מחוץ ל-callback כשנדרש כדי לבצע flush לעדכונים שבתוך ה-callback. לדוגמה, אם יש עדכונים ממתינים מקליק, React עשויה לבצע להם flush לפני ה-flush לעדכונים שבתוך ה-callback.

---

## שימוש {/*usage*/}

### ביצוע flush לעדכונים עבור אינטגרציות צד שלישי {/*flushing-updates-for-third-party-integrations*/}

בעת אינטגרציה עם קוד צד שלישי כמו APIs של דפדפן או ספריות UI, לעיתים נדרש לאלץ את React לבצע flush לעדכונים. השתמשו ב-`flushSync` כדי לאלץ את React לבצע flush סינכרוני לכל <CodeStep step={1}>עדכון state</CodeStep> בתוך ה-callback:

```js [[1, 2, "setSomething(123)"]]
flushSync(() => {
  setSomething(123);
});
// By this line, the DOM is updated.
```

כך מובטח שעד שהשורה הבאה בקוד רצה, React כבר עדכנה את ה-DOM.

**שימוש ב-`flushSync` אינו נפוץ, ושימוש תכוף בה עלול לפגוע משמעותית בביצועים של האפליקציה.** אם האפליקציה שלכם משתמשת רק ב-React APIs, ולא מבצעת אינטגרציה עם ספריות צד שלישי, לרוב `flushSync` לא נדרשת.

עם זאת, היא יכולה להיות שימושית לאינטגרציה עם קוד צד שלישי כמו APIs של דפדפן.

חלק מ-APIs של דפדפן מצפים שתוצאות בתוך callbacks ייכתבו ל-DOM בצורה סינכרונית עד סוף ה-callback, כדי שהדפדפן יוכל לפעול על ה-DOM המרונדר. ברוב המקרים React מטפלת בזה אוטומטית. אבל במקרים מסוימים ייתכן שיהיה צורך לאלץ עדכון סינכרוני.

לדוגמה, ה-API של הדפדפן `onbeforeprint` מאפשר לשנות את העמוד רגע לפני שנפתח חלון ההדפסה. זה שימושי ליישום סגנונות הדפסה מותאמים אישית שמשפרים את תצוגת המסמך להדפסה. בדוגמה הבאה משתמשים ב-`flushSync` בתוך ה-callback של `onbeforeprint` כדי לבצע "flush" מיידי של state של React ל-DOM. כך, בזמן שחלון ההדפסה נפתח, `isPrinting` מוצג כ-"yes":

<Sandpack>

```js src/App.js active
import { useState, useEffect } from 'react';
import { flushSync } from 'react-dom';

export default function PrintApp() {
  const [isPrinting, setIsPrinting] = useState(false);

  useEffect(() => {
    function handleBeforePrint() {
      flushSync(() => {
        setIsPrinting(true);
      })
    }

    function handleAfterPrint() {
      setIsPrinting(false);
    }

    window.addEventListener('beforeprint', handleBeforePrint);
    window.addEventListener('afterprint', handleAfterPrint);
    return () => {
      window.removeEventListener('beforeprint', handleBeforePrint);
      window.removeEventListener('afterprint', handleAfterPrint);
    }
  }, []);

  return (
    <>
      <h1>isPrinting: {isPrinting ? 'yes' : 'no'}</h1>
      <button onClick={() => window.print()}>
        Print
      </button>
    </>
  );
}
```

</Sandpack>

בלי `flushSync`, חלון ההדפסה יציג `isPrinting` כ-"no". הסיבה היא ש-React מאגדת עדכונים באופן אסינכרוני וחלון ההדפסה מוצג לפני שה-state מתעדכן.

<Pitfall>

`flushSync` יכולה לפגוע משמעותית בביצועים, ועלולה באופן בלתי צפוי לאלץ גבולות Suspense ממתינים להציג את מצב ה-fallback שלהם.

ברוב הזמן אפשר להימנע מ-`flushSync`, לכן השתמשו בה כמוצא אחרון.

</Pitfall>
