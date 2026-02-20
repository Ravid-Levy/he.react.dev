---
title: "התוכנית ל-React 18"
---

8 ביוני 2021 מאת [Andrew Clark](https://twitter.com/acdlite), [Brian Vaughn](https://github.com/bvaughn), [Christine Abernathy](https://twitter.com/abernathyca), [Dan Abramov](https://twitter.com/dan_abramov), [Rachel Nabors](https://twitter.com/rachelnabors), [Rick Hanlon](https://twitter.com/rickhanlonii), [Sebastian Markbåge](https://twitter.com/sebmarkbage), and [Seth Webster](https://twitter.com/sethwebster)

---

<Intro>

צוות React שמח לשתף כמה עדכונים:

1. התחלנו לעבוד על React 18, שתהיה גרסת ה-major הבאה שלנו.
2. הקמנו Working Group כדי להכין את הקהילה לאימוץ הדרגתי של יכולות חדשות ב-React 18.
3. פרסמנו React 18 Alpha כדי שמחברי ספריות יוכלו לנסות ולתת משוב.

העדכונים האלה מיועדים בעיקר למתחזקי ספריות צד שלישי. אם אתם לומדים, מלמדים או משתמשים ב-React לבניית אפליקציות למשתמשי קצה, אפשר להתעלם בבטחה מהפוסט הזה. אבל אתם בהחלט מוזמנים לעקוב אחרי הדיונים ב-React 18 Working Group אם זה מסקרן אתכם.

---

</Intro>

## מה מגיע ב-React 18 {/*whats-coming-in-react-18*/}

כשתשוחרר, React 18 תכלול שיפורים מובנים (כמו [automatic batching](https://github.com/reactwg/react-18/discussions/21)), APIs חדשים (כמו [`startTransition`](https://github.com/reactwg/react-18/discussions/41)), וגם [renderer חדש לשרת בסטרימינג](https://github.com/reactwg/react-18/discussions/37) עם תמיכה מובנית ב-`React.lazy`.

היכולות האלה מתאפשרות בזכות מנגנון opt-in חדש שאנחנו מוסיפים ב-React 18. קוראים לו "concurrent rendering", והוא מאפשר ל-React להכין כמה גרסאות של ה-UI בו-זמנית. זה שינוי שמתרחש בעיקר מאחורי הקלעים, אבל הוא פותח אפשרויות חדשות לשיפור גם הביצועים בפועל וגם חוויית המהירות הנתפסת של האפליקציה.

אם עקבתם אחרי המחקר שלנו לגבי עתיד React (אנחנו לא מצפים שכן), אולי שמעתם על משהו שנקרא "concurrent mode" או שהוא עלול לשבור את האפליקציה שלכם. בעקבות משוב כזה מהקהילה, עיצבנו מחדש את אסטרטגיית השדרוג כך שתתמוך באימוץ הדרגתי. במקום "mode" של הכול-או-כלום, concurrent rendering יופעל רק עבור עדכונים שמופעלים על ידי אחת היכולות החדשות. בפועל זה אומר ש-**תוכלו לאמץ את React 18 בלי כתיבה מחדש, ולנסות יכולות חדשות בקצב שלכם.**

## אסטרטגיית אימוץ הדרגתית {/*a-gradual-adoption-strategy*/}

מכיוון ש-concurrency ב-React 18 הוא opt-in, אין שינויים שוברים משמעותיים בהתנהגות קומפוננטות כברירת מחדל. **אפשר לשדרג ל-React 18 עם מעט מאוד שינויים בקוד האפליקציה, או בלי שינויים בכלל, במאמץ דומה לשחרור major טיפוסי של React**. על סמך הניסיון שלנו בהסבת כמה אפליקציות ל-React 18, אנחנו מצפים שמשתמשים רבים יצליחו לשדרג תוך אחר צהריים אחד.

הוצאנו לפועל יכולות concurrent בעשרות אלפי קומפוננטות ב-Facebook, ובניסיון שלנו רוב קומפוננטות React "פשוט עובדות" בלי שינויים נוספים. אנחנו מחויבים לוודא שזה יהיה שדרוג חלק לכל הקהילה, ולכן אנחנו מכריזים היום על React 18 Working Group.

## עבודה עם הקהילה {/*working-with-the-community*/}

בשחרור הזה אנחנו מנסים משהו חדש: הזמנו פאנל של מומחים, מפתחים, מחברי ספריות ומדריכים מכל קהילת React להשתתף ב-[React 18 Working Group](https://github.com/reactwg/react-18), לתת משוב, לשאול שאלות ולשתף פעולה סביב השחרור. לא הצלחנו להזמין את כל מי שרצינו לקבוצה הראשונית והקטנה הזו, אבל אם הניסוי יעבוד טוב אנחנו מקווים שיהיו עוד בהמשך.

**המטרה של React 18 Working Group היא להכין את האקו-סיסטם לאימוץ חלק והדרגתי של React 18 באפליקציות ובספריות קיימות.** ה-Working Group מתארח ב-[GitHub Discussions](https://github.com/reactwg/react-18/discussions), וכל הציבור יכול לקרוא אותו. חברי הקבוצה יכולים להשאיר משוב, לשאול שאלות ולשתף רעיונות. צוות הליבה גם ישתמש במאגר הדיונים כדי לשתף ממצאי מחקר. ככל שנתקרב לשחרור היציב, מידע חשוב יפורסם גם בבלוג הזה.

למידע נוסף על שדרוג ל-React 18 ומשאבים נוספים על השחרור, ראו את [פוסט ההכרזה על React 18](https://github.com/reactwg/react-18/discussions/4).

## גישה ל-React 18 Working Group {/*accessing-the-react-18-working-group*/}

כולם יכולים לקרוא את הדיונים ב-[מאגר React 18 Working Group](https://github.com/reactwg/react-18).

מכיוון שאנחנו מצפים לגל עניין ראשוני, רק חברים מוזמנים יורשו ליצור או להגיב בשרשורים. עם זאת, השרשורים גלויים לגמרי לציבור, כך שלכולם יש גישה לאותו מידע. אנחנו מאמינים שזה איזון טוב בין יצירת סביבה פרודוקטיבית לחברי הקבוצה לבין שקיפות מלאה מול הקהילה הרחבה.

כמו תמיד, אפשר לשלוח דיווחי באגים, שאלות ומשוב כללי ל-[issue tracker](https://github.com/facebook/react/issues) שלנו.

## איך לנסות React 18 Alpha כבר היום {/*how-to-try-react-18-alpha-today*/}

גרסאות alpha חדשות [מתפרסמות באופן קבוע ל-npm עם התגית `@alpha`](https://github.com/reactwg/react-18/discussions/9). הגרסאות האלה נבנות מהקומיט האחרון במאגר הראשי שלנו. כשיכולת או תיקון באג מתמזגים, הם יופיעו ב-alpha ביום העבודה הבא.

ייתכנו שינויים משמעותיים בהתנהגות או ב-API בין שחרורי alpha. חשוב לזכור ש-**שחרורי alpha לא מומלצים לאפליקציות ייצור מול משתמשים.**

## לוח זמנים משוער לשחרור React 18 {/*projected-react-18-release-timeline*/}

אין לנו כרגע תאריך שחרור ספציפי, אבל אנחנו מצפים שיידרשו כמה חודשים של משוב ואיטרציה לפני ש-React 18 תהיה מוכנה לרוב אפליקציות הייצור.

* Library Alpha: זמין היום
* Public Beta: לפחות כמה חודשים
* Release Candidate (RC): לפחות כמה שבועות אחרי Beta
* General Availability: לפחות כמה שבועות אחרי RC

פרטים נוספים על לוח הזמנים המשוער [זמינים ב-Working Group](https://github.com/reactwg/react-18/discussions/9). נפרסם עדכונים בבלוג הזה כשנתקרב לשחרור ציבורי.
