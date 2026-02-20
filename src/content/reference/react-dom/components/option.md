---
title: "<option>"
---

<Intro>

רכיב הדפדפן המובנה [`<option>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/option) מאפשר לרנדר אפשרות בתוך תיבת [`<select>`](/reference/react-dom/components/select).

```js
<select>
  <option value="someOption">Some option</option>
  <option value="otherOption">Other option</option>
</select>
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `<option>` {/*option*/}

רכיב הדפדפן המובנה [`<option>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/option) מאפשר לרנדר אפשרות בתוך תיבת [`<select>`](/reference/react-dom/components/select).

```js
<select>
  <option value="someOption">Some option</option>
  <option value="otherOption">Other option</option>
</select>
```

[ראו דוגמאות נוספות בהמשך.](#usage)

#### Props {/*props*/}

`<option>` תומך בכל [מאפייני האלמנט הנפוצים.](/reference/react-dom/components/common#props)

בנוסף, `<option>` תומך ב-props האלה:

* [`disabled`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/option#disabled): ערך בוליאני. אם `true`, לא ניתן יהיה לבחור באפשרות והיא תוצג מעומעמת.
* [`label`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/option#label): מחרוזת. מציינת את משמעות האפשרות. אם לא הוגדרה, ישמש הטקסט שבתוך האפשרות.
* [`value`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/option#value): הערך שישמש [בעת שליחת טופס ה-`<select>` ההורה](/reference/react-dom/components/select#reading-the-select-box-value-when-submitting-a-form) אם האפשרות הזו נבחרה.

#### Caveats {/*caveats*/}

* React לא תומכת במאפיין `selected` על `<option>`. במקום זאת, העבירו את ה-`value` של האפשרות ל-[`<select defaultValue>`](/reference/react-dom/components/select#providing-an-initially-selected-option) של ההורה עבור select לא נשלט, או ל-[`<select value>`](/reference/react-dom/components/select#controlling-a-select-box-with-a-state-variable) עבור select נשלט.

---

## שימוש {/*usage*/}

### הצגת תיבת select עם אפשרויות {/*displaying-a-select-box-with-options*/}

רנדרו `<select>` עם רשימת קומפוננטות `<option>` בתוכו כדי להציג תיבת select. תנו לכל `<option>` ערך `value` שמייצג את הנתון שיישלח עם הטופס.

[קראו עוד על הצגת `<select>` עם רשימת קומפוננטות `<option>`. ](/reference/react-dom/components/select)

<Sandpack>

```js
export default function FruitPicker() {
  return (
    <label>
      Pick a fruit:
      <select name="selectedFruit">
        <option value="apple">Apple</option>
        <option value="banana">Banana</option>
        <option value="orange">Orange</option>
      </select>
    </label>
  );
}
```

```css
select { margin: 5px; }
```

</Sandpack>  
