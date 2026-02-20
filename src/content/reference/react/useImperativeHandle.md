---
title: useImperativeHandle
---

<Intro>

`useImperativeHandle` הוא React Hook שמאפשר להתאים אישית את ה-handle שנחשף כ-[ref.](/learn/manipulating-the-dom-with-refs)

```js
useImperativeHandle(ref, createHandle, dependencies?)
```

</Intro>

<InlineToc />

---

## Reference {/*reference*/}

### `useImperativeHandle(ref, createHandle, dependencies?)` {/*useimperativehandle*/}

קראו ל-`useImperativeHandle` ברמה העליונה של הקומפוננטה כדי להתאים אישית את ref handle שהיא חושפת:

```js
import { forwardRef, useImperativeHandle } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  useImperativeHandle(ref, () => {
    return {
      // ... your methods ...
    };
  }, []);
  // ...
```

[ראו דוגמאות נוספות בהמשך.](#usage)

#### Parameters {/*parameters*/}

* `ref`: ה-`ref` שקיבלתם כארגומנט השני מתוך [`forwardRef` render function.](/reference/react/forwardRef#render-function)

* `createHandle`: פונקציה שלא מקבלת ארגומנטים ומחזירה את ref handle שתרצו לחשוף. ה-handle הזה יכול להיות מכל סוג. בדרך כלל תחזירו אובייקט עם המתודות שתרצו לחשוף.

* **אופציונלי** `dependencies`: רשימה של כל הערכים הריאקטיביים שמופנים בתוך קוד ה-`createHandle`. ערכים ריאקטיביים כוללים props, state וכל המשתנים והפונקציות שמוגדרים ישירות בתוך גוף הקומפוננטה. אם ה-linter שלכם [מוגדר ל-React](/learn/editor-setup#linting), הוא יוודא שכל ערך ריאקטיבי מצוין נכון כ-dependency. רשימת ה-dependencies חייבת להיות במספר פריטים קבוע ולהיכתב inline כמו `[dep1, dep2, dep3]`. React תשווה כל dependency לערך הקודם שלו באמצעות [`Object.is`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/is). אם re-render גרם לשינוי ב-dependency כלשהו, או אם השמטתם את הארגומנט הזה, פונקציית `createHandle` תרוץ מחדש, וה-handle החדש שייווצר יוקצה ל-ref.

#### Returns {/*returns*/}

`useImperativeHandle` מחזיר `undefined`.

---

## שימוש {/*usage*/}

### חשיפת ref handle מותאם לקומפוננטת ההורה {/*exposing-a-custom-ref-handle-to-the-parent-component*/}

כברירת מחדל, קומפוננטות לא חושפות את ה-DOM nodes שלהן לקומפוננטות הורה. למשל, אם אתם רוצים שלקומפוננטת ההורה של `MyInput` תהיה [גישה](/learn/manipulating-the-dom-with-refs) ל-`<input>` DOM node, צריך לבצע opt-in עם [`forwardRef`:](/reference/react/forwardRef)

```js {4}
import { forwardRef } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  return <input {...props} ref={ref} />;
});
```

עם הקוד למעלה, [ref ל-`MyInput` יקבל את `<input>` DOM node.](/reference/react/forwardRef#exposing-a-dom-node-to-the-parent-component) אבל אפשר גם לחשוף ערך מותאם אישית במקום. כדי להתאים את ה-handle שנחשף, קראו ל-`useImperativeHandle` ברמה העליונה של הקומפוננטה:

```js {4-8}
import { forwardRef, useImperativeHandle } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  useImperativeHandle(ref, () => {
    return {
      // ... your methods ...
    };
  }, []);

  return <input {...props} />;
});
```

שימו לב שבקוד למעלה, ה-`ref` כבר לא מועבר ל-`<input>`.

לדוגמה, נניח שאתם לא רוצים לחשוף את כל `<input>` DOM node, אלא רק שתי מתודות: `focus` ו-`scrollIntoView`. כדי לעשות זאת, שמרו את ה-DOM האמיתי של הדפדפן ב-ref נפרד. אחר כך השתמשו ב-`useImperativeHandle` כדי לחשוף handle שמכיל רק את המתודות שאתם רוצים שההורה יוכל לקרוא להן:

```js {7-14}
import { forwardRef, useRef, useImperativeHandle } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  const inputRef = useRef(null);

  useImperativeHandle(ref, () => {
    return {
      focus() {
        inputRef.current.focus();
      },
      scrollIntoView() {
        inputRef.current.scrollIntoView();
      },
    };
  }, []);

  return <input {...props} ref={inputRef} />;
});
```

עכשיו, אם קומפוננטת ההורה מקבלת ref ל-`MyInput`, היא תוכל לקרוא למתודות `focus` ו-`scrollIntoView`. עם זאת, לא תהיה לה גישה מלאה ל-`<input>` DOM node עצמו.

<Sandpack>

```js
import { useRef } from 'react';
import MyInput from './MyInput.js';

export default function Form() {
  const ref = useRef(null);

  function handleClick() {
    ref.current.focus();
    // This won't work because the DOM node isn't exposed:
    // ref.current.style.opacity = 0.5;
  }

  return (
    <form>
      <MyInput placeholder="Enter your name" ref={ref} />
      <button type="button" onClick={handleClick}>
        Edit
      </button>
    </form>
  );
}
```

```js src/MyInput.js
import { forwardRef, useRef, useImperativeHandle } from 'react';

const MyInput = forwardRef(function MyInput(props, ref) {
  const inputRef = useRef(null);

  useImperativeHandle(ref, () => {
    return {
      focus() {
        inputRef.current.focus();
      },
      scrollIntoView() {
        inputRef.current.scrollIntoView();
      },
    };
  }, []);

  return <input {...props} ref={inputRef} />;
});

export default MyInput;
```

```css
input {
  margin: 5px;
}
```

</Sandpack>

---

### חשיפת מתודות imperative משלכם {/*exposing-your-own-imperative-methods*/}

המתודות שאתם חושפים דרך imperative handle לא חייבות להתאים בדיוק למתודות DOM. לדוגמה, קומפוננטת `Post` הזו חושפת מתודה בשם `scrollAndFocusAddComment` דרך imperative handle. זה מאפשר ל-`Page` ההורה לגלול את רשימת התגובות *וגם* לפקס את שדה הקלט כשאתם לוחצים על הכפתור:

<Sandpack>

```js
import { useRef } from 'react';
import Post from './Post.js';

export default function Page() {
  const postRef = useRef(null);

  function handleClick() {
    postRef.current.scrollAndFocusAddComment();
  }

  return (
    <>
      <button onClick={handleClick}>
        Write a comment
      </button>
      <Post ref={postRef} />
    </>
  );
}
```

```js src/Post.js
import { forwardRef, useRef, useImperativeHandle } from 'react';
import CommentList from './CommentList.js';
import AddComment from './AddComment.js';

const Post = forwardRef((props, ref) => {
  const commentsRef = useRef(null);
  const addCommentRef = useRef(null);

  useImperativeHandle(ref, () => {
    return {
      scrollAndFocusAddComment() {
        commentsRef.current.scrollToBottom();
        addCommentRef.current.focus();
      }
    };
  }, []);

  return (
    <>
      <article>
        <p>Welcome to my blog!</p>
      </article>
      <CommentList ref={commentsRef} />
      <AddComment ref={addCommentRef} />
    </>
  );
});

export default Post;
```


```js src/CommentList.js
import { forwardRef, useRef, useImperativeHandle } from 'react';

const CommentList = forwardRef(function CommentList(props, ref) {
  const divRef = useRef(null);

  useImperativeHandle(ref, () => {
    return {
      scrollToBottom() {
        const node = divRef.current;
        node.scrollTop = node.scrollHeight;
      }
    };
  }, []);

  let comments = [];
  for (let i = 0; i < 50; i++) {
    comments.push(<p key={i}>Comment #{i}</p>);
  }

  return (
    <div className="CommentList" ref={divRef}>
      {comments}
    </div>
  );
});

export default CommentList;
```

```js src/AddComment.js
import { forwardRef, useRef, useImperativeHandle } from 'react';

const AddComment = forwardRef(function AddComment(props, ref) {
  return <input placeholder="Add comment..." ref={ref} />;
});

export default AddComment;
```

```css
.CommentList {
  height: 100px;
  overflow: scroll;
  border: 1px solid black;
  margin-top: 20px;
  margin-bottom: 20px;
}
```

</Sandpack>

<Pitfall>

**אל תשתמשו ב-refs מעבר לנדרש.** כדאי להשתמש ב-refs רק להתנהגויות *imperative* שלא ניתן לבטא כ-props: למשל גלילה ל-node, פוקוס ל-node, הפעלת אנימציה, בחירת טקסט, וכן הלאה.

**אם אפשר לבטא משהו כ-prop, לא כדאי להשתמש ב-ref.** לדוגמה, במקום לחשוף imperative handle כמו `{ open, close }` מתוך קומפוננטת `Modal`, עדיף לקבל `isOpen` כ-prop כמו `<Modal isOpen={isOpen} />`. [Effects](/learn/synchronizing-with-effects) יכולות לעזור לחשוף התנהגויות imperative דרך props.

</Pitfall>
