# React-setState五大问

## 第一个例子——普通

```jsx
import React, { Component } from 'react';

export default class App extends Component {
  state = {
    count: 0
  };

  componentWillMount() {
    this.setState({
      count: this.state.count + 2
    });
    this.setState({
      count: this.state.count + 1
    });
  }

  componentDidMount() {
    this.setState({
      count: this.state.count + 2
    });
    this.setState({
      count: this.state.count + 1
    });
  }

  onClick = () => {
    console.log('click后：')
    this.setState({
      count: this.state.count + 2
    });
    this.setState({
      count: this.state.count + 1
    });
  }

  render() {
    console.log('rendered', `count = ${this.state.count}`);

    return (
      <div>
        <h2>setState - normal</h2>
        <p>Count：{this.state.count}</p>
        <button onClick={this.onClick}>Click me</button>
      </div>
    )
  }
}
```

* 结果：

```console
> rendered count = 1
> rendered count = 2
> click后：
> rendered count = 3
```

* WHY: setState的批量更新

## 第二个例子

```jsx
import React, { Component } from 'react';

export default class App extends Component {
  state = {
    count: 0
  };
  componentWillMount() {
    setTimeout(() => {
      this.setState({
        count: this.state.count + 1
      });
      this.setState({
        count: this.state.count + 1
      });
    }, 0);
  }
  componentDidMount() {
    setTimeout(() => {
      this.setState({
        count: this.state.count + 1
      });
      this.setState({
        count: this.state.count + 1
      });
    }, 0);
  }
  onClick = () => {
    console.log('click后：')
    setTimeout(() => {
      this.setState({
        count: this.state.count + 1
      });
      this.setState({
        count: this.state.count + 1
      });
    }, 0);
  }
  render() {
    console.log('rendered', `count = ${this.state.count}`);

    return (
      <div>
        <h2>setState - setTimeout</h2>
        <p>Count：{this.state.count}</p>
        <button onClick={this.onClick}>Click me</button>
      </div>
    )
  }
}
```

* 结果：

```console
> rendered count = 0
> rendered count = 1
> rendered count = 2
> rendered count = 3
> rendered count = 4
> click后：
> rendered count = 5
> rendered count = 6
```

* WHY: setTimeout没有走到react时间的批量更新里

## 第三个例子

```jsx
import React, { Component } from 'react';

export default class App extends Component {
  state = {
    count: 0
  };

  componentWillMount() {
    setTimeout(() => {
      console.log('开始执行计时器');
      this.setState({
        count: this.state.count + 1
      });
      this.setState({
        count: this.state.count + 1
      });
    }, 0);

    new Promise((resolve) => {
      console.log('开始执行Promis');
      setTimeout(() => {
        console.log('请求成功');
        resolve(true);
      }, 0);
    }).then(() => {
      this.setState({
        count: this.state.count + 1
      });
      this.setState({
        count: this.state.count + 1
      });
    });
  }

  render() {
    console.log('rendered', `count = ${this.state.count}`);

    return (
      <div>
        <h2>setState - Promise</h2>
        <p>Count：{this.state.count}</p>
      </div>
    )
  }
}

```

* 结果：

```console
> 开始执行Promis
> rendered count = 0
> 开始执行计时器
> rendered count = 1
> rendered count = 2
> 请求成功
> rendered count = 3
> rendered count = 4
```

* WHY: setTimeout和promise都没有走到react事件的批量更新里

## 第四个例子

```jsx
import React, { Component } from 'react';

export default class App extends Component {
  state = {
    count: 0
  };

  componentDidMount() {
    this.button.addEventListener(
      'click',
      () => this.onClick('原生事件'),
      false
    );
  }

  onClick = (info) => {
    console.log(info);
    this.setState({
      count: this.state.count + 1
    });
    this.setState({
      count: this.state.count + 2
    });
  }

  render() {
    console.log('rendered', `count = ${this.state.count}`);

    return (
      <div>
        <h2>setState - 原生事件vsReact事件</h2>
        <p>Count：{this.state.count}</p>
        <button
          onClick={() => this.onClick('React事件')}
          ref={input => this.button = input}
        >Click me</button>
      </div>
    )
  }
}

```

* 结果：

```console
> rendered count = 0
> 原生事件
> rendered count = 1
> rendered count = 3
> React事件
> rendered count = 5
```

* WHY: 原生事件没有走到react事件的批量更新里

## 第五个例子

```jsx
import React, { Component } from 'react';

export default class App extends Component {
  state = {
    count: 0
  };

  componentWillMount() {
    this.setState({
      count: this.state.count + 1
    });
    this.setState({
      count: this.state.count + 1
    });
    this.setState((state) => ({
      count: state.count + 1
    }));
    this.setState((state) => ({
      count: state.count + 1
    }));
  }

  onClick = () => {
    console.log('click后')
    setTimeout(() => {
      this.setState({
        count: this.state.count + 1
      });
      this.setState((state) => ({
        count: state.count + 1
      }));
      this.setState((state) => ({
        count: state.count + 1
      }));
    }, 0)
  }

  render() {
    console.log('rendered', `count = ${this.state.count}`);

    return (
      <div>
        <h2>setState - 函数式</h2>
        <p>Count：{this.state.count}</p>
        <button
          onClick={this.onClick}
        >Click me</button>
      </div>
    )
  }
}
```

* 结果：

```console
> rendered count = 3
> click后
> rendered count = 4
> rendered count = 5
> rendered count = 6
```
