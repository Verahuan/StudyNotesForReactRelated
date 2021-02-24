## 1.前言

主要是涉及到Space的构成原理，方便其布局等特性

!!!!!

不要试图使用space布局

## ！！何时使用[#](https://ant-design.gitee.io/components/space-cn/#何时使用)

避免组件紧贴在一起，拉开统一的空间。

- 适合行内元素的水平间距。
- 可以设置各种水平对齐方式。

## 2.组成原理

目前看来是利用margin进行定位

## 3.`Space`内部元素定位

Space内部的元素会被一个容器包裹，可以使用flex进行定位

```jsx
            <Space size={40} direction={'vertical'}>
              <Space
                size={"16px"} direction={'vertical'}
                style={{
                  display:'flex',
                  alignItems:'center',
                  backgroundColor:'pink'
                }}>
                <Space
                  size={-10}>
                  <TriangleIcon/>
                  <TriangleReverseIcon/>
                </Space>
                <text style={{
                  fontFamily: "FZZongYi-M05S",
                  fontSize: "14px",
                  lineHeight: "16px"
                }}>LOGO</text>
              </Space>
```

![image-20210304224948036](D:\typora\images\image-20210304224948036.png)

如图中的部分，如果直接使用position进行定位的话，可能是因为外层是modal组件，所以没办法正常使用

**问题描述**

给space设置使用absolute进行定位的时候，不会从Form的边缘进行定位，而是会从红色最左边进行定位，这个问题没办法按照"子绝父相"的原理来解决

**代码如下：**

```jsx
      <Modal
        bodyStyle={
          {
            position:'absolute',
            top:64,
            right:70,
            padding:0,
            backgroundColor:'green'
          }
        }
        style={{
          height: 480,
          left: 80,
          top: 80,
          backgroundColor:'red'
        }
        }
        width={440}
        visible={true}
        centered={true}
        footer={null}
        closable={false}
        maskClosable={false}
      >
        <div>
          <Form
            {...layout}
            name="basic"
            initialValues={{ remember: true }}
            onFinish={onFinish}
            onFinishFailed={onFinishFailed}
          >
            <Space size={40} direction={'vertical'}>
              <Space
                size={"16px"} direction={'vertical'}
                style={{
/*这里的flex变为position：absolute进行定位的时候会出现以上问题*/
                  display:'flex',
                  alignItems:'center',
                  backgroundColor:'pink'
                }}>
                <Space
                  size={-10}>
                  <TriangleIcon/>
                  <TriangleReverseIcon/>
                </Space>
                <text style={{
                  fontFamily: "FZZongYi-M05S",
                  fontSize: "14px",
                  lineHeight: "16px"
                }}>LOGO</text>
              </Space>
              <Space size={"20px"} direction={'vertical'}>
                <Form.Item
                  style={FromItemStyle}
                  rules={[{ required: true, message: 'Please input your username!' }]}
                >
                  <Input />
                </Form.Item>

                <Form.Item
                  style={FromItemStyle}
                  rules={[{ required: true, message: 'Please input your password!' }]}
                >
                  <Input.Password />
                </Form.Item>

                <Form.Item name="remember" valuePropName="checked">
                  <Checkbox>Remember me</Checkbox>
                </Form.Item>
              </Space>
              <Form.Item>
                <Button type="primary" htmlType="submit">
              登录
                </Button>
              </Form.Item>
            </Space>
          </Form>
        </div>
      </Modal>
```

