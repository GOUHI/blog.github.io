---
title: JS并发请求demo
categories: 'js'
tags: 
    - "demo"
    - "并发"
---



```
'use strict';

const { default: axios } = require("axios");

for (let i = 0; i < 1000; i++) {
  axios.post('http://127.0.0.1:7007/fission/1', {
    "openid": "o8u9C5mA0_bFz-XkZ-srvy3rhfpc",
    "unionid": "o9PCMwi2dHXA8qXL1f1ppX8mLhP4",
    "region": "341102"
  }, { headers: {
    'x-cloud-site-id': 10001
  } });
  console.log(i);
}
```

