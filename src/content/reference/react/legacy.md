---
title: "Legacy React APIs"
---

<Intro>

ה-APIs האלה מיוצאים מחבילת `react`, אבל לא מומלצים לשימוש בקוד חדש. ראו את עמודי ה-API המקושרים עבור חלופות מומלצות.

</Intro>

---

## Legacy APIs {/*legacy-apis*/}

* [`Children`](/reference/react/Children) מאפשר לבצע מניפולציה וטרנספורמציה ל-JSX שהתקבל דרך prop בשם `children`. [ראו חלופות.](/reference/react/Children#alternatives)
* [`cloneElement`](/reference/react/cloneElement) מאפשר ליצור React element מנקודת התחלה של element אחר. [ראו חלופות.](/reference/react/cloneElement#alternatives)
* [`Component`](/reference/react/Component) מאפשר להגדיר קומפוננטת React כ-class ב-JavaScript. [ראו חלופות.](/reference/react/Component#alternatives)
* [`createElement`](/reference/react/createElement) מאפשר ליצור React element. בדרך כלל תשתמשו ב-JSX במקום.
* [`createRef`](/reference/react/createRef) יוצר אובייקט ref שיכול להכיל כל ערך. [ראו חלופות.](/reference/react/createRef#alternatives)
* [`isValidElement`](/reference/react/isValidElement) בודק האם ערך הוא React element. בדרך כלל בשימוש עם [`cloneElement`.](/reference/react/cloneElement)
* [`PureComponent`](/reference/react/PureComponent) דומה ל-[`Component`,](/reference/react/Component) אבל מדלג על רינדורים חוזרים עם אותם props. [ראו חלופות.](/reference/react/PureComponent#alternatives)


---

## APIs שהוצאו משימוש {/*deprecated-apis*/}

<Deprecated>

ה-APIs האלה יוסרו בגרסה ראשית עתידית של React.

</Deprecated>

* [`createFactory`](/reference/react/createFactory) מאפשר ליצור פונקציה שמייצרת React elements מסוג מסוים.
