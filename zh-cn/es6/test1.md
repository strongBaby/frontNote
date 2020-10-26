本文档整理了项目中可能会经常使用的方法或 API，以文档的形式进行记录。dfdfdf


### H5 滚动穿透

> 解决在 h5 移动端在弹出层进行滚动到底部时，下层也会进行滚动的问题

```javascript
// 为滚动容器绑定bindTouchByElment
export const bindScrollBouce = (vue) => {
    // 滚动容器
    let scrollContent = vue.$refs.scrollContent;
    scrollContent && bindTouchByElment(scrollContent);
};
// 解决滚动穿透问题
export const bindTouchByElment = (el) => {
    // el 为滚动容器
    el.addEventListener("touchstart", function (e) {
        this.allowUp = this.scrollTop > 0;
        this.allowDown = this.scrollTop < this.scrollHeight - this.clientHeight;
        this.lastY = e.targetTouches[0].pageY;
    });

    el.addEventListener("touchmove", function (e) {
        let up = e.targetTouches[0].pageY > this.lastY;
        let down = !up;

        this.lastY = e.targetTouches[0].pageY;
        if ((up && this.allowUp) || (down && this.allowDown)) {
            e.stopPropagation();
        } else {
            e.preventDefault();
        }
    });
};


// 以上是两个方法；然后在对应的vue文件调用bindScrollBouce即可
...
 mounted() {
    // 处理滚动穿透问题
    this.global.bindScrollBouce(this)
},
...
```
