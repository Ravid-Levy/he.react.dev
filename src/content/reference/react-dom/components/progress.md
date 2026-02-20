---
title: "<progress>"
---

<Intro>

רכיב הדפדפן המובנה [`<progress>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/progress) מאפשר לרנדר מחוון התקדמות.

```js
<progress value={0.5} />
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `<progress>` {/*progress*/}

כדי להציג מחוון התקדמות, רנדרו את רכיב הדפדפן המובנה [`<progress>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/progress).

```js
<progress value={0.5} />
```

[ראו דוגמאות נוספות בהמשך.](#usage)

#### Props {/*props*/}

`<progress>` תומך בכל [מאפייני האלמנט הנפוצים.](/reference/react-dom/components/common#props)

בנוסף, `<progress>` תומך ב-props האלה:

* [`max`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/progress#max): מספר. מציין את ערך ה-`value` המקסימלי. ברירת המחדל היא `1`.
* [`value`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/progress#value): מספר בין `0` ל-`max`, או `null` להתקדמות לא-מוגדרת. מציין כמה כבר הושלם.

---

## שימוש {/*usage*/}

### שליטה במחוון התקדמות {/*controlling-a-progress-indicator*/}

כדי להציג מחוון התקדמות, רנדרו קומפוננטת `<progress>`. אפשר להעביר מספר ב-`value` בין `0` לבין ערך ה-`max` שתגדירו. אם לא תעבירו `max`, ברירת המחדל תהיה `1`.

אם הפעולה אינה מתקדמת כרגע, העבירו `value={null}` כדי להעביר את מחוון ההתקדמות למצב לא-מוגדר.

<Sandpack>

```js
export default function App() {
  return (
    <>
      <progress value={0} />
      <progress value={0.5} />
      <progress value={0.7} />
      <progress value={75} max={100} />
      <progress value={1} />
      <progress value={null} />
    </>
  );
}
```

```css
progress { display: block; }
```

</Sandpack>
