## 新增数据类型

### Symbol类型
> 每一个从Symbol()返回的symbol值都是唯一的

### Set类型
> 类似于数组，但是成员的值都是唯一的，没有重复值

```
const s = new Set();

[2, 3, 5, 4, 5, 2, 2].forEach(x => s.add(x));

for (let i of s) {
  console.log(i);
}
// 2 3 5 4
```

用来去重非常合适
```
export function uniqueArr(arr) {
  return Array.from(new Set(arr))
}
```

