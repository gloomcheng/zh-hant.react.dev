---
title: 快速開始
---

<Intro>

歡迎來到 React 教學文件！本章節所介紹的概念，將涵蓋 React 日常會使用功能的 80%。

</Intro>

<YouWillLearn>

- 如何建立單層與巢狀元件（component）
- 如何加入標記語言和樣式
- 如何顯示資料
- 如何使用條件式和列表渲染結果
- 如何回應事件和更新畫面
- 如何在元件之間共用資料

</YouWillLearn>

## 建立單層和巢狀元件 {/*components*/}

React 應用程式是由許多 **元件** 組成的。元件是 UI（使用者介面）的一部分，它具有自己的程式邏輯和外觀樣式。一個元件可以是小到只有一個按鈕，也可以大到包含一整個頁面。

React　元件具體是由 JavaScript 函式定義的，並回傳標記語言：

```js
function MyButton() {
  return (
    <button>I'm a button</button>
  );
}
```

現在你已經定義了 `MyButton` 元件，你可以將它嵌入另一個元件中：

```js {5}
export default function MyApp() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton />
    </div>
  );
}
```

你可能已經注意到 `<MyButton />` 是以大寫字母開頭的，你可以據此識別出 React 元件。React 元件必須以大寫字母開頭，而 HTML 標記語言則必須為小寫字母。

來看看範例程式的執行結果：

<Sandpack>

```js
function MyButton() {
  return (
    <button>
      I'm a button
    </button>
  );
}

export default function MyApp() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton />
    </div>
  );
}
```

</Sandpack>

`export default` 關鍵字用來指定這份檔案中的主要元件。如果你對 JavaScript 的語法還不熟悉，可以參考 [MDN](https://developer.mozilla.org/en-US/docs/web/javascript/reference/statements/export) 和 [javascript.info](https://javascript.info/import-export)。

## 使用 JSX 撰寫標記語言 {/*writing-markup-with-jsx*/}

上面使用來撰寫標記語言的語法稱為 *JSX*。你可以不用 JSX 撰寫標記語言，但大多數的 React 專案都會使用 JSX，主要是它很方便。所有[我們推薦的本機開發工具](/learn/installation)都預設支援 JSX。

JSX 比 HTML 更嚴格。你必須正確地封閉標記，像是 `<br />`。你的元件不能回傳多個 JSX 標記，你必須將它們包裹在共同的父標記中，例如使用 `<div>...</div>` 或空的 `<>...</>` 包裏：

```js {3,6}
function AboutPage() {
  return (
    <>
      <h1>About</h1>
      <p>Hello there.<br />How do you do?</p>
    </>
  );
}
```

如果你有大量的 HTML 要移植到 JSX，你可以使用[線上轉換](https://transform.tools/html-to-jsx)工具。

## 加上樣式 {/*adding-styles*/}

在 React 中，你可以使用 `className` 來指定 CSS 的 class，它與 HTML 的 [`class`](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/class) 屬性的工作方式相同：

```js
<img className="avatar" />
```

接著，你可以在某個 CSS 文件中撰寫對應的 CSS 規則：

```css
/* In your CSS */
.avatar {
  border-radius: 50%;
}
```

React 沒有規定你應該如何加入 CSS 文件。最簡單的方式是使用 HTML 的 [`<link>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link) 標籤引入 CSS 文件。如果你使用構建工具（build tool）或框架開發你的程式，請查閱那些文件以了解如何將 CSS 文件加到你的專案中。

## 顯示資料 {/*displaying-data*/}

JSX 能讓你將標記語言放入 JavaScript 程式中。而大括號能讓你「切換回 JavaScript」，好讓你能夠在你的程式碼中嵌入變數，並將其顯示給使用者看。例如，以下範例將顯示 `user.name`　變數的內容：

```js {3}
return (
  <h1>
    {user.name}
  </h1>
);
```

你也可以在 JSX 屬性「切換回 JavaScript」，但你必須使用大括號*而非*引號。舉例來說，`className="avatar"` 中會傳遞 `"avatar"` 字串作為 CSS class，而 `src={user.imageUrl}` 則是讀取 JavaScript 中 `user.imageUrl` 的變數值，並將該值作為 `src` 的屬性值傳遞：

```js {3,4}
return (
  <img
    className="avatar"
    src={user.imageUrl}
  />
);
```

你可以將更複雜的 JavaScript 表達式放入 JSX 大括號中，例如[字串串接](https://javascript.info/operators#string-concatenation-with-binary)。

<Sandpack>

```js
const user = {
  name: 'Hedy Lamarr',
  imageUrl: 'https://i.imgur.com/yXOvdOSs.jpg',
  imageSize: 90,
};

export default function Profile() {
  return (
    <>
      <h1>{user.name}</h1>
      <img
        className="avatar"
        src={user.imageUrl}
        alt={'Photo of ' + user.name}
        style={{
          width: user.imageSize,
          height: user.imageSize
        }}
      />
    </>
  );
}
```

```css
.avatar {
  border-radius: 50%;
}

.large {
  border: 4px solid gold;
}
```

</Sandpack>

在上述範例中，`style={{}}` 並非特殊語法，而是在 `style={ }` JSX 大括號中，放入一個 `{}` 物件。當你的樣式取決於 JavaScript 變數時，可以使用 `style` 屬性。

## 條件式渲染 {/*conditional-rendering*/}

在 React 中，沒有特殊的語法可以撰寫條件判斷。因此你使用的就是與撰寫一般 JavaScript 程式碼相同的技巧來撰寫條件判斷。例如，你可以使用 [`if`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else) 條件判斷來包含 JSX：

```js
let content;
if (isLoggedIn) {
  content = <AdminPanel />;
} else {
  content = <LoginForm />;
}
return (
  <div>
    {content}
  </div>
);
```

如果你偏好撰寫更緊湊的程式碼，你可以使用 [`?` 條件運算子](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)。與 `if` 不同的是，它可以在 JSX 內部運作：

```js
<div>
  {isLoggedIn ? (
    <AdminPanel />
  ) : (
    <LoginForm />
  )}
</div>
```

當你不需要 `else` 分支時，你也可以使用更簡短的 [`&&` 邏輯語法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND#short-circuit_evaluation)：

```js
<div>
  {isLoggedIn && <AdminPanel />}
</div>
```

這些方法也適用於有條件地指定屬性。當然，如果你對這些 JavaScript 語法不熟悉，你也可以使用 `if...else` 來撰寫條件判斷邏輯即可。

## 渲染列表 {/*rendering-lists*/}

你需要使用 JavaScript 的 [`for` loop](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for) 語法和 [array `map()` 函式](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)來渲染列表的元件。

例如，你有一個產品的列表如下：

```js
const products = [
  { title: 'Cabbage', id: 1 },
  { title: 'Garlic', id: 2 },
  { title: 'Apple', id: 3 },
];
```

在你的元件中，你需要使用 `map()` 函式將產品這個陣列（array）轉換成一系列 `<li>` 標記的列表項目：

```js
const listItems = products.map(product =>
  <li key={product.id}>
    {product.title}
  </li>
);

return (
  <ul>{listItems}</ul>
);
```

請注意 `<li>` 有一個 `key` 屬性。對於列表中的每個項目，你都應該傳遞一個字串或數字作為 `key` 值，用於在其兄弟元素中可以唯一識別該項目。通常，`key` 值應該來自於你的資料，例如資料庫的 ID。React 使用這些 `key` 值來了解後續的插入、刪除或重新排序項目時所發生的變化。

<Sandpack>

```js
const products = [
  { title: 'Cabbage', isFruit: false, id: 1 },
  { title: 'Garlic', isFruit: false, id: 2 },
  { title: 'Apple', isFruit: true, id: 3 },
];

export default function ShoppingList() {
  const listItems = products.map(product =>
    <li
      key={product.id}
      style={{
        color: product.isFruit ? 'magenta' : 'darkgreen'
      }}
    >
      {product.title}
    </li>
  );

  return (
    <ul>{listItems}</ul>
  );
}
```

</Sandpack>

## 事件回應 {/*responding-to-events*/}

你可以透過在元件中宣告 **事件處理** 函式來回應事件：

```js {2-4,7}
function MyButton() {
  function handleClick() {
    alert('You clicked me!');
  }

  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );
}
```

注意 `onClick={handleClick}` 的結尾沒有小括號！不要 **呼叫** 事件處理函式：你只需要 **將函式傳遞給事件** 即可。當使用者點擊按鈕時，React 將呼叫你的事件處理函式。

## 更新畫面 {/*updating-the-screen*/}

有時候，你想要元件能「記住」一些資訊並呈現出來。例如，你想要計算按鈕被按過的次數。要達成這個目標，你需要在你的元件中加入 **state**。

首先，從 React 引入 [`useState`](/reference/react/useState)：

```js
import { useState } from 'react';
```

接著你可以在元件內宣告一個 **state 變數**：

```js
function MyButton() {
  const [count, setCount] = useState(0);
  // ...
```

你會從 `useState` 得到兩個東西：目前的 state（`count`），以及用於更新 state 的函式（`setCount`）。你可以給它們取任何的名稱，但通常慣例會用類似 `[something, setSomething]`　這樣的名稱來命名。

第一次顯示按鈕時，`count` 的值為 `0`，因為你在程式碼內將 `0` 傳遞給 `useState()`。 當你想改變 state 時，呼叫 `setCount()` 函式並將新的值傳遞給它。點擊此按鈕後，計數器將遞增其數值：

```js {5}
function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Clicked {count} times
    </button>
  );
}
```

每次點擊按鈕，React 都會再次呼叫你的元件函式。第一次點擊，`count` 會變成 `1`，繼續點擊就變成 `2`，以此類推。

如果你重複渲染相同的元件，那麼每個元件都會有自己的 state。你可以分別點擊各個按鈕試試：

<Sandpack>

```js
import { useState } from 'react';

export default function MyApp() {
  return (
    <div>
      <h1>Counters that update separately</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Clicked {count} times
    </button>
  );
}
```

```css
button {
  display: block;
  margin-bottom: 5px;
}
```

</Sandpack>

注意，每個按鈕會「記住」自己的 `count` state，並且不影響到其他的按鈕。

## 使用 Hook {/*using-hooks*/}

以 `use` 開頭的函式稱為 **Hook**。`useState` 是 React 內建提供的其中一個 Hook。你可以在 [API 文件](/reference/react)中找到其他內建的 Hook。你也可以透過組合既有的 Hook 來撰寫屬於你自己的 Hook。

與一般的函式不同，使用 Hook 有些特殊的限制。你只能在你的元件（或其他的 Hook）的 **頂部** 呼叫 Hook。如果你想在條件判斷或迴圈內使用 `useState`，請建立一個新的元件，並將你要使用的 Hook 放在那個元件裡。

## 在元件之間共用資料 {/*sharing-data-between-components*/}

在先前的範例中，每個 `MyButton` 都有自己獨立的 `count`，當點擊某個按鈕時，只有該按鈕的 `count` 會發生變化：

<DiagramGroup>

<Diagram name="sharing_data_child" height={367} width={407} alt="Diagram showing a tree of three components, one parent labeled MyApp and two children labeled MyButton. Both MyButton components contain a count with value zero.">

最初每個 `MyButton` 的 `count` state 都是 `0`

</Diagram>

<Diagram name="sharing_data_child_clicked" height={367} width={407} alt="The same diagram as the previous, with the count of the first child MyButton component highlighted indicating a click with the count value incremented to one. The second MyButton component still contains value zero." >

第一個 `MyButton` 將 `count` 更新為 `1`

</Diagram>

</DiagramGroup>

但是，你通常會需要元件間可以 **共用資料並同步更新顯示內容**。

要讓兩個 `MyButton` 元件顯示相同的 `count` 並一起更新結果，你需要將 state 從各個按鈕拿掉，並「向上」移動到最接近包含所有按鈕的元件之中。

在這個範例中，這個最接近且包含所有按鈕的元件是 `MyApp`：

<DiagramGroup>

<Diagram name="sharing_data_parent" height={385} width={410} alt="Diagram showing a tree of three components, one parent labeled MyApp and two children labeled MyButton. MyApp contains a count value of zero which is passed down to both of the MyButton components, which also show value zero." >

一開始，`MyApp` 的 `count` state 為 `0` 並傳遞給兩個子元件

</Diagram>

<Diagram name="sharing_data_parent_clicked" height={385} width={410} alt="The same diagram as the previous, with the count of the parent MyApp component highlighted indicating a click with the value incremented to one. The flow to both of the children MyButton components is also highlighted, and the count value in each child is set to one indicating the value was passed down." >

點擊按鈕後，`MyApp` 將 `count` state 更新為 `1` 並將其傳遞給兩個子元件

</Diagram>

</DiagramGroup>

現在，不論點擊哪個按鈕，`MyApp` 中的 `count` 都將發生變化，並同時更改兩個 `MyButton` 中的計數結果。具體的程式碼寫法如下。

首先，將 `MyButton` 的 **state 上移** 到 `MyApp`：

```js {2-6,18}
export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Counters that update separately</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  // ... we're moving code from here ...
}

```

接著，將 `MyApp` 中的點擊事件處理函數，以及 **state 一同向下傳遞到** 每個 `MyButton` 中。你可以使用 JSX 的大括號傳遞函數到 `MyButton`，就像先前對 `<img>` 標記傳遞變數的範例一樣：

```js {11-12}
export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Counters that update together</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}
```

使用這種方式傳遞的資訊被稱為 **props**。現在 `MyApp` 元件包含了 `count` state 和 `handleClick` 事件處理函式，並將它們 **作為 props 傳遞** 給每個按鈕。

最後，再改寫一下 `MyButton` 變成可以 **讀取** 從父元件傳遞來的 props：

```js {1,3}
function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      Clicked {count} times
    </button>
  );
}
```

當你點擊按鈕時，將觸發 `onClick` 事件。每個按鈕的 `onClick` prop 都設定為 `MyApp` 中的 `handleClick` 函式，所以函式內的程式碼就會被執行。該段程式碼呼叫 `setCount(count + 1)`，使得 `count` state 變數遞增。接著，新的 `count` 值會被作為 prop 傳遞給每個按鈕，因此每個按鈕就會顯示最新的 `count` 值。這種技巧稱為「lifting state up」。通過向上移動 state 到父元件的做法，我們實現了在組件內共用資料的目標。

<Sandpack>

```js
import { useState } from 'react';

export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Counters that update together</h1>
      <MyButton count={count} onClick={handleClick} />
      <MyButton count={count} onClick={handleClick} />
    </div>
  );
}

function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      Clicked {count} times
    </button>
  );
}
```

```css
button {
  display: block;
  margin-bottom: 5px;
}
```

</Sandpack>

## 下一步 {/*next-steps*/}

到目前為止，你已經學到如何撰寫 React 程式碼的基本知識了！

接下來，你可以繼續閱讀[教程](/learn/tutorial-tic-tac-toe)，嘗試將所學付諸實踐，用 React 建構你的第一個應用程式。
