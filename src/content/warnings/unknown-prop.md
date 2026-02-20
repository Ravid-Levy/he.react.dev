---
title: אזהרת Unknown Prop
---

האזהרה unknown-prop תופיע אם תנסו לרנדר אלמנט DOM עם prop ש-React לא מזהה כ-DOM attribute/property חוקי. חשוב לוודא שלאלמנטים שלכם אין props מיותרים שמועברים בטעות.

יש כמה סיבות נפוצות לאזהרה הזו:

1. האם אתם משתמשים ב-`{...props}` או `cloneElement(element, props)`? כשמעתיקים props לקומפוננטת child, צריך לוודא שלא מעבירים בטעות props שנועדו רק לקומפוננטת parent. בהמשך מופיעים תיקונים נפוצים לבעיה הזו.

2. אתם משתמשים ב-DOM attribute לא סטנדרטי על DOM node רגיל, אולי כדי לייצג נתונים מותאמים אישית. אם המטרה היא להצמיד נתונים מותאמים לאלמנט DOM סטנדרטי, כדאי להשתמש ב-custom data attribute כפי שמוסבר [ב-MDN](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Using_data_attributes).

3. React עדיין לא מזהה את המאפיין שציינתם. סביר שזה יתוקן בגרסה עתידית של React. React תאפשר להעביר אותו בלי אזהרה אם תכתבו את שם המאפיין באותיות קטנות.

4. אתם משתמשים בקומפוננטת React בלי אות גדולה בתחילת השם, למשל `<myButton />`. React מפרשת את זה כתגית DOM כי ה-JSX transform של React משתמש בכלל אותיות גדולות/קטנות כדי להבדיל בין קומפוננטות מוגדרות-משתמש לבין תגיות DOM. לקומפוננטות שלכם השתמשו ב-PascalCase. לדוגמה, כתבו `<MyButton />` במקום `<myButton />`.

---

אם קיבלתם את האזהרה כי העברתם props כמו `{...props}`, קומפוננטת ה-parent צריכה "לצרוך" כל prop שמיועד ל-parent ולא ל-child. דוגמה:

**Bad:** ה-prop הלא צפוי `layout` מועבר לתגית `div`.

```js
function MyDiv(props) {
  if (props.layout === 'horizontal') {
    // BAD! Because you know for sure "layout" is not a prop that <div> understands.
    return <div {...props} style={getHorizontalStyle()} />
  } else {
    // BAD! Because you know for sure "layout" is not a prop that <div> understands.
    return <div {...props} style={getVerticalStyle()} />
  }
}
```

**Good:** אפשר להשתמש ב-spread syntax כדי לחלץ משתנים מ-props, ולהכניס את שאר ה-props למשתנה נפרד.

```js
function MyDiv(props) {
  const { layout, ...rest } = props
  if (layout === 'horizontal') {
    return <div {...rest} style={getHorizontalStyle()} />
  } else {
    return <div {...rest} style={getVerticalStyle()} />
  }
}
```

**Good:** אפשר גם להעתיק את props לאובייקט חדש ולמחוק ממנו את המפתחות שבהם משתמשים. חשוב לא למחוק props מהאובייקט המקורי `this.props`, כי צריך להתייחס אליו כ-immutable.

```js
function MyDiv(props) {
  const divProps = Object.assign({}, props);
  delete divProps.layout;

  if (props.layout === 'horizontal') {
    return <div {...divProps} style={getHorizontalStyle()} />
  } else {
    return <div {...divProps} style={getVerticalStyle()} />
  }
}
```
