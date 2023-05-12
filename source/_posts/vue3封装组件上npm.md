---
title: vue3封装组件
categories: 'vue'
tags: 
    - "封装"
    - "npm"
    - "vue3"
thumbnail: "https://gouhi-typora.oss-cn-shanghai.aliyuncs.com/uPic/code-647012_1280.jpg"
---

### 通用的需求

> 一些项目会用到一些通用的封装，但是如果多个项目使用的话，是否只能复制再粘贴过来。
>
> 如果修改了某一处，5个项目是否需要全部都更新一遍。带着这个困扰，我们来把通用的组件封装起来，并且上传至npm。可以供多份项目使用，并且仅需要更新一次通用组件即可



### 样例

此次的样例使用vue3+element-plus来记录如何自己创建npm包

> 1、创建vue3的项目，然后封装通用搜索，通用表格，通用弹窗，通用抽屉
>
> 2、配置打包配置
>
> 3、开始打包，并且进行本地测试
>
> 4、组件发布到npm包（使用npm login）
>
> 5、新建一个新的vue3项目。导入刚上传的包。测试功能是否正常可用



### 步骤

#### 初始化封装组件项目

1.1、初始化一个vue3的项目

```
npm init vue@latest
```

初始化后的目录结构

![在这里插入图片描述](https://gouhi-typora.oss-cn-shanghai.aliyuncs.com/uPic/fef6a1da-880f-11ed-ba26-5cea1d84200c.png)



1.2、引入必要的组件库

```
yarn add element-plus
yarn add sass
yarn add dayjs
yarn add loadsh
yarn add vite-plugin-dts (必要的打包文件。不然引入文件会提示找不到ui)
```



#### 更新目录结构

2.1、原目录结构删除

- 组件库是没有html，所以移除了App.vue和main.ts
- 删除原来的demo组件

![在这里插入图片描述](https://gouhi-typora.oss-cn-shanghai.aliyuncs.com/uPic/ff2650b3-880f-11ed-b9d2-5cea1d84200c.png)



2.2、新目录结构

- dist: 存放打包好的组件库文件
- src/client: 用于本地测试组件的文件目录（把上述App.vue和main.ts放进来）
- src/components: 用来存放封装的组件
- index.html：用来测试封装组件的入口（需要改main.ts的路径）
- vite.config.ts：主要配置build 的打包参数
- src/index.ts：用于配置打包内容
- package.json 修改配置，使其指向生成的库文件；以及基础的配置信息，方便上传到npm 中

<img src="https://gouhi-typora.oss-cn-shanghai.aliyuncs.com/uPic/image-20230512163348452.png" alt="image-20230512163348452" style="zoom:50%;" />



2.3 、新建一个测试组件 PenkButton.vue（scr/components/PenkButton.vue）

```vue
<template>
  <button @click="clickBtn">原生按钮</button>
  <el-button @click="clickBtn">element-plus按钮</el-button>
</template>

<script setup lang="ts">
function clickBtn() {
  console.log("clickBtn...");
}
</script>
<script lang="ts">
export default {
  name: "PenkButton",
};
</script>
```



#### 修改源文件

3.1、修改vite.config.ts文件。具体可参考vite的官方文件[此处](https://cn.vitejs.dev/guide/build.html#library-mode)

```tsx
import { fileURLToPath, URL } from "node:url";
import { resolve } from "path";
import { defineConfig } from "vite";
import vue from "@vitejs/plugin-vue";
import dts from 'vite-plugin-dts'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue(),dts()],
  resolve: {
    alias: {
      "@": fileURLToPath(new URL("./src", import.meta.url)),
    },
  },
  build: {
    // 到处文件目录，penk-ui 用于存放package.json，避免被覆盖
    // 这里不设置也是默认dist
    outDir:"dist",
    // 兼容
    target: "es2015",
    lib: {
      // Could also be a dictionary or array of multiple entry points
      entry: resolve(__dirname, "src/index.ts"),
      name: "GouhileeUi",
      // the proper extensions will be added
      // 如果不用format文件后缀可能会乱
      fileName: (format) => `gouhilee-ui.${format}.js`,
    },
    rollupOptions: {
      // 确保外部化处理那些你不想打包进库的依赖
      external: ["vue", "element-plus", "sass"],
      output: {
        // 在 UMD 构建模式下为这些外部化的依赖提供一个全局变量
        globals: {
          vue: "Vue",
          "element-plus": "ElementPlus",
        },
      },
    },
  },
});
```



3.2、修改index.html的指向地址

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8">
    <link rel="icon" href="/favicon.ico">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vite App</title>
  </head>
  <body>
    <div id="app"></div>
    <script type="module" src="/src/client/main.ts"></script>
  </body>
</html>

```



3.3、修改刚刚移动到src/client/main.ts的文件

```tsx
import '../assets/main.css'

import { createApp } from 'vue'
import App from './App.vue'
import ElementPlus from "element-plus";
import "element-plus/dist/index.css";

const  app = createApp(App)
app.use(ElementPlus)

// 引入element的icon组件
import * as ElementPlusIconsVue from '@element-plus/icons-vue'
for (const [key, component] of Object.entries(ElementPlusIconsVue)) {
  app.component(key, component)
}

// 定义全局分页参数
app.config.globalProperties.$pageSize = 20;
app.config.globalProperties.$pageSizeOptions = [20, 50, 100];

app.mount('#app')

```



3.4、修改刚刚移动到src/client/App.vue的文件

```vue
<script setup lang="ts">
</script>

<template>
  <PenkButton></PenkButton>
</template>

<style scoped></style>
```



至此封装组件就可以正式测试了,`yarn dev`后效果如下

![在这里插入图片描述](https://gouhi-typora.oss-cn-shanghai.aliyuncs.com/uPic/ffa67cf4-880f-11ed-86ac-5cea1d84200c.png)

#### 封装组件

4.1、封装4个通用组件（src/components）下面

BaseDialog.vue

```vue
<template>
  <el-dialog
    v-bind="$attrs"
    :close-on-click-modal="false"
    :close-on-press-escape="false"
    :draggable="true"
    :before-close="handleClose"
  >
    <template #header="{ row }">
      <slot name="header" :data="row"></slot>
    </template>
    <template #default="{ row }">
      <slot name="default" :data="row"></slot>
    </template>
  </el-dialog>
</template>
<script setup lang="ts">
const emits = defineEmits(['close'])
const handleClose = () => {
  emits('close')
}
</script>
<script lang="ts">
export default {
  name:'BaseDialog'
}
</script>
```



BaseDrawer.vue

```vue
<template>
  <el-drawer
    v-bind="$attrs"
    class="drawer"
    :before-close="handleClose"
    append-to-body
    :show-close="false"
    :close-on-click-modal="false"
  >
    <template #header>
      <slot name="header"/>
    </template>
    <div class="drawer-box">
      <div class="drawer-close" @click="handleClose">
        <el-icon><Close /></el-icon>
      </div>
      <slot name="default"/>
    </div>
  </el-drawer>
</template>

<script setup lang="ts">
const emits = defineEmits(['close'])
const handleClose = ()=>  {  
  // 通知父组件关闭，如果是true就代表清空和刷新数据
  emits('close', true)
}
</script>
<script lang="ts">
export default {
  name: 'BaseDrawer'
}
</script>
<style lang='scss'>
.drawer {
  overflow: visible !important;
}

.drawer-close {
  width: 40px;
  height: 60px;
  position: absolute;
  left: -40px;
  top: 50%;
  transform: translateY(-50%);
  background-color: #f5f5f5;
  cursor: pointer;
  padding: 15px 5px;
  border-bottom-left-radius: 8px;
  border-top-left-radius: 8px;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 24px;
}
</style>

```



BaseSearch.vue

```vue
<style lang="scss" scoped>
.search_box{
  border-bottom: 1px solid#eee;
  padding: 20px 20px 0 20px;
  display: flex;
  // align-items: center;
  justify-content: space-between;
  .search_left{
    display: flex;
    align-items: center;
    flex-wrap: wrap;
    .search_btn{
      display: flex;
      margin-bottom: 20px;
      .search_screen{
        display: flex;
        align-items: center;
        margin-right: 20px;
        color:#409EFF;
        cursor: pointer;
      }
    }
  }
  .search_right{
    display: flex;
    align-items: center;
    margin-bottom: 20px;
  }
}
</style>
<template>
  <div class="search_box">
    <div class="search_left">
      <slot name="searchFirst" />
      <slot v-if="show" name="searchItem" />
      <div class="search_btn">
        <div class="search_screen" @click="handleShow" v-if="$slots.searchItem">
          {{ show ? '收筛选' : '展筛选' }}
          <el-icon>
            <ArrowUp v-if="show"/>
            <ArrowDown v-else />
          </el-icon>
        </div>
        <el-button type="primary" @click="onSearch" :loading="loading" :loading-icon="Eleme">查询</el-button>
        <el-button type="warning" @click="onReset" v-if="showReset">重置</el-button>
      </div>
    </div>
    <div class="search_right">
      <slot name="searchRight"></slot>
    </div>
  </div>
</template>
<script setup lang="ts">
import { Eleme } from '@element-plus/icons-vue'
import { ref } from 'vue'
// eslint-disable-next-line @typescript-eslint/no-unused-vars
const props = defineProps({
  loading: Boolean,
  showReset:{
    type: Boolean,
    default: false
  }
})

const show = ref(false)
const handleShow = () => {
  show.value = !show.value
}

const emits = defineEmits(['search', 'reset'])
const onSearch = () => {
  emits('search')
}
const onReset = () => {
  emits('reset')
}
</script>
<script lang="ts">
export default {
  name: "BaseSearch"
}
</script>
```



BaseTable.vue

```vue
<style lang="scss" scoped>
.table_header{
  padding: 20px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  height: 40px;
  border-bottom: 1px solid #eee;
  .table_header_option{
    display: flex;
    align-items: center;
    ::v-deep .el-link{
      font-size: 12px;
    }

    .setting{
      padding: 20px;
    }
  }
}

.table_foot_box{
  height: 46px;
  padding: 10px;
  border-radius: 4px;
}
</style>
<template>
  <div class="table_header">
    <div><slot name="btn"></slot></div>
    <div>
      <div class="table_header_option">
        <el-link :underline="false" icon="download" class="m-r-10" v-if="isExport">导出</el-link>
        <el-link :underline="false" icon="refreshLeft" class="m-r-10" @click="refresh">刷新</el-link>
        <el-popover
          placement="bottom"
          :width="300"
          trigger="click"
        >
          <div class="setting">
            <div>
              <el-tooltip class="item" effect="dark" content="可控制显示的表格字段，刷新后保持选择项。" placement="top-start">
                <template #content>可控制显示的表格字段，刷新后保持选择项。<br>如果希望还原配置信息请清空浏览器缓存</template>
                <el-icon :size="16"><QuestionFilled /></el-icon>
              </el-tooltip>
              <el-checkbox 
                v-model="selectAll" 
                :indeterminate="isIndeterminate"
                @change="handleCheckAllChange" 
                label="全选" 
                size="large"
                class="m-l-20"
                />

              <el-radio-group v-model="customParams.tableSize" @change="setColumn" class="m-l-20">
                <el-radio label="medium">表格大</el-radio>
                <el-radio label="small">表格小</el-radio>
              </el-radio-group>
            </div>
            <div>
              <el-checkbox-group v-model="column.checkedColumn" @change="handleCheckedColumnGroupChange">
                <template v-for="(item, index) in tableColumn">
                  <div v-if="!item.required" :key="index" class="setting_table_column_item">
                    <el-checkbox :label="item.prop" @change="handleCheckedColumnChange(index)">{{ item.label}}</el-checkbox>
                  </div>
                </template>
              </el-checkbox-group>
            </div>
          </div>
          <template #reference>
            <el-link :underline="false" icon="setting" >设置</el-link>
          </template>
        </el-popover>
      </div>
    </div>  
  </div>
  <el-table 
    :data="tableData" 
    v-bind="$attrs"
    height="100%"
    :highlight-current-row="true"
    v-loading="loading"
    @selection-change="selectionChange"
    >
    <el-table-column v-if="isExpand" type="expand">
      <template #default="scope: any">
        <slot name="expand" :row="scope.row"></slot>
      </template>
    </el-table-column>
    <el-table-column v-if="isCheck" type="selection" width="55" />
    <template v-for="item in customParams.tableColumn">
      <!-- 需要单独处理的字段 -->
      <el-table-column
        v-if="item.columnType && item.display"
        :key="item.prop"
        :prop="item.prop"
        :label="item.label"
        :min-width="item.minWidth"
        :width="item.width"
        :show-overflow-tooltip="!!item.showOverflowTooltip"
        :fixed="item.prop === 'option' ? 'right' : false"
        :class-name="item.prop === 'option' ? 'operation' : ''"
      >
        <template #header v-if="item.columnSlotHeader">
          <slot :name="`${item.columnSlot}Header`" :data="item" />
        </template>
        <template #default="scope: any">
          <slot :name="item.columnSlot" :row="scope.row" :index="1" />
        </template>
      </el-table-column>
      <!-- 不需要处理的数据 -->
      <el-table-column
        v-if="!item.columnType && item.display"
        :key="item.prop"
        :prop="item.prop"
        :label="item.label"
        :min-width="item.minWidth"
        :width="item.width"
      >
        <template #default="scope: any">
          <span v-if="item.date && getProLevel(scope.row , item)">{{ dayjs(getProLevel(scope.row , item)).format(item.date)}}</span>
          <span v-else>{{ getProLevel(scope.row , item) || '-'}}</span>
        </template>
      </el-table-column>
    </template>
  </el-table>
  <div class="table_foot_box">
    <el-pagination
      v-if="tableData && tableData.length > 0"
      background
      small
      :page-size="page.pageSize && page.pageSize >= 20 ? page.pageSize : $pageSize"
      :page-sizes="$pageSizeOptions"
      layout="total, sizes, prev, pager, next, jumper"
      :total="page.total"
      :current-page="page.pageNum"
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
    />
  </div>
</template>

<script setup lang="ts">
import { ElMessage } from 'element-plus'
import _get from 'lodash/get'
import dayjs from 'dayjs'
import { getCurrentInstance, ref, reactive, onMounted, type PropType } from 'vue' 
const emits = defineEmits(['selectionChange', 'handleCurrentChange', 'handleSizeChange', 'refresh'])
const current = getCurrentInstance()

const $pageSize = current?.appContext.config.globalProperties.$pageSize
const $pageSizeOptions = current?.appContext.config.globalProperties.$pageSizeOptions

const props = defineProps({
  tableData: Array,
  tableColumn: {
    type: Array as PropType<TableColumn[]>,
      required: true
  },
  tableName: {
    type: String,
    required: true
  },
  debug: Boolean,
  isCheck: Boolean,
  isExpand: Boolean,
  isExport: {
    type: Boolean,
    default: false
  },
  page: {
    type: Object as PropType<Page>,
    required: true
  },
  loading: Boolean
})

const selectAll = ref(false)
const isIndeterminate = ref(true)
const column:Column = reactive({
  checkedColumn:[],
  allColumn:[]
})
let customParams:CustomParams = reactive({
  tableColumn: props.tableColumn || [],
  tableSize: 'medium'
})

onMounted(()=>{
  if(!props.tableName){
    ElMessage.error('调用子组件请填写表格名称')
  }
  
  if(props.debug){
    customParams.tableColumn = props.tableColumn 
    customParams.tableSize = 'medium'
  }else{
    const sessionStorageCustomTable = sessionStorage.getItem(props.tableName)
    if (sessionStorageCustomTable) {
      customParams = reactive(JSON.parse(sessionStorageCustomTable))
    } else {
      setColumn()
    }
  }
  
  // 传入外部的数据并且存入
  for (const item of customParams.tableColumn) {
    if (!item.required && item.prop) {
      column.allColumn.push(item.prop)
      if (item.display !== false) {
        column.checkedColumn.push(item.prop)
      }
    }
  }
  // 判断初始化全选多选框的状态
  selectAll.value = column.checkedColumn.length === column.allColumn.length
  isIndeterminate.value = column.checkedColumn.length > 0 && column.checkedColumn.length < column.allColumn.length  
})

/**
 * 设置可显示表格列
 */
const handleCheckAllChange = (val: boolean) => {
  column.checkedColumn = val ? column.allColumn : []
  isIndeterminate.value = false
  customParams.tableColumn.forEach((i: TableColumn) =>
    i.required !== true ? (i.display = val) : true
  )
  setColumn()
}
const handleCheckedColumnGroupChange = (value: string[]) => {
  const checkedCount = value.length
  selectAll.value = checkedCount === column.allColumn.length
  isIndeterminate.value = checkedCount > 0 && checkedCount < column.allColumn.length
}
const handleCheckedColumnChange = (index:number) => {
  customParams.tableColumn[index].display = !customParams.tableColumn[index].display
  setColumn()
}

// 通用改值存会话中
const setColumn = () => {
  sessionStorage.setItem(props.tableName, JSON.stringify(customParams))
}

// 获取到prop下级元素
// eslint-disable-next-line @typescript-eslint/no-explicit-any
const getProLevel = (row: any,item: TableColumn) => {
  if (item.prop.indexOf('.') > -1) {
    return _get(row, item.prop)
  } else {
    return row[item.prop]
  }
}

 const selectionChange = (v: number[]) => {
  emits('selectionChange',v)
 }

// 分页操作
const handleSizeChange = (v: number)=>{
  console.log('handleSizeChange');
  emits('handleSizeChange', v)
}
const handleCurrentChange = (v: number)=>{
  console.log('handleCurrentChange');
  emits('handleCurrentChange',v)
}

const refresh = () => {
  emits('refresh')
}
</script>
<script lang="ts">
export default {
  name: "BaseTable"
}
</script>

<style scoped>
</style>

```



4.2、封装css文件（scr/assets/css）

 [base.scss](../../../../finance/npm-gouhilee-element-base/src/assets/css/base.scss) 

 [common.scss](../../../../finance/npm-gouhilee-element-base/src/assets/css/common.scss) 

 [element-ui.scss](../../../../finance/npm-gouhilee-element-base/src/assets/css/element-ui.scss) 

 [index.scss](../../../../finance/npm-gouhilee-element-base/src/assets/css/index.scss) 

 [reset.scss](../../../../finance/npm-gouhilee-element-base/src/assets/css/reset.scss) 

 [variable.scss](../../../../finance/npm-gouhilee-element-base/src/assets/css/variable.scss) 



4.3、根目录下env.d.ts文件中新增ts的声明文件

```tsx
declare interface TableColumn {
  prop: string,
  label: string,
  display: boolean,
  minWidth?: number,
  width?: number,
  required?: boolean,
  columnType?: boolean,
  columnSlot?: string,
  showOverflowTooltip?: boolean,
  columnSlotHeader?: string,
  date?: string
}

declare interface Column {
  checkedColumn: string[],
  allColumn: string[]
}

declare interface CustomParams {
  tableColumn: TableColumn[];
  tableSize: string
}

declare interface Page {
  pageSize: number,
  pageNum: number,
  total?: number
}
```



4.4、修改package.json文件内的内容

- files：可选的files字段是一个文件模式数组，它描述了包作为依赖项安装时要包含的条目。
- main：主字段是一个模块ID，它是程序的主要入口点。也就是说，如果你的包被命名为foo，用户安装了它，然后确实需要(“foo”)，那么你的主模块的exports对象将被返回。如果main未设置，则默认为包根文件夹中的index.js。
- exports：导出的文件，这里要注意样式文件的`导出`

```json
{
  "name": "gouhilee-ui",
  "version": "1.0.22",
  "description": "对elementPlus进行二次封装的组件库",
  "files": [
    "dist",
    "dist/**"
  ],
  "keywords": [],
  "author": "gouhilee",
  "email": "755913611@qq.com",
  "scripts": {
    "dev": "vite",
    "build": "run-p type-check build-only",
    "preview": "vite preview",
    "build-only": "vite build",
    "type-check": "vue-tsc --noEmit",
    "on-up": "run-p type-check build-only && yarn publish"
  },
  "dependencies": {
    "dayjs": "^1.11.7",
    "element-plus": "^2.3.4",
    "loadsh": "^0.0.4",
    "sass": "^1.62.1",
    "vite-plugin-dts": "^2.3.0",
    "vue": "^3.2.47"
  },
  "devDependencies": {
    "@tsconfig/node18": "^2.0.0",
    "@types/node": "^18.16.3",
    "@vitejs/plugin-vue": "^4.2.1",
    "@vue/tsconfig": "^0.3.2",
    "npm-run-all": "^4.1.5",
    "typescript": "~5.0.4",
    "vite": "^4.3.4",
    "vue-tsc": "^1.6.4"
  },
  "main": "./dist/gouhilee-ui.umd.js",
  "module": "./dist/gouhilee-ui.es.js",
  "exports": {
    ".": {
      "import": "./dist/gouhilee-ui.es.js",
      "require": "./dist/gouhilee-ui.umd.js"
    },
    "./dist/style.css": "./dist/style.css"
  },
  "typings": "./dist/src/index.d.ts"
}

```





4.5、 打包组件，新增src/index.ts文件

```tsx
// 引入封装好的组件
import BaseDialog from "./components/BaseDialog.vue";
import BaseDrawer from "./components/BaseDrawer.vue"
import BaseSearch from "./components/BaseSearch.vue"
import BaseTable from "./components/BaseTable.vue"
import "./assets/css/index.scss"

let arr = [BaseDialog, BaseDrawer, BaseSearch, BaseTable]; // 如果有多个其它组件,都可以写到这个数组里

// 批量组件注册
const install = function (Vue: any) {
  arr.forEach((com) => {
    Vue.component(com.name, com);
  });
};

export default install;
```



4.6、构建并发布

```
yarn build
yarn publish
```



#### 本地测试安装一下

5.1、新增一个vue新项目

```
yarn add vue@latest
```



5.2、安装npm库

```
yarn add gouhilee-ui
```



5.3、修改main.ts文件

```
import GouhiLeeUi from 'gouhilee-ui'
import "gouhilee-ui/dist/style.css"

app.use(GouhiLeeUi)
```



5.4、修改App.vue

```
<script setup>
</script>

<template>
  <PenkButton></PenkButton>
</template>

<style scoped>
</style>
```



5.5、测试，效果如下

```
yarn dev
```

![在这里插入图片描述](https://gouhi-typora.oss-cn-shanghai.aliyuncs.com/uPic/001dafb0-8810-11ed-b94e-5cea1d84200c.png)
