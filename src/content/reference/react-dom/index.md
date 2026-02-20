---
title: React DOM APIs
---

<Intro>

החבילה `react-dom` כוללת מתודות שנתמכות רק באפליקציות ווב (שפועלות בסביבת DOM של הדפדפן). הן לא נתמכות ב-React Native.

</Intro>

---

## APIs {/*apis*/}

את ה-APIs האלה אפשר לייבא מתוך הקומפוננטות. הם בשימוש נדיר:

* [`createPortal`](/reference/react-dom/createPortal) מאפשר לרנדר קומפוננטות ילדים בחלק אחר של עץ ה-DOM.
* [`flushSync`](/reference/react-dom/flushSync) מאפשר לאלץ את React לבצע flush לעדכון state ולעדכן את ה-DOM בצורה סינכרונית.

## APIs לטעינה מוקדמת של משאבים {/*resource-preloading-apis*/}

אפשר להשתמש ב-APIs האלה כדי להאיץ אפליקציות באמצעות טעינה מוקדמת של משאבים כמו סקריפטים, קובצי סגנון וגופנים, מיד כשידוע שתצטרכו אותם, למשל לפני ניווט לעמוד אחר שבו המשאבים האלה יידרשו.

[Frameworks מבוססי React](/learn/start-a-new-react-project) מטפלים לעיתים קרובות בטעינת משאבים בשבילכם, אז ייתכן שלא תצטרכו לקרוא ל-APIs האלה ישירות. לפרטים, עיינו בתיעוד של ה-framework שלכם.

* [`prefetchDNS`](/reference/react-dom/prefetchDNS) מאפשר לבצע prefetch לכתובת ה-IP של שם דומיין DNS שאתם מצפים להתחבר אליו.
* [`preconnect`](/reference/react-dom/preconnect) מאפשר להתחבר מראש לשרת שממנו אתם מצפים לבקש משאבים, גם אם עדיין לא ידוע אילו משאבים תצטרכו.
* [`preload`](/reference/react-dom/preload) מאפשר להביא מראש stylesheet, font, image, או סקריפט חיצוני שאתם מצפים להשתמש בהם.
* [`preloadModule`](/reference/react-dom/preloadModule) מאפשר להביא מראש מודול ESM שאתם מצפים להשתמש בו.
* [`preinit`](/reference/react-dom/preinit) מאפשר להביא ולהעריך סקריפט חיצוני, או להביא ולהכניס stylesheet.
* [`preinitModule`](/reference/react-dom/preinitModule) מאפשר להביא ולהעריך מודול ESM.

---

## נקודות כניסה {/*entry-points*/}

החבילה `react-dom` מספקת שתי נקודות כניסה נוספות:

* [`react-dom/client`](/reference/react-dom/client) כוללת APIs לרינדור קומפוננטות React בצד לקוח (בדפדפן).
* [`react-dom/server`](/reference/react-dom/server) כוללת APIs לרינדור קומפוננטות React בצד שרת.

---

## APIs שהוצאו משימוש {/*deprecated-apis*/}

<Deprecated>

ה-APIs האלה יוסרו בגרסה ראשית עתידית של React.

</Deprecated>

* [`findDOMNode`](/reference/react-dom/findDOMNode) מוצא את DOM node הקרוב ביותר שמקביל למופע class component.
* [`hydrate`](/reference/react-dom/hydrate) מרכיב עץ לתוך DOM שנוצר מ-HTML של שרת. הוצא משימוש לטובת [`hydrateRoot`](/reference/react-dom/client/hydrateRoot).
* [`render`](/reference/react-dom/render) מרכיב עץ לתוך ה-DOM. הוצא משימוש לטובת [`createRoot`](/reference/react-dom/client/createRoot).
* [`unmountComponentAtNode`](/reference/react-dom/unmountComponentAtNode) מסיר עץ מה-DOM. הוצא משימוש לטובת [`root.unmount()`](/reference/react-dom/client/createRoot#root-unmount).
