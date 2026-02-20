---
title: "React Labs: על מה עבדנו - פברואר 2024"
---

15 בפברואר 2024 מאת [Joseph Savona](https://twitter.com/en_JS), [Ricky Hanlon](https://twitter.com/rickhanlonii), [Andrew Clark](https://twitter.com/acdlite), [Matt Carroll](https://twitter.com/mattcarrollcode), and [Dan Abramov](https://twitter.com/dan_abramov).

---

<Intro>

בפוסטים של React Labs אנחנו כותבים על פרויקטים שנמצאים במחקר ופיתוח פעילים. התקדמנו משמעותית מאז [העדכון הקודם שלנו](/blog/2023/03/22/react-labs-what-we-have-been-working-on-march-2023), ורצינו לשתף בהתקדמות.

</Intro>

<Note>

React Conf 2024 מתוכנן ל-15-16 במאי ב-Henderson, Nevada! אם מעניין אתכם להגיע פיזית ל-React Conf, אפשר [להירשם להגרלת הכרטיסים](https://forms.reform.app/bLaLeE/react-conf-2024-ticket-lottery/1aRQLK) עד 28 בפברואר.

למידע נוסף על כרטיסים, סטרימינג בחינם, חסויות ועוד, ראו את [אתר React Conf](https://conf.react.dev).

</Note>

---

## React Compiler {/*react-compiler*/}

React Compiler כבר לא פרויקט מחקר: הקומפיילר מריץ עכשיו את instagram.com בפרודקשן, ואנחנו עובדים כדי לפרוס אותו למשטחים נוספים ב-Meta ולהכין את שחרור הקוד הפתוח הראשון.

כפי שדיברנו [בפוסט הקודם](/blog/2023/03/22/react-labs-what-we-have-been-working-on-march-2023#react-optimizing-compiler), React יכול *לפעמים* לבצע יותר מדי רינדורים מחדש כשמצב משתנה. מאז הימים הראשונים של React הפתרון שלנו למקרים כאלה היה memoization ידני. ב-APIs הנוכחיים שלנו זה אומר להשתמש ב-[`useMemo`](/reference/react/useMemo), [`useCallback`](/reference/react/useCallback), ו-[`memo`](/reference/react/memo) כדי לכוונן ידנית כמה React ירנדר מחדש בזמן שינויים במצב. אבל memoization ידני הוא פשרה: הוא מעמיס על הקוד, קל לטעות בו, ודורש עבודה נוספת כדי לשמור אותו מעודכן.

memoization ידני הוא פשרה סבירה, אבל לא היינו מרוצים. החזון שלנו הוא ש-React ירנדר מחדש *אוטומטית* רק את החלקים הנכונים ב-UI כשמצב משתנה, *בלי להתפשר על המודל המחשבתי המרכזי של React*. אנחנו מאמינים שהגישה של React, UI כפונקציה פשוטה של מצב עם ערכי JavaScript סטנדרטיים ואידיומים מוכרים, היא חלק מרכזי בסיבה לכך ש-React נגיש לכל כך הרבה מפתחים. לכן השקענו בבניית optimizing compiler עבור React.

JavaScript היא שפה מאתגרת במיוחד לאופטימיזציה בגלל החוקים הרופפים והאופי הדינמי שלה. React Compiler מסוגל לקמפל קוד בצורה בטוחה על ידי מודל של גם חוקי JavaScript *וגם* "החוקים של React". לדוגמה, קומפוננטות React חייבות להיות idempotent, כלומר להחזיר את אותו ערך עבור אותם קלטים, ואסור להן לשנות props או ערכי state. החוקים האלה מגבילים מה מפתחים יכולים לעשות, וגם יוצרים מרחב בטוח שבו הקומפיילר יכול לבצע אופטימיזציה.

כמובן, אנחנו מבינים שלפעמים מפתחים מכופפים את החוקים קצת, והמטרה שלנו היא ש-React Compiler יעבוד מהקופסה על כמה שיותר קוד. הקומפיילר מנסה לזהות מתי קוד לא עומד לגמרי בחוקי React, ואז או יקמפל אותו אם זה בטוח או ידלג על הקומפילציה אם זה לא בטוח. אנחנו בודקים מול בסיס הקוד הגדול והמגוון של Meta כדי לאמת את הגישה הזו.

למפתחים שרוצים לוודא שהקוד עומד בחוקי React, אנחנו ממליצים [להפעיל Strict Mode](/reference/react/StrictMode) ו-[להגדיר את תוסף ESLint של React](/learn/editor-setup#linting). הכלים האלה יכולים לתפוס באגים עדינים בקוד React, לשפר את איכות האפליקציות כבר היום, ולחזק אותן לעתיד מול יכולות עתידיות כמו React Compiler. אנחנו גם עובדים על תיעוד מאוחד של חוקי React ועל עדכונים לתוסף ESLint כדי לעזור לצוותים להבין וליישם את החוקים האלה ולבנות אפליקציות חזקות יותר.

כדי לראות את הקומפיילר בפעולה, אפשר לצפות ב-[הרצאה שלנו מהסתיו האחרון](https://www.youtube.com/watch?v=qOQClO3g8-Y). בזמן ההרצאה היו לנו נתונים ניסיוניים מוקדמים מניסיון React Compiler על עמוד אחד ב-instagram.com. מאז פרסמנו את הקומפיילר לפרודקשן בכל instagram.com. הרחבנו גם את הצוות כדי להאיץ את הפריסה למשטחים נוספים ב-Meta ובקוד פתוח. אנחנו נרגשים מהדרך קדימה ונשתף עוד בחודשים הקרובים.

## Actions {/*actions*/}

[שיתפנו בעבר](/blog/2023/03/22/react-labs-what-we-have-been-working-on-march-2023#react-server-components) שאנחנו בוחנים פתרונות להעברת נתונים מהלקוח לשרת עם Server Actions, כך שאפשר לבצע מוטציות במסד נתונים ולממש טפסים. במהלך הפיתוח של Server Actions הרחבנו את ה-APIs האלה כדי לתמוך גם בטיפול בנתונים באפליקציות לקוח בלבד.

לקבוצת היכולות הרחבה הזו אנחנו קוראים פשוט "Actions". Actions מאפשרים להעביר פונקציה לאלמנטים ב-DOM כמו [`<form/>`](/reference/react-dom/components/form):

```js
<form action={search}>
  <input name="query" />
  <button type="submit">Search</button>
</form>
```

הפונקציה `action` יכולה לפעול סינכרונית או אסינכרונית. אפשר להגדיר Actions בצד הלקוח עם JavaScript רגיל או בצד השרת עם ההנחיה [`'use server'`](/reference/react/use-server). כשמשתמשים ב-action, React מנהל עבורכם את מחזור החיים של שליחת הנתונים ומספק hooks כמו [`useFormStatus`](/reference/react-dom/hooks/useFormStatus) ו-[`useFormState`](/reference/react-dom/hooks/useFormState) כדי לגשת למצב הנוכחי ולתגובה של פעולת הטופס.

כברירת מחדל, Actions נשלחים בתוך [transition](/reference/react/useTransition), כך שהעמוד הנוכחי נשאר אינטראקטיבי בזמן שהפעולה מעובדת. מכיוון ש-Actions תומכים בפונקציות async, הוספנו גם אפשרות להשתמש ב-`async/await` בתוך transitions. זה מאפשר להציג pending UI עם מצב `isPending` של transition כשהבקשה האסינכרונית (כמו `fetch`) מתחילה, ולהמשיך להציג pending UI עד שהעדכון מוחל.

יחד עם Actions אנחנו מציגים יכולת בשם [`useOptimistic`](/reference/react/useOptimistic) לניהול optimistic state updates. עם hook זה אפשר להחיל עדכונים זמניים שחוזרים אוטומטית אחורה ברגע שהמצב הסופי נכתב. עבור Actions, זה מאפשר לקבוע בצורה אופטימית את מצב הנתונים הסופי בצד הלקוח בהנחה שהשליחה הצליחה, ולחזור לערך שהתקבל מהשרת אם צריך. זה עובד עם `async`/`await` רגיל, ולכן מתנהג אותו דבר בין אם משתמשים ב-`fetch` בצד הלקוח או ב-Server Action בצד השרת.

מחברי ספריות יכולים לממש props מותאמים מסוג `action={fn}` בקומפוננטות שלהם עם `useTransition`. הכוונה שלנו היא שספריות יאמצו את דפוס Actions בעת תכנון APIs של קומפוננטות, כדי לספק חוויה עקבית למפתחי React. לדוגמה, אם הספרייה שלכם מספקת קומפוננטה `<Calendar onSelect={eventHandler}>`, שקלו לחשוף גם API של `<Calendar selectAction={action}>`.

למרות שבהתחלה התמקדנו ב-Server Actions להעברת נתונים לקוח-שרת, הפילוסופיה שלנו ב-React היא לספק את אותו מודל תכנות בכל הפלטפורמות והסביבות. כשאפשר, אם אנחנו מציגים יכולת בצד הלקוח, אנחנו שואפים שתעבוד גם בשרת ולהפך. הפילוסופיה הזו מאפשרת ליצור סט APIs אחיד שעובד לא משנה איפה האפליקציה רצה, ומקלה על שדרוג לסביבות שונות בעתיד.

Actions זמינים עכשיו בערוץ Canary ויישלחו בגרסת React הבאה.

## יכולות חדשות ב-React Canary {/*new-features-in-react-canary*/}

הצגנו את [React Canaries](/blog/2023/05/03/react-canaries) כאפשרות לאמץ יכולות יציבות חדשות בודדות ברגע שהעיצוב שלהן קרוב לסופי, עוד לפני שהן משתחררות בגרסת semver יציבה.

Canaries הם שינוי באופן שבו אנחנו מפתחים את React. בעבר יכולות נחקרו ונבנו פנימית בתוך Meta, כך שמשתמשים ראו רק את התוצר המלוטש הסופי כשהוא שוחרר ל-Stable. עם Canaries אנחנו בונים בפומבי בעזרת הקהילה כדי לסגור סופית יכולות שאנחנו משתפים בסדרת פוסטי React Labs. זה אומר שאתם שומעים על יכולות חדשות מוקדם יותר, בזמן שהן נסגרות ולא רק אחרי שהכול הושלם.

React Server Components, Asset Loading, Document Metadata ו-Actions כבר נכנסו ל-React Canary, והוספנו תיעוד ליכולות האלה ב-react.dev:

- **Directives**: [`"use client"`](/reference/react/use-client) ו-[`"use server"`](/reference/react/use-server) הן יכולות bundler שמיועדות ל-full-stack React frameworks. הן מסמנות את "נקודות הפיצול" בין שתי הסביבות: `"use client"` מנחה את הבאנדלר לייצר תגית `<script>` (בדומה ל-[Astro Islands](https://docs.astro.build/en/concepts/islands/#creating-an-island)), ו-`"use server"` אומר לבאנדלר לייצר endpoint של POST (בדומה ל-[tRPC Mutations](https://trpc.io/docs/concepts)). יחד הן מאפשרות לכתוב קומפוננטות לשימוש חוזר שמשלבות אינטראקטיביות בצד לקוח עם הלוגיקה המתאימה בצד שרת.

- **Document Metadata**: הוספנו תמיכה מובנית ברינדור תגיות [`<title>`](/reference/react-dom/components/title), [`<meta>`](/reference/react-dom/components/meta), ו-[`<link>`](/reference/react-dom/components/link) של metadata בכל מקום בעץ הקומפוננטות. הן עובדות אותו דבר בכל סביבה, כולל קוד לקוח מלא, SSR ו-RSC. כך מתקבלת תמיכה מובנית ביכולות שספריות כמו [React Helmet](https://github.com/nfl/react-helmet) הובילו בעבר.

- **Asset Loading**: שילבנו Suspense במחזור החיים של טעינת משאבים כמו stylesheets, פונטים וסקריפטים כך ש-React לוקח אותם בחשבון כדי לקבוע אם תוכן באלמנטים כמו [`<style>`](/reference/react-dom/components/style), [`<link>`](/reference/react-dom/components/link), ו-[`<script>`](/reference/react-dom/components/script) מוכן להצגה. הוספנו גם [Resource Loading APIs](/reference/react-dom#resource-preloading-apis) חדשים כמו `preload` ו-`preinit` כדי לאפשר שליטה טובה יותר מתי משאב צריך להיטען ולעבור אתחול.

- **Actions**: כפי ששיתפנו למעלה, הוספנו Actions לניהול שליחת נתונים מהלקוח לשרת. אפשר להוסיף `action` לאלמנטים כמו [`<form/>`](/reference/react-dom/components/form), לגשת לסטטוס עם [`useFormStatus`](/reference/react-dom/hooks/useFormStatus), לטפל בתוצאה עם [`useFormState`](/reference/react-dom/hooks/useFormState), ולעדכן את ה-UI באופן אופטימי עם [`useOptimistic`](/reference/react/useOptimistic).

מכיוון שכל היכולות האלה עובדות יחד, קשה לשחרר אותן בנפרד בערוץ Stable. שחרור Actions בלי ה-hooks המשלימים לגישה למצבי טופס היה מגביל את השימוש המעשי ב-Actions. הכנסת React Server Components בלי אינטגרציה של Server Actions הייתה מסבכת עדכון נתונים בשרת.

לפני שאפשר לשחרר סט יכולות לערוץ Stable, צריך לוודא שהן עובדות יחד בצורה קוהרנטית ושיש למפתחים את כל מה שצריך כדי להשתמש בהן בפרודקשן. React Canaries מאפשרים לנו לפתח את היכולות האלה בנפרד ולשחרר APIs יציבים בהדרגה עד שכל סט היכולות שלם.

סט היכולות הנוכחי ב-React Canary שלם ומוכן לשחרור.

## גרסת ה-major הבאה של React {/*the-next-major-version-of-react*/}

אחרי כמה שנים של איטרציה, `react@canary` מוכן עכשיו להישלח ל-`react@latest`. היכולות החדשות שצוינו למעלה תואמות לכל סביבה שבה האפליקציה שלכם רצה ומספקות את כל מה שנדרש לשימוש בפרודקשן. מכיוון ש-Asset Loading ו-Document Metadata עלולים להיות שינוי שובר עבור חלק מהאפליקציות, הגרסה הבאה של React תהיה major version: **React 19**.

עדיין נותרה עבודה לקראת השחרור. ב-React 19 אנחנו מוסיפים גם שיפורים מבוקשים שדורשים שינויים שוברים, כמו תמיכה ב-Web Components. הפוקוס שלנו עכשיו הוא להכניס את השינויים האלה, להכין את השחרור, לסיים את התיעוד ליכולות החדשות ולפרסם הכרזות על כל מה שנכלל.

נשתף בחודשים הקרובים עוד מידע על כל מה ש-React 19 כולל, איך לאמץ את יכולות הלקוח החדשות, ואיך לבנות תמיכה ב-React Server Components.

## Offscreen (שמו שונה ל-Activity) {/*offscreen-renamed-to-activity*/}

מאז העדכון הקודם שינינו שם ליכולת שאנחנו חוקרים מ-"Offscreen" ל-"Activity". השם "Offscreen" רמז שזה חל רק על חלקים באפליקציה שלא נראים, אבל תוך כדי מחקר הבנו שיכולים להיות חלקים גלויים אך לא פעילים, כמו תוכן שנמצא מאחורי modal. השם החדש משקף טוב יותר את ההתנהגות של סימון חלקים מסוימים באפליקציה כ-"active" או "inactive".

Activity עדיין במחקר, והעבודה שנותרה היא לסגור סופית את ה-primitives שנחשפים למפתחי ספריות. הורדנו עדיפות זמנית לאזור הזה בזמן שאנחנו מתמקדים בשחרור יכולות בשלות יותר.

* * *

בנוסף לעדכון הזה, הצוות שלנו הרצה בכנסים והתארח בפודקאסטים כדי לדבר יותר על העבודה שלנו ולענות על שאלות.

- [Sathya Gunasekaran](/community/team#sathya-gunasekaran) דיבר על React Compiler בכנס [React India](https://www.youtube.com/watch?v=kjOacmVsLSE)

- [Dan Abramov](/community/team#dan-abramov) נתן הרצאה ב-[RemixConf](https://www.youtube.com/watch?v=zMf_xeGPn6s) בשם "React from Another Dimension" שחוקרת היסטוריה חלופית של יצירת React Server Components ו-Actions

- [Dan Abramov](/community/team#dan-abramov) התראיין ב-[פודקאסט JS Party של Changelog](https://changelog.com/jsparty/311) על React Server Components

- [Matt Carroll](/community/team#matt-carroll) התראיין ב-[פודקאסט Front-End Fire](https://www.buzzsprout.com/2226499/14462424-interview-the-two-reacts-with-rachel-nabors-evan-bacon-and-matt-carroll) ושם דיבר על [The Two Reacts](https://overreacted.io/the-two-reacts/)

תודה ל-[Lauren Tan](https://twitter.com/potetotes), [Sophie Alpert](https://twitter.com/sophiebits), [Jason Bonta](https://threads.net/someextent), [Eli White](https://twitter.com/Eli_White), and [Sathya Gunasekaran](https://twitter.com/_gsathya) על סקירת הפוסט הזה.

תודה שקראתם, ו-[נתראה ב-React Conf](https://conf.react.dev/)!
