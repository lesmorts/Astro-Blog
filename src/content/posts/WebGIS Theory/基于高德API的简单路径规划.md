---
title: 基于高德API+Vue3+Vite+TS+Element-Plus的简单路径规划前端程序
published: 2024-03-18
tags: [高德JSAPI, Vue, Vite, Mockjs]
category: WebGIS
---
# 基于高德API+Vue3+Vite+TS+Element-Plus的简单路径规划前端程序

> 搭建前端路径规划系统，与后端Django进行对接

## 前言

程序增加了按需导入，路径规划和基本的前后端交互功能

## 项目源码

## 项目前端预览

首页

## 项目指南

功能清单：

- 搜索栏
- 定位点
- 路径规划
- 清除覆盖物

技术栈：

| **技术栈** | **描述**                             | **官网**                                      |
| :--------------- | :----------------------------------------- | :-------------------------------------------------- |
|                  |                                            |                                                     |
| Vue3             | 渐进式 JavaScript 框架                     | https://cn.vuejs.org/                               |
| Element Plus     | 基于 Vue 3，面向设计师和开发者的组件库     | https://element-plus.gitee.io/zh-CN/                |
| Vite             | 前端开发与构建工具                         | [https://cn.vitejs.dev/](https://cn.vitejs.dev/guide/) |
| TypeScript       | 微软新推出的一种语言，是 JavaScript 的超集 | https://www.tslang.cn/                              |
| Pinia            | 新一代状态管理工具                         | https://pinia.vuejs.org/                            |
| 高德APi          | 高德开放平台提供的JS APi                   | https://lbs.amap.com/api/                           |
| Mockjs           | 模拟后端，拦截axio请求以异地测试           | http://mockjs.com/                                  |

## 环境准备

|                      | **名称**                                                    |
| :------------------- | :---------------------------------------------------------------- |
| **开发工具**   | VSCode                                                            |
| **运行环境**   | Node  18+                                                         |
| **VSCode插件** | Vue Language Features (Volar)`和 `TypeScript Vue Plugin (Volar) |

## vite项目初始化

```
npm init vite@latest delivery vue-ts
```

- **`delivery`**: 自定义的项目名称
- **`vue-ts`** ： `vue` + `typescript` 模板的标识，查看 [create-vite](https://github.com/vitejs/vite/tree/main/packages/create-vite) 以获取每个模板的更多细节：vue，vue-ts，react，react-ts

启动项目

```
npm install
npm run dev
```

## src路径别名配置

> 相对路径别名配置，使用@代替src

vite配置：

配置viteconfig.ts

TS编译器配置

```json
//tsconfig.json
"compilerOptions": {
    ...
    "baseUrl": "./", // 解析非相对模块的基地址，默认是当前目录
    "paths": { // 路径映射，相对于baseUrl
    	"@/*": ["src/*"] 
    }
}
```

路径别名使用

```vue
/ src/App.vue
import HelloWorld from '/src/components/HelloWorld.vue'
						↓
import HelloWorld from '@/components/HelloWorld.vue'
```

## unplugin 自动导入

> Element Plus 官方文档中推荐 `按需自动导入` 的方式，而此需要使用额外的插件 `unplugin-auto-import` 和 `unplugin-vue-components` 来导入要使用的组件。所以在整合 `Element Plus` 之前先了解下 `自动导入`的概念和作用

概念：为了避免在多个页面重复引入 `API` 或 `组件`，由此而产生的自动导入插件来节省重复代码和提高开发效率。

| 插件                    | 概念             | 自动导入对象                                  |
| ----------------------- | ---------------- | --------------------------------------------- |
| unplugin-auto-import    | 按需自动导入API  | ref，reactive,watch,computed 等API            |
| unplugin-vue-components | 按需自动导入组件 | Element Plus 等三方库和指定目录下的自定义组件 |

具体区别对比：

| 插件名                  | 未使用自动导入                                                                                                        | 使用自动导入                                                                                                          |
| ----------------------- | --------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------- |
| unplugin-auto-import    | ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ff113799c83343acab22cbf0e810446a~tplv-k3u1fbpfcp-zoom-1.image) | ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/a78ddb9ff09e44afb96b45527ad719da~tplv-k3u1fbpfcp-zoom-1.image) |
| unplugin-vue-components | ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f1aeff2ce05346faa343eb9f1a796ebe~tplv-k3u1fbpfcp-zoom-1.image) | ![img](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/f8b9af5a27684af59d32338992a200cb~tplv-k3u1fbpfcp-zoom-1.image) |

**安装插件依赖**

```
npm install -D unplugin-auto-import unplugin-vue-components
```

**vite.config.ts -自动导入配置**

新建 `src/types`目录用于存放自动导入函数和组件的TS类型声明文件

```tsx
import AutoImport from "unplugin-auto-import/vite";
import Components from "unplugin-vue-components/vite";

plugins: [
  AutoImport({
    // 自动导入 Vue 相关函数，如：ref, reactive, toRef 等
    imports: ["vue"],
    eslintrc: {
      enabled: true, // 是否自动生成 eslint 规则，建议生成之后设置 false 
      filepath: "./.eslintrc-auto-import.json", // 指定自动导入函数 eslint 规则的文件
    },
    dts: path.resolve(pathSrc, "types", "auto-imports.d.ts"), // 指定自动导入函数TS类型声明文件路径
  }),
  Components({
    dts: path.resolve(pathSrc, "types", "components.d.ts"), // 指定自动导入组件TS类型声明文件路径
  }),
]

```

.eslintrc.cjs-自动导入函数eslint规则引入

```js
"extends": [
    "./.eslintrc-auto-import.json"
],
```

tsconfig.json-自动导入TS类型声明文件引入

```json
{
  "include": ["src/**/*.d.ts"]
}
```

## 整合Element Plus

参考： [element plus 按需自动导入](https://element-plus.gitee.io/zh-CN/guide/quickstart.html#按需导入)

安装 Element Plus

`npm install element-plus`

安装自动导入icon依赖（若必要）

`npm i -D unplugin-icons`

vite.config.ts配置

参考： [element-plus-best-practices - vite.config.ts](https://github.com/sxzz/element-plus-best-practices/blob/main/vite.config.ts)

```typescript
// vite.config.ts
import { ElementPlusResolver } from "unplugin-vue-components/resolvers";
import Icons from "unplugin-icons/vite";
import IconsResolver from "unplugin-icons/resolver";

export default ({ mode }: ConfigEnv): UserConfig => {

  return {
    plugins: [
      // ...
      AutoImport({
        // ...  
        resolvers: [
          // 自动导入 Element Plus 相关函数，如：ElMessage, ElMessageBox... (带样式)
          ElementPlusResolver(),
          // 自动导入图标组件
          IconsResolver({}),
        ]
        vueTemplate: true, // 是否在 vue 模板中自动导入
        dts: path.resolve(pathSrc, 'types', 'auto-imports.d.ts') // 自动导入组件类型声明文件位置，默认根目录
  
      }),
      Components({ 
        resolvers: [
          // 自动导入 Element Plus 组件
          ElementPlusResolver(),
          // 自动注册图标组件
          IconsResolver({
            enabledCollections: ["ep"] // element-plus图标库，其他图标库 https://icon-sets.iconify.design/
          }),
        ],
        dts: path.resolve(pathSrc, "types", "components.d.ts"), //  自动导入组件类型声明文件位置，默认根目录
      }),
      Icons({
        // 自动安装图标库
        autoInstall: true,
      }),
    ],
  };
};
```

示例代码

```vue
<el-menu-item index="3" @click="state.clearPl()" class="menu-item-de" :disabled="state.isDer">
            <el-icon style="margin: auto;">
                <CloseBold />
            </el-icon>
            <template #title>清除绘制</template>
        </el-menu-item>
```

## 高德地图API

新建 `src/utils`目录用于存放实用程序的TS类型声明文件

新建 `gaode.ts`，实现对于高德api的载入地图配置初始化，动态创建Script标签，并导出载入地图函数

```tsx
import AMapLoader from '@amap/amap-jsapi-loader';

const securityJsCode = '此处为安全密钥'; 
const key = '此处为申请的key'; 
const version = '2.0'; 
const adress = '/gdapi'; //配置的高德API网站

window._AMapSecurityConfig = {
  securityJsCode 
};

/**
 * 载入地图
 * @param {*} plugins 地图配置项
 * @returns promise
 */
const mapLoader = (plugins: string[]) =>
  AMapLoader.load({
    key, 
    version, 
    plugins 
  });

// 动态创建script标签，引入代理服务器
const script = document.createElement('script');
script.type = 'text/javascript';
script.src = adress;
document.body.appendChild(script);

// 导出载入地图函数
export default mapLoader;



```

### **创建地图与加载插件**

通过异步的方式加载地图与插件

```tsx
async createMap() {
            try {
                this.AMap = await mapLoader(this.plugins)
                let _AMap: any = this.AMap
                this.map = new _AMap.Map('container', this.config)
                let _map: any = this.map
                this.asnycLoaderPlugins(_AMap, _map, this.plugins)//加载插件函数
            } catch (e) {
                console.log(e)
            }
        },
```

功能

### 状态中心

在 `src/state`中新建index.ts，创建变量与函数实现信息交互

```tsx
import { reactive } from "vue";

export const state = reactive({
    ...
  isMark: false,
  demand: 0,
  SearchOption: {
    city: "巴中",
    input: "tipinput",
  },
  sPlan: false,
  clearPlan: false,
  setCity(msg: string) {
    this.SearchOption.city = msg;
  },
    ...
}
```

### 添加点

通过构建Reactive0状态中心+watch实现组件间信息互传,在点击添加点后，实现在地图组件中添加标点

添加点函数：

```tsx
addMarker(AMap: any, map: any) {
            this.index++
            this.state.markmess = true
            const icon = new AMap.Icon({
                text: this.index,
                size: new AMap.Size(24, 34),
                image: '//a.amap.com/jsapi_demos/static/demo-center/icons/dir-via-marker.png',
                imageSize: new AMap.Size(24, 34),
                imageOffset: new AMap.Pixel(0, 0),
                offset: new AMap.Pixel(-12, -32),
            })
            AMap.plugin(['AMap.Marker'], () => {
                this.addmarkers = new AMap.Marker({

                    position: map.getCenter(),
                    icon: icon
                });
                // eslint-disable-next-line @typescript-eslint/no-explicit-any
                (this.addmarkers as any).setLabel({
                    offset: new AMap.Pixel(0, -5),  //设置文本标注偏移量
                    content: `<el-tag type="primary">demand:${this.state.demand}</el-tag>`, //设置文本标注内容
                    direction: 'top' //设置文本标注方位
                });

                map.add(this.addmarkers)
            })
            let lng: number = map.getCenter().lng as number
            let lat: number = map.getCenter().lat as number
            //储存点数据
            let loc = {
                index: this.index,
                position: <number[]>[lng, lat],
                demand: state.demand,
            }
            this.markers.push(loc)
            console.log(this.markers)
            state.markers = this.markers
        },
```

添加watch监听

```tsx
watch: {
        'state.isMark': {
            handler(val) {
                if (val === true) {
                    state.setMark()
                    this.addMarker(this.AMap, this.map)
                    state.isDer = false
                }
            },
            deep: true,
        },
}
```

### 路径规划

在点击路径规划后，首先获取已添加路径点的坐标并按经纬度储存在数组中，同时向后端post储存数组，通过get获取经分析后的线路数据，根据线路数据索引生成线路顺序的数组。通过添加途径点的方式实现基本的线路规划

路径规划函数：

```tsx
pathPlanning(AMap: any, map: any) {
            var startLngLat = [106.741969, 31.87229]
            var endLngLat = [106.741969, 31.87229]
            this.getLinePntCoord(this.linePoints)
            AMap.plugin(["AMap.Driving"], () => {
                this.drivingPlan = new AMap.Driving({
                    map: map,
                    showTraffic: false,
                    hideMarkers: true,
                });
                for (let i = 0; i < this.linePoints.length; i++) {
                    const wayPath = this.middlePoints[i].map(item => {
                        // eslint-disable-next-line @typescript-eslint/no-explicit-any
                        return (item as any).position

                    });
                    //console.log("wayPath:")
                    //console.log(wayPath);
                    // eslint-disable-next-line @typescript-eslint/no-explicit-any
                    (this.drivingPlan as any).search(
                        startLngLat, endLngLat,
                        { waypoints: wayPath },
                        // eslint-disable-next-line @typescript-eslint/no-explicit-any
                        (status: any, result: any) => {
                            if (status === 'complete') {
                                if (result.routes && result.routes.length) {
                                    this.drawRoute(result.routes[0], i, AMap)
                                }
                                //console.log(result)
                            }
                            else {
                                console.log(result)
                            }
                        }
                    )
                }
            });
        },
```

绘制折线样式更改

```tsx
drawRoute(route: any, index: number, AMap: any) {
            const path = this.parseRouteToPath(route)
            // 生成 折线路线
            // eslint-disable-next-line @typescript-eslint/no-explicit-any
            this.routeLine_obj = new AMap.Polyline({
                zIndex: 52,
                path: path,
                isOutline: true,
                outlineColor: '#ffeeee',
                borderWeight: 2,
                strokeWeight: 5,
                strokeColor: this.colors[index] || '#A52A2A',
                lineJoin: 'round',
                map: this.map
            })
        },
```

折线转路径

```tsx
        parseRouteToPath(route: any) {
            const path = []
            for (let i = 0, l = route.steps.length; i < l; i++) {
                const step = route.steps[i]
                for (let j = 0, n = step.path.length; j < n; j++) {
                    path.push(step.path[j])
                }
            }
            return path
        },
```

通过axios实现的get和post函数

```tsx
fetchGet() {
            axios.get('/ap/users') // 发送 GET 请求到 /api/users
                .then(response => {
                    this.linePoints = response.data
                })
                .catch(error => {
                    console.error('Error fetching users', error);
                });
        },
        fetchPost() {
            axios.post('/ap/users', JSON.stringify(this.markers), {
                headers: {
                    'Content-Type': 'application/json'
                }
            }) // 发送 GET 请求到 /api/users
                .then(response => {
                    console.log("response")
                    console.log(response)
                })
                .catch(error => {
                    console.error('Error fetching users', error);
                });
        },
```

通过watch监听实现异步路径规划

```tsx
       'state.isPlan': {
            handler(val) {
                if (val) {
                    this.fetchPost()
                    this.fetchGet()
                }

            },
            deep: true,
        },
        'linePoints': {
            handler(val) {
                console.log(val)
                this.pathPlanning(this.AMap, this.map)
                state.setLinemess()
                state.isDer = false
                state.setPlan()
            }
        },
```

### 删除地图覆盖物

同理通过状态中心+watch形式实现删除功能

watch：

```ts
'state.clearPlan': {
            handler(val) {
                console.log(val)
                if (val === true) {

                    console.log(val)
                    if (state.linemess) {
                        // eslint-disable-next-line no-undef
                        ElMessage.error("未进行路径规划")
                    }
                    else {
                        state.clearPl()
                        this.removeAll();
                        state.isDer = true
                    }
                    // eslint-disable-next-line @typescript-eslint/no-explicit-any
                    (this.map as any).add(this.stMark)

                }

            }
        }
```

删除函数：

```tsx
removeAll() {
            if (this.map) {
                // 清除地图上的覆盖物

                if (this.markers.length > 1) {
                    // eslint-disable-next-line @typescript-eslint/no-explicit-any, no-undef
                    (this.map as any).clearMap();

                    this.index = 0
                    // 将 markers 设为默认
                    this.markers = [{ index: 0, position: [106.741969, 31.87229], }];
                }
                if (state.markmess) {
                    // eslint-disable-next-line @typescript-eslint/no-explicit-any, no-undef
                    (this.drivingPlan as any).clear();
                    this.middlePoints = []
                }

            } else {
                console.error('Map is not initialized');
            }
        },
```

## mockjs

在后端异地情况下，使用mockjs能够通过拦截axios的post和get请求实现模拟后端

安装mockjs

```
npm install mockjs
```

在 `src/mock`中新建index.ts配置交互数据以及拦截函数

```tsx
import Mock from "mockjs";

const linePoints = [
  [0,4,0],
  [0,2,1,0],
  [0,3,0]
];

Mock.mock("/ap/users", "post", (options) => {
  console.log("options.body");
  console.log(options.body);
  return options.body;
});

Mock.mock("/ap/users", "get", () => {
  return linePoints;
});
Mock.mock(/(amap|maps)\.amap\.com/, "get", {
  // 返回一个空对象，即不拦截高德地图 API 的请求
});
Mock.mock(/(amap|maps)\.amap\.com/, "post", {
  // 返回一个空对象，即不拦截高德地图 API 的请求
});
```

在main.ts中引入该文件

`import './mock/index'`

> 此处使用mockjs会使得高德地图api地图不显示，但插件都能加载的情况

## 跨域问题解决

在项目打包后，出现了跨域问题，这里通过配置proxy的方式解决

`vite.config.ts`中

```tsx
export default defineConfig({
  server: {
    proxy: {
        //高德api
      "/gdapi": {
        target: "https://webapi.amap.com/loader.js",
        changeOrigin: true,
        secure: false,
        rewrite: (path) => path.replace(/^\/gdapi/, ""),
      },
        //后端api
      "/api":{
        target:"http://127.0.0.1:8000",
        changeOrigin:true,
        secure:false,
        rewrite:(path)=>path.replace(/^\/api/, "")
      }
    },
  },
  ...
  })
```

调用时，使用 `/api`或 `/gaode`代替原来网址

## 总结

此次程序为开学的练手，基于Vite+vue+Element Plus ，主要包括项目构建，前端工程化，ElementPlus UI,状态中心，mockjs，跨域等知识点，但是只了解了些皮毛，例如mockjs的使用，组件通信方式，前端工程化等都还能进一步拓展。下一个阶段，打算先学习android端的native app后过渡到跨平台的React Native框架，同时学习数据库管理。
