验证码倒计时 hook

```js
interface CountDownOptions {
  second: number;
  ck: () => void;
  initialBtnTxt: string;
  sendingTxt?: string;
}
/**
 *
 * @param {number} options.second 倒计时秒数
 * @param {() => void} options.ck 倒计时开始之前的处理函数
 * @param {string} options.initialBtnTxt 按钮初始展示文字
 * @param {string} options.sendingTxt 倒计时开始按钮展示文字
 * @returns
 */
export const useCountDown = (options: CountDownOptions) => {
  const { second, ck, initialBtnTxt, sendingTxt } = options;
  const count = ref(0);
  const timer = ref();
  const btnTxt = ref(initialBtnTxt);
  const btnDisabled = ref(false);
  const countDown = async () => {
    if (count.value == 0 && !timer.value) {
      try {
        await ck();
        count.value = second;
        timer.value = setInterval(() => {
          count.value--;
          btnTxt.value = sendingTxt || `${count.value}`;
          btnDisabled.value = true;
          if (count.value <= 0) {
            btnTxt.value = initialBtnTxt;
            btnDisabled.value = false;
            clearInterval(timer.value);
            timer.value = null;
          }
        }, 1000);
      } catch (err) {
        btnTxt.value = initialBtnTxt;
        btnDisabled.value = false;
        console.log("err", err);
      }
    }
  };

  const clearCountDown = () => {
    btnDisabled.value = false;
    btnTxt.value = initialBtnTxt;
    count.value = 0;
    timer.value && clearInterval(timer.value);
    timer.value = null;
  };

  onUnmounted(() => {
    count.value = 0;
    timer.value && clearInterval(timer.value);
    timer.value = null;
  });

  return { count, countDown, clearCountDown, btnTxt, btnDisabled };
};
```
