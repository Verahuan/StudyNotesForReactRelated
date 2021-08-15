## 1.前言

由于设计稿的jumper和antD的文字有差异，因此涉及到需要自定义

## 2.实现

```jsx
const { Pagination, Input } = antd;

class CustomPagination extends React.Component {
  state = {
    current: 1,
  };
  onChange = (current) => {
    this.setState({
      current,
    });
  }
  go = (e) => {
    console.log(e.key)
    if (e.key === 'Enter') {
      console.log(e.target.value)
      const page = parseInt(e.target.value, 10);
      if (isNaN(page)) {
        return;
      }
      this.setState({
        current: page,
      });
    }
  }
  render() {
    return (
      <span>
        <Pagination
          current={this.state.current}
          total={500}
          onChange={this.onChange}
          style={{ display: 'inline-block', verticalAlign: 'top', marginRight: 8 }}
        />
        <Input style={{ width: 60 }} placeholder="Go" onKeyDown={this.go} />
      </span>
    );
  }
}

ReactDOM.render(
  <CustomPagination />
, mountNode);
```

