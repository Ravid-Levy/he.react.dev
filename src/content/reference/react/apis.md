---
title: "React APIs מובנים"
---

<Intro>

בנוסף ל-[Hooks](/reference/react) ול-[Components](/reference/react/components), החבילה `react` מייצאת עוד כמה APIs שימושיים להגדרת קומפוננטות. העמוד הזה מפרט את שאר ה-APIs המודרניים של React.

</Intro>

---

* [`createContext`](/reference/react/createContext) מאפשר להגדיר ולהעביר context לקומפוננטות ילדים. בשימוש יחד עם [`useContext`.](/reference/react/useContext)
* [`forwardRef`](/reference/react/forwardRef) מאפשר לקומפוננטה לחשוף DOM node בתור ref לקומפוננטת הורה. בשימוש יחד עם [`useRef`.](/reference/react/useRef)
* [`lazy`](/reference/react/lazy) מאפשר לדחות את טעינת קוד הקומפוננטה עד לרינדור הראשון שלה.
* [`memo`](/reference/react/memo) מאפשר לקומפוננטה לדלג על רינדורים חוזרים עם אותם props. בשימוש יחד עם [`useMemo`](/reference/react/useMemo) ו-[`useCallback`.](/reference/react/useCallback)
* [`startTransition`](/reference/react/startTransition) מאפשר לסמן עדכון state כלא-דחוף. דומה ל-[`useTransition`.](/reference/react/useTransition)
