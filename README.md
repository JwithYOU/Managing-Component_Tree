# READ.md

## Challenge

CHALLENGE: I have extracted the Input Area, including the < input > and
< button > elements into a seperate Component called InputArea.
Your job is to make the app work as it did before but this time with the
InputArea as a seperate Component.

---

> InputArea 컴포넌트가 분리 되어 있는데 문제없이 작동하도록 바꾸는 것이 관건

```js
//App.jsx
import React, { useState } from "react";
import ToDoItem from "./ToDoItem";
import InputArea from "./InputArea";

function App() {
  const [inputText, setInputText] = useState("");
  const [items, setItems] = useState([]);

  function handleChange(event) {
    const newValue = event.target.value;
    setInputText(newValue);
  }

  function addItem() {
    setItems((prevItems) => {
      return [...prevItems, inputText];
    });
    setInputText("");
  }

  function deleteItem(id) {
    setItems((prevItems) => {
      return prevItems.filter((item, index) => {
        return index !== id;
      });
    });
  }

  return (
    <div className="container">
      <div className="heading">
        <h1>To-Do List</h1>
      </div>
      <InputArea />
      <div>
        <ul>
          {items.map((todoItem, index) => (
            <ToDoItem
              key={index}
              id={index}
              text={todoItem}
              onChecked={deleteItem}
            />
          ))}
        </ul>
      </div>
    </div>
  );
}

export default App;
```

```js
// InputArea.jsx
import React from "react";
function InputArea() {
  return (
    <div className="form">
      <input onChange={handleChange} type="text" value={inputText} />
      <button onClick={addItem}>
        <span>Add</span>
      </button>
    </div>
  );
}
export default InputArea;
```

> App.jsx에서 선언 되있는 함수들을 props로 넘기는게 아닌 일부는 InputArea로 옮기고 나머지는 props로 전달한다
> 관련있는 함수는 addItem() 과 handleChange() 인데 handleChange() 와 [inputText, setInputText]는 InputArea로 보내고 addItem()는 props로 전달한다.

```js
// InputArea.jsx
import React from "react";
function InputArea() {
  // [inputText, setInputText] 추가
  const [inputText, setInputText] = useState("");

  // handleChange() 추가
  function handleChange(event) {
    const newValue = event.target.value;
    setInputText(newValue);
  }

  return (
    <div className="form">
      <input onChange={handleChange} type="text" value={inputText} />
      <button onClick={addItem}>
        <span>Add</span>
      </button>
    </div>
  );
}
export default InputArea;
```

> addItem()은 아래와 같이 props로 전달하고 InputArea에서 onClick으로 추가하고 실행시 onClick과 무관하게 작동되는 것을 방지하기 위해서 화살표 함수로 작성

```js
<InputArea onAdd={addItem} />
```

```js
// InputArea.jsx
import React from "react";
//props로 받아오기
function InputArea(props) {
  const [inputText, setInputText] = useState("");

  function handleChange(event) {
    const newValue = event.target.value;
    setInputText(newValue);
  }
  // props로 전달된 addItem 전달하고 화살표 함수로 작성
  // Click 이벤트 후에 input안에를 비워주기 위해서 setInputText("")를 추가
  return (
    <div className="form">
      <input onChange={handleChange} type="text" value={inputText} />
      <button
        onClick={() => {
          props.onAdd(inputText);
          setInputText("");
        }}
      >
        <span>Add</span>
      </button>
    </div>
  );
}
```
