---
title: css常用代码
categories: 'css'
tags: 
    - "代码"
    - "代码片段"
thumbnail: "https://gouhi-typora.oss-cn-shanghai.aliyuncs.com/uPic/598ff1d433dd40e8a828257aa0495140_tplv-k3u1fbpfcp-zoom-in-crop-mark_4536_0_0_0.gif"
---



### 未节流的效果

![image-20230506152716924](https://gouhi-typora.oss-cn-shanghai.aliyuncs.com/uPic/image-20230506152716924.png)

```
<button onclick="console.log('保存1')">我是“普通”保存</button>
<button class="throttle" onclick="console.log('保存2')">我是“节流”保存</button>
.throttle {
  animation: throttle 2s step-end forwards;
}
.throttle:active {
  animation: none;
}
@keyframes throttle {
  from {
    pointer-events: none;
    opacity: .5;
  }
  to {
    pointer-events: all;
    opacity: 1;
  }
}
```



### 加节流的效果

![0149a2bfc61a4bfe9476e4817977b380_tplv-k3u1fbpfcp-zoom-in-crop-mark_4536_0_0_0](https://gouhi-typora.oss-cn-shanghai.aliyuncs.com/uPic/0149a2bfc61a4bfe9476e4817977b380_tplv-k3u1fbpfcp-zoom-in-crop-mark_4536_0_0_0.gif)
