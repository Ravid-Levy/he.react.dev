---
title: "React Labs: על מה עבדנו - מרץ 2023"
---

22 במרץ 2023 מאת [Joseph Savona](https://twitter.com/en_JS), [Josh Story](https://twitter.com/joshcstory), [Lauren Tan](https://twitter.com/potetotes), [Mengdi Chen](https://twitter.com/mengdi_en), [Samuel Susla](https://twitter.com/SamuelSusla), [Sathya Gunasekaran](https://twitter.com/_gsathya), [Sebastian Markbåge](https://twitter.com/sebmarkbage), and [Andrew Clark](https://twitter.com/acdlite)

---

<Intro>

בפוסטים של React Labs אנחנו כותבים על פרויקטים במחקר ופיתוח פעילים. מאז [העדכון הקודם](/blog/2022/06/15/react-labs-what-we-have-been-working-on-june-2022) הייתה התקדמות משמעותית, ורצינו לשתף מה למדנו.

</Intro>

---

## React Server Components {/*react-server-components*/}

React Server Components (או RSC) היא ארכיטקטורת אפליקציה חדשה שתוכננה על ידי צוות React.

שיתפנו לראשונה את המחקר על RSC ב-[הרצאת מבוא](/blog/2020/12/21/data-fetching-with-react-server-components) וב-[RFC](https://github.com/reactjs/rfcs/pull/188). בקצרה: אנחנו מציגים סוג חדש של קומפוננטות, Server Components, שרצות מראש ולא נכללות בחבילת ה-JavaScript של הלקוח. הן יכולות לרוץ בזמן build ולקרוא ממערכת הקבצים או לטעון תוכן סטטי. הן יכולות גם לרוץ בשרת, כך שאפשר לגשת לשכבת הנתונים בלי לבנות API נפרד. נתונים עוברים דרך props מ-Server Components ל-Client Components אינטראקטיביות בדפדפן.

RSC משלבת את המודל הפשוט של "request/response" מאפליקציות Multi-Page ממוקדות-שרת, עם האינטראקטיביות החלקה של Single-Page Apps ממוקדות-לקוח, כדי לקבל את הטוב משני העולמות.

מאז העדכון האחרון מיזגנו את [React Server Components RFC](https://github.com/reactjs/rfcs/blob/main/text/0188-server-components.md) כדי לאשרר את ההצעה. פתרנו נושאים פתוחים סביב [React Server Module Conventions](https://github.com/reactjs/rfcs/blob/main/text/0227-server-module-conventions.md), והגענו להסכמה עם השותפים לאמץ את קונבנציית `"use client"`. המסמכים האלה משמשים גם כמפרט לתמיכה שנדרשת ממימוש תואם-RSC.

השינוי הגדול ביותר הוא אימוץ [`async` / `await`](https://github.com/reactjs/rfcs/pull/229) כדרך הראשית לטעינת נתונים מתוך Server Components. בנוסף, אנחנו מתכננים לתמוך בטעינת נתונים מצד הלקוח דרך Hook חדש בשם `use` שפורס Promises. אמנם לא ניתן לתמוך ב-`async / await` בקומפוננטות שרירותיות באפליקציות client-only, אבל אנחנו מתכננים לתמוך בזה כאשר המבנה של האפליקציה דומה למבנה של אפליקציות RSC.

אחרי שתחום טעינת הנתונים התייצב יחסית, אנחנו בוחנים גם את הכיוון ההפוך: שליחת נתונים מהלקוח לשרת, כדי לבצע database mutations ולממש טפסים. אנחנו עושים זאת על ידי העברת פונקציות Server Action דרך הגבול בין שרת ללקוח, כך שהלקוח יכול לקרוא להן וליהנות מ-RPC חלק. Server Actions מאפשרות גם progressive enhancement לטפסים עוד לפני טעינת JavaScript.

React Server Components כבר נשלחה ב-[Next.js App Router](/learn/start-a-new-react-project#nextjs-app-router). זה מדגים אינטגרציה עמוקה של router שמאמץ את RSC כ-prmitive מרכזי, אבל זו לא הדרך היחידה לבנות router/framework תואמי-RSC. יש הפרדה ברורה בין יכולות שניתנות מהמפרט והיישום של RSC. React Server Components מיועדת להיות מפרט לקומפוננטות שעובדות בין frameworks תואמים.

באופן כללי אנחנו ממליצים להשתמש ב-framework קיים, אבל אם צריך אפשר לבנות framework מותאם אישית. בניית framework תואם-RSC עדיין מורכבת יותר ממה שהיינו רוצים, בעיקר בגלל אינטגרציית bundler עמוקה שנדרשת. הדור הנוכחי של bundlers מצוין ללקוח, אבל לא תוכנן עם תמיכה מדרגה ראשונה בפיצול module graph יחיד בין שרת ללקוח. לכן אנחנו עובדים ישירות עם מפתחי bundlers כדי להכניס primitives מובנים ל-RSC.

## Asset Loading {/*asset-loading*/}

[`Suspense`](/reference/react/Suspense) מאפשרת להגדיר מה להציג בזמן שהנתונים או הקוד של הקומפוננטות עדיין נטענים. כך משתמשים רואים יותר ויותר תוכן בהדרגה בזמן טעינת העמוד וגם בזמן ניווטים ב-router שטוענים עוד נתונים וקוד. אבל מנקודת מבט המשתמש, טעינת נתונים ורינדור לא מספרים את כל הסיפור לגבי מוכנות התוכן. כברירת מחדל, דפדפנים טוענים stylesheets, fonts ותמונות בנפרד, מה שעלול לגרום לקפיצות UI ושינויי layout עוקבים.

אנחנו עובדים על אינטגרציה מלאה של Suspense עם מחזור הטעינה של stylesheets, fonts ותמונות, כך ש-React תביא אותם בחשבון כדי לקבוע מתי התוכן באמת מוכן להצגה. בלי לשנות את אופן הכתיבה של הקומפוננטות שלכם, העדכונים ירגישו קוהרנטיים ונעימים יותר. כאופטימיזציה, נספק גם דרך ידנית לבצע preload למשאבים כמו fonts ישירות מתוך קומפוננטות.

היכולות האלה כרגע במימוש, ונשתף עוד בקרוב.

## Document Metadata {/*document-metadata*/}

עמודים ומסכים שונים באפליקציה עשויים לדרוש metadata שונה, כמו תגית `<title>`, תיאור, ותגיות `<meta>` נוספות. מבחינת תחזוקה, עדיף לשמור מידע כזה קרוב לקומפוננטת React של אותו עמוד/מסך. אבל בפועל תגיות HTML הללו חייבות להיות בתוך `<head>` של המסמך, שבדרך כלל נרנדר מקומפוננטה בשורש האפליקציה.

היום פותרים את זה לרוב באחת משתי דרכים.

דרך אחת היא לרנדר קומפוננטת צד-שלישי מיוחדת שמעבירה `<title>`, `<meta>` ותגיות נוספות ל-`<head>`. זה עובד בדפדפנים מרכזיים, אבל יש הרבה לקוחות שלא מריצים JavaScript בצד הלקוח, כמו Open Graph parsers, ולכן זו לא גישה אוניברסלית.

דרך שנייה היא לרנדר את העמוד בשרת בשני שלבים: קודם התוכן הראשי נאסף יחד עם כל התגיות הרלוונטיות, ואז מרנדרים `<head>` עם אותן תגיות, ורק אז שולחים את הכול לדפדפן. הגישה הזו עובדת, אבל מונעת ניצול של [React 18 Streaming Server Renderer](/reference/react-dom/server/renderToReadableStream), כי צריך לחכות לכל התוכן לפני שליחת `<head>`.

לכן אנחנו מוסיפים תמיכה מובנית ברינדור `<title>`, `<meta>`, ותגיות metadata מסוג `<link>` בכל מקום בעץ הקומפוננטות, מהקופסה. זה יעבוד אותו דבר בכל סביבה: קוד client-side מלא, SSR, ובעתיד גם RSC. נשתף פרטים נוספים בקרוב.

## React Optimizing Compiler {/*react-optimizing-compiler*/}

מאז העדכון הקודם עשינו איטרציה פעילה על העיצוב של [React Forget](/blog/2022/06/15/react-labs-what-we-have-been-working-on-june-2022#react-compiler), קומפיילר אופטימיזציה ל-React. דיברנו עליו בעבר כ-"auto-memoizing compiler", וזה נכון במובן מסוים. אבל העבודה עליו העמיקה אצלנו את ההבנה של מודל התכנות ב-React. דרך מדויקת יותר להבין את React Forget היא כקומפיילר *reactivity* אוטומטי.

הרעיון המרכזי ב-React הוא שמפתחים מגדירים UI כפונקציה של המצב הנוכחי. עובדים עם ערכי JavaScript רגילים - מספרים, מחרוזות, מערכים, אובייקטים - ומשתמשים באידיומים רגילים של JavaScript - `if/else`, `for` וכו' - כדי לתאר את לוגיקת הקומפוננטה. המודל המנטלי הוא ש-React תרנדר מחדש כשמצב האפליקציה משתנה. אנחנו מאמינים שהמודל הפשוט הזה וההיצמדות לסמנטיקה של JavaScript הם עקרון חשוב ב-React.

האתגר הוא שלפעמים React *מגיבה יותר מדי*: היא מרנדרת מחדש יותר מדי. לדוגמה, ב-JavaScript אין דרך זולה להשוות אם שני אובייקטים/מערכים שקולים, ולכן יצירת אובייקט חדש בכל render יכולה לגרום ל-React לבצע יותר עבודה מהנדרש. לכן מפתחים נאלצים לבצע memoization מפורש כדי לא "להגיב יתר על המידה" לשינויים.

המטרה שלנו ב-React Forget היא להבטיח כמות reactivity נכונה כברירת מחדל: שהאפליקציה תרנדר מחדש רק כשערכי state משתנים *באופן משמעותי*. מבחינת מימוש זה אומר memoization אוטומטי, אבל אנחנו חושבים שהמסגור כ-reactivity מסביר טוב יותר את React ו-Forget. דרך להבין זאת: כיום React מרנדרת מחדש לפי שינוי בזהות אובייקט. עם Forget, React תרנדר מחדש לפי שינוי בערך הסמנטי, בלי לשלם עלות runtime של השוואות עמוקות.

מבחינת התקדמות קונקרטית, מאז העדכון האחרון עשינו איטרציה משמעותית על עיצוב הקומפיילר כדי ליישר אותו עם הגישה הזו ולשלב משוב משימוש פנימי. אחרי refactors גדולים בסוף השנה שעברה, התחלנו להשתמש בקומפיילר בפרודקשן באזורים מוגבלים ב-Meta. אנחנו מתכננים לפתוח אותו לקוד פתוח אחרי שנוכיח אותו בפרודקשן.

בנוסף, הרבה אנשים ביקשו להבין יותר איך הקומפיילר עובד. נשמח לשתף הרבה יותר פרטים אחרי שנוכיח ונפתח אותו. בינתיים אפשר לשתף כמה נקודות:

ליבת הקומפיילר כמעט מנותקת לגמרי מ-Babel, ו-API הליבה הוא בקירוב AST נכנס, AST יוצא (תוך שמירה על source locations). מתחת למכסה המנוע אנחנו משתמשים בייצוג קוד מותאם וב-pipeline טרנספורמציות כדי לבצע ניתוח סמנטי ברמה נמוכה. עם זאת, הממשק הציבורי המרכזי לקומפיילר יהיה דרך Babel ותוספי build נוספים. לצורכי בדיקות יש לנו כרגע Babel plugin דק מאוד שקורא לקומפיילר, מייצר גרסה חדשה לכל פונקציה ומחליף אותה.

במהלך ה-refactor בחודשים האחרונים רצינו להתמקד בשיפור מודל הקומפילציה המרכזי כדי לוודא שהוא מתמודד עם מורכבויות כמו תנאים, לולאות, השמה מחדש ומוטציות. אבל ל-JavaScript יש דרכים רבות לבטא כל אחת מהיכולות האלה: `if/else`, ternaries, `for`, `for-in`, `for-of` וכו'. תמיכה מלאה בכל השפה מראש הייתה מעכבת את האימות של מודל הליבה. במקום זה התחלנו בתת-קבוצה קטנה אך מייצגת: `let/const`, `if/else`, לולאות `for`, אובייקטים, מערכים, primitives, קריאות פונקציה ועוד כמה יכולות. ככל שהביטחון במודל גדל והפשטנו את ההפשטות הפנימיות, הרחבנו את תת-הקבוצה הנתמכת. אנחנו גם מסמנים במפורש תחביר שלא נתמך עדיין, מדווחים diagnostics ומדלגים על קומפילציה לקלט לא נתמך. יש לנו כלים להריץ את הקומפיילר על בסיסי הקוד של Meta ולזהות אילו יכולות חסרות הן הנפוצות ביותר כדי לתעדף אותן בהמשך. נמשיך להרחיב בהדרגה עד תמיכה בשפה כולה.

כדי להפוך JavaScript רגיל בקומפוננטות React ל-reactive צריך קומפיילר עם הבנה סמנטית עמוקה, כדי להבין בדיוק מה הקוד עושה. בגישה הזו אנחנו בונים מערכת reactivity בתוך JavaScript שמאפשרת כתיבת קוד מוצר בכל רמת מורכבות עם כל העוצמה של השפה, במקום להיות מוגבלים לשפה ייעודית צרה.

## Offscreen Rendering {/*offscreen-rendering*/}

Offscreen rendering היא יכולת מתוכננת ב-React לרינדור מסכים ברקע ללא עלות ביצועים נוספת. אפשר לחשוב עליה כגרסה של [`content-visibility` ב-CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/content-visibility) שעובדת לא רק על אלמנטים ב-DOM אלא גם על קומפוננטות React. במהלך המחקר מצאנו מגוון שימושים:

- router יכול לבצע prerender למסכים ברקע כך שכשהמשתמש מנווט אליהם הם זמינים מיד.
- קומפוננטת החלפת טאבים יכולה לשמור את המצב של טאבים מוסתרים, כך שאפשר לעבור ביניהם בלי לאבד התקדמות.
- קומפוננטת רשימה וירטואלית יכולה לבצע prerender לשורות נוספות מעל ומתחת לאזור הגלוי.
- בעת פתיחת modal/popup אפשר להעביר את שאר האפליקציה למצב "background" כך שאירועים ועדכונים כבויים לכל מה שמחוץ למודל.

רוב מפתחי React לא יעבדו ישירות עם APIs של offscreen. במקום זה, offscreen rendering תשולב בראוטרים ובספריות UI, ומי שמשתמש בספריות האלה ירוויח אוטומטית בלי עבודה נוספת.

הרעיון הוא שתוכלו לרנדר כל עץ React ב-offscreen בלי לשנות את אופן כתיבת הקומפוננטות. כשקומפוננטה מרונדרת ב-offscreen היא לא באמת *מתבצעת mount* עד שהיא הופכת לגלויה, ולכן ה-effects שלה לא מופעלים. לדוגמה, אם קומפוננטה משתמשת ב-`useEffect` כדי לרשום אנליטיקה כשהיא מופיעה לראשונה, prerendering לא יפגע בדיוק האנליטיקה. באופן דומה, כשהקומפוננטה עוברת ל-offscreen גם ה-effects שלה עושים unmount. תכונה מרכזית היא שאפשר להחליף נראות בלי לאבד state.

מאז העדכון האחרון בדקנו גרסה ניסיונית פנימית של prerendering באפליקציות React Native ב-Android וב-iOS עם תוצאות ביצועים חיוביות. שיפרנו גם את האינטגרציה עם Suspense: השהיה בתוך עץ offscreen לא תפעיל Suspense fallbacks. העבודה שנותרה היא לסיים את ה-primitives שנחשפים למפתחי ספריות. אנחנו מצפים לפרסם RFC בהמשך השנה יחד עם API ניסיוני לבדיקות ומשוב.

## Transition Tracing {/*transition-tracing*/}

Transition Tracing API מאפשר לזהות מתי [React Transitions](/reference/react/useTransition) נעשים איטיים ולחקור למה. אחרי העדכון האחרון השלמנו את העיצוב הראשוני ופרסמנו [RFC](https://github.com/reactjs/rfcs/pull/238). היכולות הבסיסיות גם מומשו. כרגע הפרויקט בהשהיה זמנית. נשמח למשוב על ה-RFC ומצפים לחזור לפיתוח כדי לספק כלי מדידה ביצועים טוב יותר ל-React. זה יהיה שימושי במיוחד עם routers שמבוססים על React Transitions, כמו [Next.js App Router](/learn/start-a-new-react-project#nextjs-app-router).

* * *
בנוסף לעדכון הזה, הצוות שלנו התארח לאחרונה בפודקאסטים ושידורים חיים של הקהילה כדי לדבר על העבודה שלנו ולענות על שאלות.

* [Dan Abramov](https://twitter.com/dan_abramov) ו-[Joe Savona](https://twitter.com/en_JS) התראיינו אצל [Kent C. Dodds ביוטיוב](https://www.youtube.com/watch?v=h7tur48JSaw), ושם דיברו על חששות סביב React Server Components.
* [Dan Abramov](https://twitter.com/dan_abramov) ו-[Joe Savona](https://twitter.com/en_JS) היו אורחים ב-[JSParty podcast](https://jsparty.fm/267) ושיתפו מחשבות על עתיד React.

תודה ל-[Andrew Clark](https://twitter.com/acdlite), [Dan Abramov](https://twitter.com/dan_abramov), [Dave McCabe](https://twitter.com/mcc_abe), [Luna Wei](https://twitter.com/lunaleaps), [Matt Carroll](https://twitter.com/mattcarrollcode), [Sean Keegan](https://twitter.com/DevRelSean), [Sebastian Silbermann](https://twitter.com/sebsilbermann), [Seth Webster](https://twitter.com/sethwebster), and [Sophie Alpert](https://twitter.com/sophiebits) על סקירת הפוסט הזה.

תודה שקראתם, נתראה בעדכון הבא!
