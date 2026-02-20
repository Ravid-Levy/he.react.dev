---
title: "React Labs: על מה עבדנו - יוני 2022"
---

15 ביוני 2022 מאת [Andrew Clark](https://twitter.com/acdlite), [Dan Abramov](https://twitter.com/dan_abramov), [Jan Kassens](https://twitter.com/kassens), [Joseph Savona](https://twitter.com/en_JS), [Josh Story](https://twitter.com/joshcstory), [Lauren Tan](https://twitter.com/potetotes), [Luna Ruan](https://twitter.com/lunaruan), [Mengdi Chen](https://twitter.com/mengdi_en), [Rick Hanlon](https://twitter.com/rickhanlonii), [Robert Zhang](https://twitter.com/jiaxuanzhang01), [Sathya Gunasekaran](https://twitter.com/_gsathya), [Sebastian Markbåge](https://twitter.com/sebmarkbage), and [Xuan Huang](https://twitter.com/Huxpro)

---

<Intro>

[React 18](/blog/2022/03/29/react-v18) נבנתה במשך שנים, ובדרך סיפקה לצוות React לא מעט שיעורים חשובים. השחרור שלה היה תוצאה של הרבה שנות מחקר ובדיקה של כיוונים רבים. חלק מהכיוונים הצליחו; הרבה אחרים היו מבוי סתום שהוביל לתובנות חדשות. אחד הדברים שלמדנו הוא שמאוד מתסכל עבור הקהילה להמתין ליכולות חדשות בלי חשיפה לכיווני החשיבה שאנחנו בודקים.

</Intro>

---

בכל רגע נתון יש לנו כמה פרויקטים בעבודה, מטווח ניסיוני מאוד ועד דברים עם הגדרה ברורה. קדימה, אנחנו רוצים להתחיל לשתף באופן קבוע יותר במה שעבדנו עליו מול הקהילה בכל הפרויקטים האלה.

כדי לתאם ציפיות: זו לא מפת דרכים עם לוחות זמנים ברורים. הרבה מהפרויקטים האלה עדיין במחקר פעיל וקשה להצמיד להם תאריך שחרור קונקרטי. ייתכן שחלקם בכלל לא יגיעו לשחרור בצורה הנוכחית, תלוי במה שנלמד. במקום זה, אנחנו רוצים לשתף אתכם בתחומי הבעיה שאנחנו חושבים עליהם באופן פעיל ומה למדנו עד כה.

## Server Components {/*server-components*/}

הכרזנו על [דמו ניסיוני של React Server Components](https://legacy.reactjs.org/blog/2020/12/21/data-fetching-with-react-server-components.html) (RSC) בדצמבר 2020. מאז סיימנו להשלים את התלויות שלהם ב-React 18, ועבדנו על שינויים בהשראת משוב מניסויים.

בפרט, אנחנו נוטשים את הרעיון של ספריות I/O מפוצלות (למשל react-fetch), ובמקום זאת מאמצים מודל async/await לתאימות טובה יותר. זה לא חוסם טכנית את שחרור RSC כי אפשר להשתמש גם בראוטרים לשליפת נתונים. שינוי נוסף הוא שאנחנו מתרחקים גם מגישת סיומות קבצים לטובת [תיוג גבולות](https://github.com/reactjs/rfcs/pull/189#issuecomment-1116482278).

אנחנו עובדים יחד עם Vercel ו-Shopify כדי לאחד תמיכה בבאנדלרים עבור סמנטיקה משותפת גם ב-Webpack וגם ב-Vite. לפני ההשקה, חשוב לנו לוודא שסמנטיקת ה-RSC זהה בכל אקו-סיסטם React. זה החסם המרכזי בדרך ליציבות.

## טעינת נכסים {/*asset-loading*/}

כיום נכסים כמו סקריפטים, סגנונות חיצוניים, פונטים ותמונות נטענים לרוב דרך מערכות חיצוניות. זה יכול להקשות על תיאום מול סביבות חדשות כמו streaming, Server Components ועוד.
אנחנו בוחנים הוספת APIs ל-preload ו-load של נכסים חיצוניים בצורה deduplicated דרך APIs של React שעובדים בכל סביבות React.

אנחנו גם בוחנים תמיכה ב-Suspense כדי שתמונות, CSS ופונטים יוכלו לחסום תצוגה עד שנטענים, אבל בלי לחסום streaming ו-concurrent rendering. זה יכול לסייע במניעת ["popcorning"](https://twitter.com/sebmarkbage/status/1516852731251724293) שבו המראה קופץ ומשתנה.

## אופטימיזציות לרינדור סטטי בשרת {/*static-server-rendering-optimizations*/}

Static Site Generation (SSG) ו-Incremental Static Regeneration (ISR) הן דרכים מצוינות להשיג ביצועים טובים עבור דפים שניתנים לקאש, אבל אנחנו חושבים שאפשר להוסיף יכולות לשיפור ביצועים גם עבור Server Side Rendering (SSR) דינמי, במיוחד כשהתוכן ברובו ניתן לקאש אבל לא כולו. אנחנו בוחנים דרכים לאופטם רינדור שרת בעזרת קומפילציה ומעברים סטטיים.

## React Optimizing Compiler {/*react-compiler*/}

נתנו [הצצה מוקדמת](https://www.youtube.com/watch?v=lGEMwh32soc) ל-React Forget ב-React Conf 2021. זה קומפיילר שמייצר אוטומטית את המקבילה לקריאות `useMemo` ו-`useCallback`, כדי למזער עלות רינדור מחדש ועדיין לשמר את מודל התכנות של React.

לאחרונה סיימנו שכתוב של הקומפיילר כדי להפוך אותו לאמין ומסוגל יותר. הארכיטקטורה החדשה מאפשרת לנו לנתח ולעשות memoization לדפוסים מורכבים יותר, למשל שימוש ב-[local mutations](/learn/keeping-components-pure#local-mutation-your-components-little-secret), ופותחת הזדמנויות רבות לאופטימיזציה בזמן קומפילציה מעבר להתאמה בלבד ל-Hooks של memoization.

אנחנו גם עובדים על playground לחקירת היבטים שונים של הקומפיילר. המטרה העיקרית שלו היא להקל על פיתוח הקומפיילר, אבל אנחנו חושבים שהוא גם יקל לנסות אותו ולבנות אינטואיציה למה הוא עושה. הוא מציג תובנות לגבי איך הוא עובד מאחורי הקלעים, ומרנדר בזמן אמת את הפלט של הקומפיילר בזמן ההקלדה. זה ישוחרר יחד עם הקומפיילר כשיהיה מוכן.

## Offscreen {/*offscreen*/}

היום, אם רוצים להסתיר ולהציג קומפוננטה, יש שתי אפשרויות. אחת היא להוסיף או להסיר אותה לגמרי מהעץ. הבעיה בגישה הזו היא שמצב ה-UI הולך לאיבוד בכל unmount, כולל מצב שנשמר ב-DOM כמו מיקום גלילה.

האפשרות השנייה היא להשאיר את הקומפוננטה mounted ולהחליף את התצוגה באופן ויזואלי עם CSS. זה שומר על מצב ה-UI, אבל מגיע עם עלות ביצועים כי React חייב להמשיך לרנדר את הקומפוננטה המוסתרת ואת כל ילדיה בכל עדכון חדש.

Offscreen מוסיף אפשרות שלישית: להסתיר את ה-UI ויזואלית, אבל להוריד עדיפות לתוכן שלו. הרעיון דומה ל-`content-visibility` ב-CSS: כשהתוכן מוסתר, הוא לא חייב להישאר מסונכרן עם שאר ה-UI. React יכול לדחות את עבודת הרינדור עד ששאר האפליקציה פנויה או עד שהתוכן שוב גלוי.

Offscreen הוא יכולת low-level שמאפשרת יכולות high-level. בדומה ליכולות concurrent אחרות של React כמו `startTransition`, ברוב המקרים לא תעבדו ישירות עם Offscreen API, אלא דרך framework עם דעה ברורה שמממש דפוסים כמו:

* **Instant transitions.** חלק מ-frameworks לניתוב כבר מבצעים prefetch לנתונים כדי להאיץ ניווטים הבאים, למשל בזמן hover על קישור. עם Offscreen, הם יוכלו גם prerender למסך הבא ברקע.
* **Reusable state.** באופן דומה, בניווט בין ראוטים או טאבים, אפשר להשתמש ב-Offscreen כדי לשמור את מצב המסך הקודם כדי לחזור ולהמשיך מהמקום שבו הפסקתם.
* **Virtualized list rendering.** כשמציגים רשימות גדולות של פריטים, frameworks של רשימות וירטואליות עושים prerender ליותר שורות מאלה שכרגע גלויות. אפשר להשתמש ב-Offscreen כדי לעשות prerender לשורות מוסתרות בעדיפות נמוכה יותר מהפריטים הגלויים.
* **Backgrounded content.** אנחנו בוחנים גם יכולת קשורה להורדת עדיפות לתוכן שרץ ברקע בלי להסתיר אותו, למשל כאשר מוצגת שכבת modal מעל.

## Transition Tracing {/*transition-tracing*/}

כרגע יש ל-React שני כלי profiling. ה-[Profiler המקורי](https://legacy.reactjs.org/blog/2018/09/10/introducing-the-react-profiler.html) מציג סקירה של כל ה-commits בסשן profiling. לכל commit הוא מציג גם את כל הקומפוננטות שרונדרו וכמה זמן זה לקח. בנוסף יש לנו גרסת beta של [Timeline Profiler](https://github.com/reactwg/react-18/discussions/76) שנוספה ב-React 18, שמציגה מתי קומפוננטות מתזמנות עדכונים ומתי React עובד עליהם. שני הכלים האלו עוזרים לזהות בעיות ביצועים בקוד.

הבנו שמפתחים לא תמיד מקבלים ערך גדול מידע על commits איטיים או קומפוננטות איטיות בלי הקשר. יותר מועיל להבין מה בפועל גורם ל-commits האיטיים. בנוסף, מפתחים רוצים לעקוב אחרי אינטראקציות ספציפיות (למשל לחיצה על כפתור, טעינה ראשונית או ניווט עמוד) כדי לזהות רגרסיות ביצועים ולהבין למה אינטראקציה הייתה איטית ואיך לתקן.

ניסינו בעבר לפתור את זה עם [Interaction Tracing API](https://gist.github.com/bvaughn/8de925562903afd2e7a12554adcdda16), אבל היו לו פגמי תכנון בסיסיים שפגעו בדיוק של מעקב אחר מקור האיטיות, ולעיתים גם גרמו לכך שאינטראקציות לא הסתיימו לעולם. בסוף [הסרנו את ה-API הזה](https://github.com/facebook/react/pull/20037) בגלל הבעיות האלה.

אנחנו עובדים על גרסה חדשה ל-Interaction Tracing API (כרגע בשם זמני Transition Tracing כי הוא מתחיל דרך `startTransition`) שפותרת את הבעיות האלה.

## תיעוד React החדש {/*new-react-docs*/}

בשנה שעברה הכרזנו על גרסת beta של אתר התיעוד החדש של React ([ששוחרר בהמשך כ-react.dev](/blog/2023/03/16/introducing-react-dev)). חומרי הלימוד החדשים מלמדים קודם Hooks וכוללים דיאגרמות ואיורים חדשים, וגם הרבה דוגמאות ואתגרים אינטראקטיביים. עצרנו זמנית את העבודה הזו כדי להתמקד בשחרור React 18, אבל עכשיו כש-React 18 יצא, חזרנו לעבוד באופן פעיל כדי להשלים ולשחרר את התיעוד החדש.

כרגע אנחנו כותבים סעיף מפורט על effects, כי שמענו שזה אחד הנושאים המאתגרים ביותר גם למשתמשי React חדשים וגם למנוסים. [Synchronizing with Effects](/learn/synchronizing-with-effects) הוא הדף הראשון בסדרה שפורסם, ועוד דפים יגיעו בשבועות הקרובים. כשהתחלנו לכתוב סעיף מפורט על effects, הבנו שניתן לפשט דפוסים נפוצים רבים על ידי הוספת primitive חדש ל-React. שיתפנו מחשבות ראשוניות על כך ב-[useEvent RFC](https://github.com/reactjs/rfcs/pull/220). כרגע זה עדיין במחקר מוקדם ואנחנו ממשיכים באיטרציה על הרעיון. אנחנו מעריכים מאוד את תגובות הקהילה ל-RFC עד כה, וגם את ה-[feedback](https://github.com/reactjs/reactjs.org/issues/3308) והתרומות לשכתוב התיעוד המתמשך. אנחנו רוצים להודות במיוחד ל-[Harish Kumar](https://github.com/harish-sethuraman) על שליחת ובדיקת שיפורים רבים למימוש האתר החדש.

*תודה ל-[Sophie Alpert](https://twitter.com/sophiebits) על סקירת הפוסט הזה!* 
