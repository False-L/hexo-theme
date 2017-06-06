---
title: use localstorage in react
description: 这是一篇渣翻译文档，找了一篇比较好理解的翻译了一下
date: 2017-05-12 16:57:57
categories: [技术]
tags:
- react
- localStorage
- sessionStorage
---
# Local Store in React
原文链接：https://www.robinwieruch.de/local-storage-react/

## 
在阅读the Road to learn React 中，有一些读者向我提了一个问题：如何在react中保持state？(注：如登录密码等)   
显然，通过后台将state在数据库中持久化是可能的。当app启动之后，react应用会向后端发起请求来取回state。之后取回的state将被存储到本地组件的state中或者通过一个state管理库如Redux、MobX来管理。但是一个更简单并且大部分时间足够的方法是使用浏览器本地local storage 。无后端也不需要额外的库。   
本文将为您展示如何在本地存储的React中保持状态，如何将其用作缓存以及如何使其过期。
## 创建react项目
你可以使用creact-react-app或者其他react脚手架来创建react项目，我推荐使用creact-react-app。   
因为我们将使用fetch API来从第三方应用检索数据，所以你应该确定你得浏览器支持fetch。否则你将需要工具去支持它。这是我推荐使用creact-react-app的原因。   
在你创建你的项目之后，你的项目样板和下面一样：
```javascript
import React from 'react';

class App extends React.Component {

  constructor(props) {
    super(props);
    this.state = { hits: null };
  }

  onSearch = (e) => {
    e.preventDefault();

    const { value } = this.input;

    if (value === '') {
      return;
    }

    fetch('https://hn.algolia.com/api/v1/search?query=' + value)
      .then(response => response.json())
      .then(result => this.onSetResult(result));
  }

  onSetResult = (result) => {
    this.setState({ hits: result.hits });
  }

  render() {
    return (
      <div>
        <h1>Search Hacker News with Local Storage</h1>
        <p>There shouldn't be a second network request, when you search for something twice.</p>

        <form type="submit" onSubmit={this.onSearch}>
          <input type="text" ref={node => this.input = node} />
          <button type="button">Search</button>
        </form>

        {
          this.state.hits &&
          this.state.hits.map(item => <div key={item.objectID}>{item.title}</div>)
        }
      </div>
    );
  }
}

export default App;
```
如果你理解上面的所有代码，你可以转到下一个标题。不然你可以阅读下快速概括。   
在render方法中你将找到一个使用了ref属性的表格和一个数组（注：map()）,在这个比较直接的方法中，你可以尝试下react中的单向数据流。<code>onSubmit</code>的回调会调用<code>onSearch()</code>方法。当input中什么都没有时，<code>onSearch()</code>方法什么也不干。但是当input中不为空时，它将会向Hacker News API发起一个请求。在请求成功后，<code>onSearch()</code>方法会通过<code>setState()</code>将结果存储到本地组件的state中。(注：react的基本知识)   
<code>onSearch()</code>方法是通过fetch方法来请求api，当然你也可以用其他第三方方法如axios或者superagent发起同样的请求。这是使React如此强大的事情之一，因为可以灵活选择的构建组件。   
## 介绍Local Storage
Local Storage被大多数浏览器锁支持。你可以检测浏览器兼容性并且在官方文档中阅读更多相关知识。   
 local storage的使用方法非常直接。在你的浏览器中运行你的js代码，你需要访问LocalStorage对象，该对象有一个setter和getter来存储和检索对象中的数据。
 ```javascript
 // setter
localStorage.setItem('myData', data);

// getter
localStorage.getItem('myData');
 ```
 当你关闭浏览器并再次打开应用程序时，您会发现数据仍在对象中。
## 在react中将LocalStorage作为Cache使用
现在你可以在你的应用中添加几行代码就可以使用LocalStorage了，即使你关掉你的浏览器再打开，你还是能得到被存储的结果。
```javascript
import React from 'react';

class App extends React.Component {

  constructor(props) {
    super(props);
    this.state = { hits: null };
  }

  onSearch = (e) => {
    e.preventDefault();

    const { value } = this.input;

    if (value === '') {
      return;
    }
/*****添加的代码****/
    const cachedHits = localStorage.getItem(value);
    if (cachedHits) {
      this.setState({ hits: JSON.parse(cachedHits) });
      return;
    }
 /*******/
    fetch('https://hn.algolia.com/api/v1/search?query=' + value)
      .then(response => response.json())
      /*****添加的代码****/
      .then(result => this.onSetResult(result, value));
      /*******/
  }

/*****添加的代码****/
  onSetResult = (result, key) => {
    localStorage.setItem(key, JSON.stringify(result.hits));
     /*******/
    this.setState({ hits: result.hits });
  }

  render() {
    return (
      <div>
        <h1>Search Hacker News with Local Storage</h1>
        <p>There shouldn't be a second network request, when you search for something twice.</p>

        <form type="submit" onSubmit={this.onSearch}>
          <input type="text" ref={node => this.input = node} />
          <button type="button">Search</button>
        </form>

        {
          this.state.hits &&
          this.state.hits.map(item => <div key={item.objectID}>{item.title}</div>)
        }
      </div>
    );
  }
}

export default App;
```
不应该对Api请求两次，因为结果应该被缓存。如果有cachedHits，则该方法返回较早结果，而不发起请求。在缓存中的结果是一个js对象，并且当需要在storage中被检索时，被存储被解析时，需要字符串化。
There shouldn’t be a request to the API made twice, because the result should be cached. If there are cachedHits, the method returns earlier without performing a request. The result in the cache is an JavaScript object and thus needs to be stringified when stored and parsed when retrieved from the storage.
## 会话存储过期 Expiration with Session Storage
有时候你希望缓存只存在当前会话中。当关闭浏览器时，你希望缓存被清空。那就可以使用本机sessionStorage来代替localStorage。
sessionStorage的使用方法和localStorage一样：
```javascript
// setter
sessionStorage.setItem('myData', data);

// getter
sessionStorage.getItem('myData');
```
在用户登录到您的应用程序之后处理用户会话时，这可能很有用。登录会话可以保存，直到浏览器关闭。
作者的源码https://github.com/rwieruch/react-local-storage


