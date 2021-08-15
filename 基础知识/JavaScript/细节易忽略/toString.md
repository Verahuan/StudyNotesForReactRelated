## 1.前言

- 数字转换为字符串会自动转为科学计数法

## 2.详情-科学计数法

代码：数字转换为科学计数法

`value.toString();`部分如果数字的小数点前有7个零以上，会自动进行转换为科学计数法

`parseFloat`也会进行转换

`Math.abs()`也会进行转换

```html
<script>
function toScience (value) {
var res = value.toString();
let index=res.indexOf("e")
console.log(index)
if(index!==-1){
let v1=res.slice(0,index)
let v2=res.slice(index)
return v1+v2
}
var numN1 = 0;
var numN2 = 1; //小于1的数字 数字前的0的个数
var num1=0;//小数点前的数字个数
var num2=0;//小数点后的数字个数
var t1 = 1;
for(var k=0;k<res.length;k++){
if(res[k]==".") t1 = 0;
if(t1)
num1++;
else
num2++;
}
if(Math.abs(value)<1 && res.length>4)
{
for(let i=2; i<res.length; i++){
if(res[i]=="0"){
numN2++;
}else if(res[i]==".")
continue;
else
break;
}
var v = parseFloat(value);
v = (v * Math.pow(10,numN2)).toFixed(4);
return v+ "e-" + numN2;
}else if(num1>4)
{
if(res[0]=="-")
numN1 = num1 - 2;
else
numN1 = num1 - 1;
var v = parseFloat(value);
v = v / Math.pow(10,numN1);
if(num2 > 4)
v = v.toFixed(4);
return v.toString() + "e" + numN1;
}else
return parseFloat(value);
}
console.log(toScience(0.000000000000000001234))

</script>
```

