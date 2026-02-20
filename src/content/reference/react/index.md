---
title: סקירת Reference של React
---

<Intro>

החלק הזה מספק תיעוד reference מפורט לעבודה עם React. להיכרות עם React, עברו לחלק [Learn](/learn).

</Intro>

תיעוד ה-reference של React מחולק לתתי-חלקים פונקציונליים:

## React {/*react*/}

יכולות React ברמת API:

* [Hooks](/reference/react/hooks) - שימוש ביכולות שונות של React מתוך הקומפוננטות.
* [Components](/reference/react/components) - תיעוד הקומפוננטות המובנות שאפשר להשתמש בהן ב-JSX.
* [APIs](/reference/react/apis) - APIs שימושיים להגדרת קומפוננטות.
* [Directives](/reference/react/directives) - הנחיות ל-bundlers שתואמים ל-React Server Components.

## React DOM {/*react-dom*/}

`react-dom` כולל יכולות שנתמכות רק באפליקציות ווב (שרצות בסביבת DOM של הדפדפן). החלק הזה מחולק ל:

* [Hooks](/reference/react-dom/hooks) - Hooks לאפליקציות ווב שרצות בסביבת DOM של הדפדפן.
* [Components](/reference/react-dom/components) - React תומכת בכל רכיבי HTML ו-SVG המובנים בדפדפן.
* [APIs](/reference/react-dom) - החבילה `react-dom` כוללת מתודות שנתמכות רק באפליקציות ווב.
* [Client APIs](/reference/react-dom/client) - ה-APIs של `react-dom/client` מאפשרים לרנדר קומפוננטות React בצד לקוח (בדפדפן).
* [Server APIs](/reference/react-dom/server) - ה-APIs של `react-dom/server` מאפשרים לרנדר קומפוננטות React ל-HTML בצד שרת.

## Legacy APIs {/*legacy-apis*/}

* [Legacy APIs](/reference/react/legacy) - מיוצאים מחבילת `react`, אבל לא מומלצים לשימוש בקוד חדש.
