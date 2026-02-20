---
title: "סיכום React Conf 2021"
---

17 בדצמבר 2021 מאת [Jesslyn Tannady](https://twitter.com/jtannady) and [Rick Hanlon](https://twitter.com/rickhanlonii)

---

<Intro>

בשבוע שעבר אירחנו את React Conf השישי שלנו. בשנים קודמות השתמשנו בבמה של React Conf כדי למסור הכרזות ששינו את התעשייה, כמו [_React Native_](https://engineering.fb.com/2015/03/26/android/react-native-bringing-modern-web-techniques-to-mobile/) ו-[_React Hooks_](https://reactjs.org/docs/hooks-intro.html). השנה שיתפנו את החזון הרב-פלטפורמי שלנו ל-React, החל משחרור React 18 ואימוץ הדרגתי של יכולות concurrent.

</Intro>

---

זו הייתה הפעם הראשונה ש-React Conf התארח אונליין, והוא שודר בחינם עם תרגום ל-8 שפות שונות. משתתפים מכל העולם הצטרפו ל-Discord של הכנס ולאירוע השידור החוזר, כדי לאפשר נגישות בכל אזורי הזמן. יותר מ-50,000 אנשים נרשמו, עם מעל 60,000 צפיות ב-19 הרצאות, ו-5,000 משתתפים ב-Discord בשני האירועים יחד.

כל ההרצאות [זמינות לצפייה אונליין](https://www.youtube.com/watch?v=FZ0cG47msEk&list=PLNG_1j3cPCaZZ7etkzWA7JfdmKWT0pMsa).

הנה סיכום של מה שעלה על הבמה:

## React 18 ויכולות concurrent {/*react-18-and-concurrent-features*/}

בהרצאת הפתיחה שיתפנו את החזון שלנו לעתיד React, שמתחיל ב-React 18.

React 18 מוסיף את מנוע ה-renderer של concurrent שחיכו לו זמן רב, יחד עם עדכונים ל-Suspense, בלי שינויים שוברים משמעותיים. אפליקציות יכולות לשדרג ל-React 18 ולהתחיל לאמץ יכולות concurrent בהדרגה, במאמץ דומה לכל שחרור major אחר.

**זה אומר שאין concurrent mode, יש רק concurrent features.**

בהרצאת הפתיחה שיתפנו גם את החזון שלנו לגבי Suspense, Server Components, קבוצות עבודה חדשות של React, וחזון רב-פלטפורמי ארוך טווח עבור React Native.

צפו בהרצאת הפתיחה המלאה של [Andrew Clark](https://twitter.com/acdlite), [Juan Tejada](https://twitter.com/_jstejada), [Lauren Tan](https://twitter.com/potetotes), and [Rick Hanlon](https://twitter.com/rickhanlonii):

<YouTubeIframe src="https://www.youtube.com/embed/FZ0cG47msEk" />

## React 18 למפתחי אפליקציות {/*react-18-for-application-developers*/}

בהרצאת הפתיחה הכרזנו גם ש-React 18 RC זמין לניסיון כבר עכשיו. בכפוף למשוב נוסף, זו תהיה בדיוק גרסת React שנפרסם כיציבה בתחילת השנה הבאה.

כדי לנסות את React 18 RC, שדרגו את התלויות:

```bash
npm install react@rc react-dom@rc
```

ועברו ל-API החדש של `createRoot`:

```js
// before
const container = document.getElementById('root');
ReactDOM.render(<App />, container);

// after
const container = document.getElementById('root');
const root = ReactDOM.createRoot(container);
root.render(<App/>);
```

להדגמה של שדרוג ל-React 18, ראו את ההרצאה של [Shruti Kapoor](https://twitter.com/shrutikapoor08):

<YouTubeIframe src="https://www.youtube.com/embed/ytudH8je5ko" />

## Streaming Server Rendering עם Suspense {/*streaming-server-rendering-with-suspense*/}

React 18 כולל גם שיפורים לביצועי רינדור צד שרת באמצעות Suspense.

Streaming server rendering מאפשר לייצר HTML מקומפוננטות React בשרת, ולהזרים את ה-HTML הזה למשתמשים. ב-React 18 אפשר להשתמש ב-`Suspense` כדי לפרק את האפליקציה ליחידות קטנות ועצמאיות שאפשר להזרים בנפרד בלי לחסום את שאר האפליקציה. כלומר, משתמשים יראו תוכן מוקדם יותר ויוכלו להתחיל אינטראקציה הרבה יותר מהר.

לצלילה עמוקה, ראו את ההרצאה של [Shaundai Person](https://twitter.com/shaundai):

<YouTubeIframe src="https://www.youtube.com/embed/pj5N-Khihgc" />

## קבוצת העבודה הראשונה של React {/*the-first-react-working-group*/}

עבור React 18 יצרנו את קבוצת העבודה הראשונה שלנו כדי לשתף פעולה עם פאנל של מומחים, מפתחים, מתחזקי ספריות ומדריכים. יחד בנינו את אסטרטגיית האימוץ ההדרגתי שלנו ושייפנו APIs חדשים כמו `useId`, `useSyncExternalStore`, ו-`useInsertionEffect`.

לסקירה של העבודה הזו, ראו את ההרצאה של [Aakansha' Doshi](https://twitter.com/aakansha1216):

<YouTubeIframe src="https://www.youtube.com/embed/qn7gRClrC9U" />

## כלי פיתוח עבור React {/*react-developer-tooling*/}

כדי לתמוך ביכולות החדשות של השחרור הזה, הכרזנו גם על צוות React DevTools החדש ועל Timeline Profiler חדש שיעזור למפתחים לדבג אפליקציות React.

למידע נוסף ולהדגמה של יכולות DevTools חדשות, ראו את ההרצאה של [Brian Vaughn](https://twitter.com/brian_d_vaughn):

<YouTubeIframe src="https://www.youtube.com/embed/oxDfrke8rZg" />

## React בלי memo {/*react-without-memo*/}

במבט רחוק יותר לעתיד, [Xuan Huang (黄玄)](https://twitter.com/Huxpro) שיתף עדכון ממחקר React Labs שלנו על קומפיילר עם auto-memoization. לצפייה במידע נוסף ובהדגמה של אבטיפוס הקומפיילר:

<YouTubeIframe src="https://www.youtube.com/embed/lGEMwh32soc" />

## הרצאת תיעוד React {/*react-docs-keynote*/}

[Rachel Nabors](https://twitter.com/rachelnabors) פתחה מקבץ הרצאות על למידה ועיצוב עם React, עם keynote על ההשקעה שלנו בתיעוד React החדש ([שכיום הושק כ-react.dev](/blog/2023/03/16/introducing-react-dev)):

<YouTubeIframe src="https://www.youtube.com/embed/mneDaMYOKP8" />

## ועוד... {/*and-more*/}

**שמענו גם הרצאות על למידה ועיצוב עם React:**

* Debbie O'Brien: [Things I learnt from the new React docs](https://youtu.be/-7odLW_hG7s).
* Sarah Rainsberger: [Learning in the Browser](https://youtu.be/5X-WEQflCL0).
* Linton Ye: [The ROI of Designing with React](https://youtu.be/7cPWmID5XAk).
* Delba de Oliveira: [Interactive playgrounds with React](https://youtu.be/zL8cz2W0z34).

**הרצאות מצוותי Relay, React Native ו-PyTorch:**

* Robert Balicki: [Re-introducing Relay](https://youtu.be/lhVGdErZuN4).
* Eric Rozell and Steven Moyes: [React Native Desktop](https://youtu.be/9L4FFrvwJwY).
* Roman Rädle: [On-device Machine Learning for React Native](https://youtu.be/NLj73vrc2I8)

**וגם הרצאות קהילה על נגישות, כלי פיתוח ו-Server Components:**

* Daishi Kato: [React 18 for External Store Libraries](https://youtu.be/oPfSC5bQPR8).
* Diego Haz: [Building Accessible Components in React 18](https://youtu.be/dcm8fjBfro8).
* Tafu Nakazaki: [Accessible Japanese Form Components with React](https://youtu.be/S4a0QlsH0pU).
* Lyle Troxell: [UI tools for artists](https://youtu.be/b3l4WxipFsE).
* Helen Lin: [Hydrogen + React 18](https://youtu.be/HS6vIYkSNks).

## תודה {/*thank-you*/}

זו הייתה השנה הראשונה שבה תכננו כנס בעצמנו, ויש לנו הרבה אנשים להודות להם.

קודם כול תודה לכל הדוברים שלנו [Aakansha Doshi](https://twitter.com/aakansha1216), [Andrew Clark](https://twitter.com/acdlite), [Brian Vaughn](https://twitter.com/brian_d_vaughn), [Daishi Kato](https://twitter.com/dai_shi), [Debbie O'Brien](https://twitter.com/debs_obrien), [Delba de Oliveira](https://twitter.com/delba_oliveira), [Diego Haz](https://twitter.com/diegohaz), [Eric Rozell](https://twitter.com/EricRozell), [Helen Lin](https://twitter.com/wizardlyhel), [Juan Tejada](https://twitter.com/_jstejada), [Lauren Tan](https://twitter.com/potetotes), [Linton Ye](https://twitter.com/lintonye), [Lyle Troxell](https://twitter.com/lyle), [Rachel Nabors](https://twitter.com/rachelnabors), [Rick Hanlon](https://twitter.com/rickhanlonii), [Robert Balicki](https://twitter.com/StatisticsFTW), [Roman Rädle](https://twitter.com/raedle), [Sarah Rainsberger](https://twitter.com/sarah11918), [Shaundai Person](https://twitter.com/shaundai), [Shruti Kapoor](https://twitter.com/shrutikapoor08), [Steven Moyes](https://twitter.com/moyessa), [Tafu Nakazaki](https://twitter.com/hawaiiman0), and  [Xuan Huang (黄玄)](https://twitter.com/Huxpro).

תודה לכל מי שעזרו לתת פידבק על ההרצאות, כולל [Andrew Clark](https://twitter.com/acdlite), [Dan Abramov](https://twitter.com/dan_abramov), [Dave McCabe](https://twitter.com/mcc_abe), [Eli White](https://twitter.com/Eli_White), [Joe Savona](https://twitter.com/en_JS),  [Lauren Tan](https://twitter.com/potetotes), [Rachel Nabors](https://twitter.com/rachelnabors), and [Tim Yung](https://twitter.com/yungsters).

תודה ל-[Lauren Tan](https://twitter.com/potetotes) על הקמת Discord של הכנס ועל התפקיד כ-Discord admin שלנו.

תודה ל-[Seth Webster](https://twitter.com/sethwebster) על משוב לגבי הכיוון הכללי ועל כך שווידא שנשארנו ממוקדים בגיוון והכלה.

תודה ל-[Rachel Nabors](https://twitter.com/rachelnabors) על הובלת מאמץ המודרציה שלנו, ול-[Aisha Blake](https://twitter.com/AishaBlake) על יצירת מדריך המודרציה, הובלת צוות המודרציה, הכשרת המתרגמים והמודרטורים, וסיוע במודרציה של שני האירועים.

תודה למודרטורים שלנו [Jesslyn Tannady](https://twitter.com/jtannady), [Suzie Grange](https://twitter.com/missuze), [Becca Bailey](https://twitter.com/beccaliz), [Luna Wei](https://twitter.com/lunaleaps), [Joe Previte](https://twitter.com/jsjoeio), [Nicola Corti](https://twitter.com/Cortinico), [Gijs Weterings](https://twitter.com/gweterings), [Claudio Procida](https://twitter.com/claudiopro), Julia Neumann, Mengdi Chen, Jean Zhang, Ricky Li, and [Xuan Huang (黄玄)](https://twitter.com/Huxpro).

תודה ל-[Manjula Dube](https://twitter.com/manjula_dube), [Sahil Mhapsekar](https://twitter.com/apheri0), and Vihang Patel from [React India](https://www.reactindia.io/), ול-[Jasmine Xie](https://twitter.com/jasmine_xby), [QiChang Li](https://twitter.com/QCL15), and [YanLun Li](https://twitter.com/anneincoding) from [React China](https://twitter.com/ReactChina) על עזרה במודרציה של אירוע השידור החוזר ועל שמירה על מעורבות גבוהה בקהילה.

תודה ל-Vercel על פרסום [Virtual Event Starter Kit](https://vercel.com/virtual-event-starter-kit), שעליו נבנה אתר הכנס, ול-[Lee Robinson](https://twitter.com/leeerob) ו-[Delba de Oliveira](https://twitter.com/delba_oliveira) על שיתוף ניסיון מהפקת Next.js Conf.

תודה ל-[Leah Silber](https://twitter.com/wifelette) על שיתוף הניסיון שלה בהפקת כנסים, תובנות מ-[RustConf](https://rustconf.com/), ועל הספר שלה [Event Driven](https://leanpub.com/eventdriven/) והעצות שבו על הפקת כנסים.

תודה ל-[Kevin Lewis](https://twitter.com/_phzn) ול-[Rachel Nabors](https://twitter.com/rachelnabors) על שיתוף ניסיון מהפקת Women of React Conf.

תודה ל-[Aakansha Doshi](https://twitter.com/aakansha1216), [Laurie Barth](https://twitter.com/laurieontech), [Michael Chan](https://twitter.com/chantastic), and [Shaundai Person](https://twitter.com/shaundai) על העצות והרעיונות לאורך כל התכנון.

תודה ל-[Dan Lebowitz](https://twitter.com/lebo) על עזרה בעיצוב ובניית אתר הכנס והכרטיסים.

תודה ל-Laura Podolak Waddell, Desmond Osei-Acheampong, Mark Rossi, Josh Toberman ואחרים מצוות Facebook Video Productions על הקלטת הסרטונים ל-Keynote ולהרצאות עובדי Meta.

תודה לשותפים שלנו ב-HitPlay על עזרה בארגון הכנס, עריכת כל סרטוני השידור, תרגום כל ההרצאות ומודרציה של Discord בכמה שפות.

ולבסוף, תודה לכל המשתתפים שלנו שהפכו את זה ל-React Conf נהדר.
