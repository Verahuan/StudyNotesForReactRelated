## 1.前言

使用modal组件过程中的注意事项

## bodyStyle/style

如图所示：style对应的样式是model框的样式

bodyStyle：modal中主体的样式，因为其还包括title等，可以在使用的时候注意一下

```javascript
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
          left: 80,//会改变modal的位置
          top: 80,
          backgroundColor:'red'
        }
        }
```

![image-20210304222435891](D:\typora\images\image-20210304222435891.png)

## 注意

1.modal的宽度有额外的属性width决定，不能由style属性决定，其默认为520px.

```javascript
 width={440}
```

