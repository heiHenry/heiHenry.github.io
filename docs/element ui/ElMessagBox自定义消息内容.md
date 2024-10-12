#### 使用 h 函数自定义弹框内部消息内容

```js
let list = [
  { origin: 200, reduce: 10 },
  { origin: 1000, reduce: 100 },
];
const genDivs = () => {
  return list.map((item) => {
    return h("div", null, `满足买满200`);
  });
};
ElMessageBox({
  title: "提示",
  confirmButtonText: "确定",
  showCancelButton: false,
  showClose: false,
  message: h("div", null, genDivs()),
  beforeClose: () => {
    console.log(document.activeElement); //获取当前焦点元素
  },
})
  .then(() => {})
  .catch((err) => {});
```

ELMessageBox 弹框弹出后按键盘 enter 键，弹框会关闭，但是关闭事件中存在冒泡

解决方案：
添加事件监听，如果 document 监听到的`keydown`事件中的目标元素是 reduce-btn,则阻止事件冒泡
这时就不会触发客户信息弹框中的键盘 enter 事件

```js
const handleKeyDownEvent = (e) => {
  if (e.target.classList.contains("reduce-btn")) {
    e.stopPropagation();
  }
};
document.addEventListener("keydown", handleKeyDownEvent);

export const handleGoodsFullReduceToCustomer = () => {
  // 判断购物车商品是否满足买满减现
  dialogInfoStore.setCurrentDialog(null);
  useCashApi()
    .getBuyFullReduce(saleFlowStore.getFormatGoodsFlow())
    .then((res) => {
      if (res.data.buyFullReduces.length > 0) {
        const genDivs = () => {
          return res.data.buyFullReduces.map((item) => {
            return h(
              "div",
              null,
              `满足买满${item.total}减${item.reduceAmt}促销活动`
            );
          });
        };
        ElMessageBox({
          title: "提示",
          confirmButtonText: "确定",
          showCancelButton: false,
          showClose: false,
          closeOnPressEscape: false,
          confirmButtonClass: "reduce-btn",
          message: h("div", null, genDivs()),
        })
          .then(() => {
            dialogInfoStore.setCurrentDialog("CustomerInfo");
          })
          .catch((err) => {});
      }
    });
};
```
