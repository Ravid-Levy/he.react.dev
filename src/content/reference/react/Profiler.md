---
title: <Profiler>
---

<Intro>

`<Profiler>` מאפשר למדוד ביצועי רינדור של עץ React באופן פרוגרמטי.

```js
<Profiler id="App" onRender={onRender}>
  <App />
</Profiler>
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `<Profiler>` {/*profiler*/}

עטפו עץ קומפוננטות ב-`<Profiler>` כדי למדוד את ביצועי הרינדור שלו.

```js
<Profiler id="App" onRender={onRender}>
  <App />
</Profiler>
```

#### Props {/*props*/}

* `id`: מחרוזת שמזהה את החלק ב-UI שאתם מודדים.
* `onRender`: [`onRender` callback](#onrender-callback) ש-React קוראת לו בכל פעם שקומפוננטות בתוך העץ המנוטר מתעדכנות. הוא מקבל מידע על מה רונדר וכמה זמן זה לקח.

#### Caveats {/*caveats*/}

* Profiling מוסיף מעט overhead, לכן **הוא כבוי כברירת מחדל ב-build של production.** כדי להפעיל profiling ב-production צריך להפעיל [build מיוחד עם profiling פעיל.](https://fb.me/react-profiling)

---

### `onRender` callback {/*onrender-callback*/}

React תקרא ל-`onRender` callback שלכם עם מידע על מה שרונדר.

```js
function onRender(id, phase, actualDuration, baseDuration, startTime, commitTime) {
  // Aggregate or log render timings...
}
```

#### Parameters {/*onrender-parameters*/}

* `id`: מחרוזת ה-`id` prop של עץ ה-`<Profiler>` שבוצע לו commit עכשיו. זה מאפשר לזהות איזה חלק בעץ עבר commit אם אתם משתמשים בכמה profilers.
* `phase`: אחד מהערכים `"mount"`, `"update"` או `"nested-update"`. כך אפשר לדעת האם העץ הורכב עכשיו לראשונה או רונדר מחדש בגלל שינוי ב-props, state או hooks.
* `actualDuration`: מספר המילישניות שהושקעו ברינדור ה-`<Profiler>` וצאצאיו בעדכון הנוכחי. זה מצביע עד כמה תת-העץ משתמש היטב ב-memoization (למשל [`memo`](/reference/react/memo) ו-[`useMemo`](/reference/react/useMemo)). אידיאלית, הערך הזה אמור לרדת משמעותית אחרי ה-mount הראשוני כי רוב הצאצאים יצטרכו לרנדר מחדש רק אם ה-props הרלוונטיים שלהם משתנים.
* `baseDuration`: מספר המילישניות שמעריך כמה זמן היה לוקח לרנדר מחדש את כל תת-העץ של `<Profiler>` בלי אופטימיזציות. הוא מחושב כסכום זמני הרינדור האחרונים של כל קומפוננטה בעץ. הערך הזה מעריך את העלות במקרה הגרוע ביותר של רינדור (למשל mount ראשוני או עץ בלי memoization). השוו אותו ל-`actualDuration` כדי לראות האם memoization עובדת.
* `startTime`: חותמת זמן מספרית לרגע שבו React התחילה לרנדר את העדכון הנוכחי.
* `commitTime`: חותמת זמן מספרית לרגע שבו React ביצעה commit לעדכון הנוכחי. הערך הזה משותף לכל ה-profilers באותו commit, מה שמאפשר לקבץ אותם אם רוצים.

---

## שימוש {/*usage*/}

### מדידת ביצועי רינדור באופן פרוגרמטי {/*measuring-rendering-performance-programmatically*/}

עטפו את קומפוננטת `<Profiler>` סביב עץ React כדי למדוד את ביצועי הרינדור שלו.

```js {2,4}
<App>
  <Profiler id="Sidebar" onRender={onRender}>
    <Sidebar />
  </Profiler>
  <PageContent />
</App>
```

נדרשים שני props: `id` (מחרוזת) ו-`onRender` callback (פונקציה) ש-React קוראת לה בכל פעם שקומפוננטה בתוך העץ מבצעת update עם "commit".

<Pitfall>

Profiling מוסיף מעט overhead, לכן **הוא כבוי כברירת מחדל ב-build של production.** כדי להפעיל profiling ב-production צריך להפעיל [build מיוחד עם profiling פעיל.](https://fb.me/react-profiling)

</Pitfall>

<Note>

`<Profiler>` מאפשר לאסוף מדידות באופן פרוגרמטי. אם אתם מחפשים profiler אינטראקטיבי, נסו את לשונית Profiler בתוך [React Developer Tools](/learn/react-developer-tools). היא חושפת יכולות דומות כהרחבת דפדפן.

</Note>

---

### מדידת חלקים שונים באפליקציה {/*measuring-different-parts-of-the-application*/}

אפשר להשתמש בכמה קומפוננטות `<Profiler>` כדי למדוד חלקים שונים באפליקציה:

```js {5,7}
<App>
  <Profiler id="Sidebar" onRender={onRender}>
    <Sidebar />
  </Profiler>
  <Profiler id="Content" onRender={onRender}>
    <Content />
  </Profiler>
</App>
```

אפשר גם לקנן קומפוננטות `<Profiler>`:

```js {5,7,9,12}
<App>
  <Profiler id="Sidebar" onRender={onRender}>
    <Sidebar />
  </Profiler>
  <Profiler id="Content" onRender={onRender}>
    <Content>
      <Profiler id="Editor" onRender={onRender}>
        <Editor />
      </Profiler>
      <Preview />
    </Content>
  </Profiler>
</App>
```

למרות ש-`<Profiler>` היא קומפוננטה קלת משקל, כדאי להשתמש בה רק כשצריך. כל שימוש מוסיף מעט עומס CPU וזיכרון לאפליקציה.

---
