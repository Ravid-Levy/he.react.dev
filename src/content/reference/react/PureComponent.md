---
title: PureComponent
---

<Pitfall>

אנחנו ממליצים להגדיר קומפוננטות כפונקציות במקום classes. [ראו איך לבצע מיגרציה.](#alternatives)

</Pitfall>

<Intro>

`PureComponent` דומה ל-[`Component`](/reference/react/Component) אבל מדלגת על רינדורים חוזרים עבור אותם props ו-state. קומפוננטות class עדיין נתמכות ב-React, אבל אנחנו לא ממליצים להשתמש בהן בקוד חדש.

```js
class Greeting extends PureComponent {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `PureComponent` {/*purecomponent*/}

כדי לדלג על רינדור חוזר של class component עבור אותם props ו-state, הרחיבו את `PureComponent` במקום את [`Component`:](/reference/react/Component)

```js
import { PureComponent } from 'react';

class Greeting extends PureComponent {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}
```

`PureComponent` היא תת-מחלקה של `Component` ותומכת [בכל ה-APIs של `Component`.](/reference/react/Component#reference) הרחבה של `PureComponent` שקולה להגדרת מתודת [`shouldComponentUpdate`](/reference/react/Component#shouldcomponentupdate) מותאמת אישית שמשווה shallow את ה-props וה-state.


[ראו דוגמאות נוספות בהמשך.](#usage)

---

## שימוש {/*usage*/}

### דילוג על רינדורים חוזרים מיותרים עבור class components {/*skipping-unnecessary-re-renders-for-class-components*/}

React בדרך כלל מרנדרת קומפוננטה מחדש בכל פעם שההורה שלה מרונדר מחדש. כאופטימיזציה, אפשר ליצור קומפוננטה ש-React לא תרנדר מחדש כשההורה מרונדר מחדש, כל עוד ה-props וה-state החדשים שלה זהים לישנים. [Class components](/reference/react/Component) יכולות להצטרף להתנהגות הזו על ידי הרחבת `PureComponent`:

```js {1}
class Greeting extends PureComponent {
  render() {
    return <h1>Hello, {this.props.name}!</h1>;
  }
}
```

לקומפוננטת React תמיד צריכה להיות [לוגיקת רינדור טהורה.](/learn/keeping-components-pure) כלומר, היא חייבת להחזיר את אותו פלט אם ה-props, ה-state וה-context לא השתנו. בשימוש ב-`PureComponent` אתם מצהירים בפני React שהקומפוננטה עומדת בדרישה הזו, לכן React לא צריכה לרנדר מחדש כל עוד ה-props וה-state לא השתנו. עם זאת, הקומפוננטה עדיין תרונדר מחדש אם context שבו היא משתמשת משתנה.

בדוגמה הזו שימו לב שקומפוננטת `Greeting` מרונדרת מחדש בכל פעם ש-`name` משתנה (כי זה אחד ה-props שלה), אבל לא כש-`address` משתנה (כי הוא לא מועבר ל-`Greeting` כ-prop):

<Sandpack>

```js
import { PureComponent, useState } from 'react';

class Greeting extends PureComponent {
  render() {
    console.log("Greeting was rendered at", new Date().toLocaleTimeString());
    return <h3>Hello{this.props.name && ', '}{this.props.name}!</h3>;
  }
}

export default function MyApp() {
  const [name, setName] = useState('');
  const [address, setAddress] = useState('');
  return (
    <>
      <label>
        Name{': '}
        <input value={name} onChange={e => setName(e.target.value)} />
      </label>
      <label>
        Address{': '}
        <input value={address} onChange={e => setAddress(e.target.value)} />
      </label>
      <Greeting name={name} />
    </>
  );
}
```

```css
label {
  display: block;
  margin-bottom: 16px;
}
```

</Sandpack>

<Pitfall>

אנחנו ממליצים להגדיר קומפוננטות כפונקציות במקום classes. [ראו איך לבצע מיגרציה.](#alternatives)

</Pitfall>

---

## חלופות {/*alternatives*/}

### מיגרציה מ-`PureComponent` class component לפונקציה {/*migrating-from-a-purecomponent-class-component-to-a-function*/}

אנחנו ממליצים להשתמש בקומפוננטות פונקציה במקום [class components](/reference/react/Component) בקוד חדש. אם יש לכם class components קיימות שמשתמשות ב-`PureComponent`, כך אפשר להמיר אותן. זה הקוד המקורי:

<Sandpack>

```js
import { PureComponent, useState } from 'react';

class Greeting extends PureComponent {
  render() {
    console.log("Greeting was rendered at", new Date().toLocaleTimeString());
    return <h3>Hello{this.props.name && ', '}{this.props.name}!</h3>;
  }
}

export default function MyApp() {
  const [name, setName] = useState('');
  const [address, setAddress] = useState('');
  return (
    <>
      <label>
        Name{': '}
        <input value={name} onChange={e => setName(e.target.value)} />
      </label>
      <label>
        Address{': '}
        <input value={address} onChange={e => setAddress(e.target.value)} />
      </label>
      <Greeting name={name} />
    </>
  );
}
```

```css
label {
  display: block;
  margin-bottom: 16px;
}
```

</Sandpack>

כש-[ממירים את הקומפוננטה הזו מ-class לפונקציה,](/reference/react/Component#alternatives) עטפו אותה ב-[`memo`:](/reference/react/memo)

<Sandpack>

```js
import { memo, useState } from 'react';

const Greeting = memo(function Greeting({ name }) {
  console.log("Greeting was rendered at", new Date().toLocaleTimeString());
  return <h3>Hello{name && ', '}{name}!</h3>;
});

export default function MyApp() {
  const [name, setName] = useState('');
  const [address, setAddress] = useState('');
  return (
    <>
      <label>
        Name{': '}
        <input value={name} onChange={e => setName(e.target.value)} />
      </label>
      <label>
        Address{': '}
        <input value={address} onChange={e => setAddress(e.target.value)} />
      </label>
      <Greeting name={name} />
    </>
  );
}
```

```css
label {
  display: block;
  margin-bottom: 16px;
}
```

</Sandpack>

<Note>

בניגוד ל-`PureComponent`, [`memo`](/reference/react/memo) לא משווה בין state חדש לישן. בקומפוננטות פונקציה, קריאה ל-[פונקציית `set`](/reference/react/useState#setstate) עם אותו state [כבר מונעת רינדורים חוזרים כברירת מחדל,](/reference/react/memo#updating-a-memoized-component-using-state) גם בלי `memo`.

</Note>
