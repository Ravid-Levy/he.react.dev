---
title: "Directives"
canary: true
---

<Canary>

ה-directives האלה נדרשים רק אם אתם [משתמשים ב-React Server Components](/learn/start-a-new-react-project#bleeding-edge-react-frameworks) או בונים ספרייה שתואמת אליהם.

</Canary>

<Intro>

Directives מספקים הנחיות ל-[bundlers שתואמים ל-React Server Components](/learn/start-a-new-react-project#bleeding-edge-react-frameworks).

</Intro>

---

## Directives בקוד מקור {/*source-code-directives*/}

* [`'use client'`](/reference/react/use-client) מאפשר לסמן איזה קוד רץ בצד לקוח.
* [`'use server'`](/reference/react/use-server) מסמן פונקציות צד שרת שאפשר לקרוא להן מקוד צד לקוח.
