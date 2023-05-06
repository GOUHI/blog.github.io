---
title: 使用canvas合成两张图片(用于海报中的二维码)
categories: 'canvas'
tags: 
    - "图片"
    - "合成"
---

{% notel blue 场景使用 %}
需要通过一张图片，加上二维码生成一张海报。

通过使用canvas绘画的技术来生成此海报。

记录仅提供思路，场景运用可以有很多

{% endnotel %}



### 创建数据

```JavaScript
import posterBgImg from '../../../assets/img/wxwork/activity/bg.png';
export default {
  name: 'push-new',
  data() {
    return {
      // 原海报地址
      posterBgImg,
      // 二维码地址
      qrcodeImg: 'https://wework.qpic.cn/wwpic/735241_H5vgyBZzRPOaLOk_1636357487/0',
      // 微信头像地址
      avatar: 'http://thirdwx.qlogo.cn/mmopen/PiajxSqBRaEIydoxiaoUddycLyAfUGY9icNOgsLyOS8Rp22pZp4BPNjzOkj5kIehECJjkbG7t2cCVSHgdEcc2XUNQ/132',
      // 微信昵称
      nickname: '李狗嗨',
      // 生成海报地址
      preview: '',
    }
  }
} 
```

### 绘制背景（本地地址，直接获取）

```JavaScript
drawBgImage(canvas, ctx) {
  return new Promise(resolve => {
    const bg = new Image();
    bg.src = this.posterBgImg;
    console.log(bg.src);
    bg.setAttribute('crossOrigin', 'anonymous')
    bg.onload = function () {
      ctx.drawImage(bg, 0, 0, canvas.width, canvas.height);
      resolve();
    }
  });
},
```

### 绘制远程二维码（先下载转base64再进行绘制）

```JavaScript
drawQrImage(canvas, ctx) {
  return new Promise(resolve => {
    const qr = new Image();
    qr.src = this.qrcodeImg;
    qr.setAttribute('crossOrigin', 'anonymous')
    qr.onload = function () {
      ctx.drawImage(qr, 35, 986, 170, 170);
      resolve();
    }
  });
},
```

### 绘制微信头像（远程地址，注意跨域）

```JavaScript
drawAvatarImage(canvas, ctx) {
  return new Promise(resolve => {
    const avatar = new Image();
    avatar.src = this.avatar;
  
    avatar.setAttribute('crossOrigin', 'anonymous')
    avatar.onload = function () {
      // 圆角修改
      ctx.arc(80, 80, 40, 0, 2 * Math.PI);
      ctx.clip();
      ctx.drawImage(avatar, 40, 40, 80, 80);
      resolve();
    }
  });
},
```

### 绘制微信文字（文本）

```JavaScript
drawNicknameText(canvas, ctx) {
  return new Promise(resolve => {
    ctx.font = 'bold 30px serif';
    ctx.fillStyle = '#fff'
    ctx.fillText(this.nickname, 133, 75);
    resolve();
  });
},
```

### 编写回调

```JavaScript
getCanvasBlob(canvas) {
  return new Promise(resolve => {
    canvas.toBlob((b) => {
      resolve(b);
    }, 'image/png', 1.0)
  });
},
```

### 创建画板

```
// 生成海报
async createPoster() {
  const canvas = document.createElement('canvas');
  canvas.width = 670;
  canvas.height = 1192;

  const ctx = canvas.getContext('2d');

  // 绘制背景
  await this.drawBgImage(canvas, ctx);

  // 绘制二维码
  await this.drawQrImage(canvas, ctx);

  // 绘制微信名称
  await this.drawNicknameText(canvas, ctx)

  // 绘制微信头像
  await this.drawAvatarImage(canvas, ctx)

  this.preview = URL.createObjectURL(await this.getCanvasBlob(canvas));

  const file = canvas.toDataURL('image/png');
  console.log(file)

  // 上传OSS
},
```



> 成品展示

![image (5)](https://gouhi-typora.oss-cn-shanghai.aliyuncs.com/uPic/image%20%285%29.png)