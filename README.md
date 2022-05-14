# reactViteMock
> [Mockjs](https://github.com/nuysoft/Mock) :模拟数据生成器，通过随机数据,模拟各种场景。方便前端在没有后端接口的情况，开发环境也能进行接口数据对接。

## 说明
前段时间,开源了一个React中后台项目[arco-admin-template](https://github.com/hu-snail/arco-admin-template),其中使用到了`Mockjs`,看了很多案例都是介绍`开发环境`中的使用，关于`生产环境`中使用注意事项较少。所以决定将两个场景结合写一篇案例分享。
## 技术栈
- [React](https://react.docschina.org/): `^18.0.0`
- [Axios](https://github.com/axios/axios):`^0.26.1`
- [Mockjs](https://github.com/nuysoft/Mock)：`^1.1.0`
- [Vite](https://vitejs.dev/):`^2.9.9`
- [Vite-plugin-mock](https://github.com/vbenjs/vite-plugin-mock): `^2.9.6`

## 构建项目
使用[vite](https://cn.vitejs.dev/guide/#scaffolding-your-first-vite-project)构建`React`项目，可参考vite文档进行构建
```sh
# 1.使用`vite`构建React项目
yarn create vite reactViteMock --template react
# OR npm create vite@latest reactViteMock --template react
# 2.进入项目根目录
cd reactViteMock
# 3.0 安装依赖
yarn # OR npm install
# 4.0 运行项目
yarn dev # OR npm run dev
```

## 安装插件
安装`vite`相关插件
```sh
yarn add mockjs -S # OR npm install mockjs
yarn add vite-plugin-mock -D # OR npm install vite-plugin-mock
```
安装`axios`用于数据请求
```sh
yarn add axios # OR npm install axios 
```

## 目录结构
根据项目目录，创建对应的文件
```sh
└── src # 主目录
    ├── api # 存放所有api接口文件
    │   ├── user.js # 用户接口
    │   ├── list.js # 列表接口
    ├── mockProdServer.js # 处理生产环境mock数据
    ├── config # 配置文件
    │   ├── net.config.js # axios请求配置
    ├── utils # 工具类文件
    │   ├── request.js # axios请求封装
└── mock # mock模拟数据文件
    ├── user.js # 用户模拟数据
    ├── list.js # 列表数据

```
## 创建Mock模拟数据
在`mock`目录下的`user.js`和`list.js`文件中添加模拟数据，可根据自己需求创建模拟数据
- ### `user.js` 用户模块
创建了`用户登录`和`退出登录`模拟数据
```js
const accessTokens = {
    admin: 'admin-accessToken',
    editor: 'editor-accessToken',
    test: 'test-accessToken'
  };
  
  export default [
    {
      url: '/api/login',
      method: 'post',
      response: (config) => {
        const { username } = config.body;
        const accessToken = accessTokens[username];
        if (!accessToken) {
          return {
            code: 500,
            msg: '帐户或密码不正确。'
          };
        }
        return {
          code: 200,
          msg: 'success',
          data: {
            accessToken
          }
        };
      }
    },
    {
      url: '/api/logout',
      type: 'post',
      response() {
        return {
          code: 200,
          msg: 'success'
        };
      }
    }
  ];
```
- ### `list.js`模拟数据
包含了`社区列表`和`学习社区列表`模拟数据
```js
const list = [
  {
    title: '掘金',
    desc: '一个帮助开发者成长的社区',
    url: 'https://juejin.cn/',
    logo: 'https://qiqihao.oss-cn-beijing.aliyuncs.com/static/coderutil/icon/juejin.png'
  },
  {
    title: 'SF思否',
    desc: '思否是中国领先的开发者技术社区',
    url: 'https://segmentfault.com/',
    logo: 'http://coderutil.oss-cn-beijing.aliyuncs.com/bbs-image/file_87feae864e274579824d7398a588e042.png'
  },
  {
    title: 'CSDN',
    desc: '中文最大的技术社区',
    url: 'https://www.csdn.net/',
    logo: 'https://qiqihao.oss-cn-beijing.aliyuncs.com/static/coderutil/icon/csdn.png'
  },
  {
    title: '开源中国',
    desc: '目前国内最大的开源技术社区',
    url: 'https://www.oschina.net/',
    logo: 'https://qiqihao.oss-cn-beijing.aliyuncs.com/static/coderutil/icon/oschina.ico'
  },
  {
    title: 'StackOverflow',
    desc: '全球最大的技术问答社区',
    url: 'https://stackoverflow.com/',
    logo: 'https://qiqihao.oss-cn-beijing.aliyuncs.com/static/coderutil/icon/stackoverflow.svg'
  },
  {
    title: '菜鸟教程',
    desc: '学的不仅是技术，更是梦想',
    url: 'https://www.runoob.com/',
    logo: 'https://static.runoob.com/images/favicon.ico'
  }
];

const studyList = [
  {
    title: 'GitHub',
    desc: '世界最大的开源代码共享社区',
    url: 'https://github.com/',
    logo: 'https://qiqihao.oss-cn-beijing.aliyuncs.com/static/coderutil/icon/github.svg'
  },
  {
    title: 'Gitchat',
    desc: 'IT知识分享平台',
    url: 'https://gitbook.cn/',
    logo: 'https://qiqihao.oss-cn-beijing.aliyuncs.com/static/coderutil/icon/gitchat.png'
  },
  {
    title: 'Gitee',
    desc: '中国最大的开源代码共享社区',
    url: 'https://gitee.com/',
    logo: 'https://qiqihao.oss-cn-beijing.aliyuncs.com/static/coderutil/icon/gitee.png'
  },
  {
    title: '慕课网',
    desc: '程序员的梦工厂',
    url: 'https://www.imooc.com/',
    logo: 'https://qiqihao.oss-cn-beijing.aliyuncs.com/static/coderutil/icon/imooc.png'
  },
  {
    title: '掘金',
    desc: '一个帮助开发者成长的社区',
    url: 'https://juejin.cn/',
    logo: 'https://qiqihao.oss-cn-beijing.aliyuncs.com/static/coderutil/icon/juejin.png'
  },
  {
    title: 'CSDN',
    desc: '中文最大的技术社区',
    url: 'https://www.csdn.net/',
    logo: 'https://qiqihao.oss-cn-beijing.aliyuncs.com/static/coderutil/icon/csdn.png'
  }
];

export default [
  {
    url: '/api/getResouceList',
    type: 'get',
    response() {
      return {
        code: 200,
        msg: 'success',
        data: {
          list, list, studyList
        }
      };
    }
  }
];
```

## Axios请求配置
在创建模拟接口之前，我们先得封装`axios`请求和完善`net.config.js`,
- Axios封装所在目录路径`src/utils/reqeust.js`,删减了一些代码，可根据自己的业务优化调整
```js
/**
 * @description axios请求封装
 * @author hu-snail 1217437592@qq.com
 */

 import axios from 'axios';
 import config from '@/config/net.config';
 let tokenLose = true;
 
 const { baseURL, successCode, invalidCode, requestTimeout, contentType } = config;
 
 const instance = axios.create({
   baseURL,
   timeout: requestTimeout,
   headers: {
     'Content-Type': contentType
   }
 });
 
 // request interceptor
 instance.interceptors.request.use(
   (configItem) => configItem,
   (error) =>
     // do something with request error
     Promise.reject(error)
 );
 
 // response interceptor
 instance.interceptors.response.use(
   /**
    * If you want to get http information such as headers or status
    * Please return  response => response
    */
   (response) => {
     const res = response.data;
     // 请求出错处理
     // -1 超时、token过期或者没有获得授权
     if (res.code === invalidCode && tokenLose) {
       tokenLose = false;
       // 根据自己业务需求调整代码
     }
 
     if (successCode.indexOf(res.code) === -1) {
       console.error(res.msg);
       return Promise.reject(res);
     }
     return res;
   },
   (error) => {
    console.error('请求出错啦！');
     return Promise.reject(error);
   }
 );
 
 export default instance;
```
- `net.config.js`所在目录`src/config`，具体代码如下：
```js
export default {
    //  axios 基础url地址
    baseURL: process.env.NODE_ENV === 'development' ? 'http://localhost:3001/api' : '/api',
    // 操作正常返回的code,根据后端第一
    successCode: [200, 0],
    // 超时时间
    requestTimeout: 10 * 1000,
    // 请求content_type类型,根据后端配置定义
    contentType: 'application/json;charset=UTF-8',
    // 登录失效code
    invalidCode: -1
  };
```

## 创建Api模拟接口
完成以上配置后，就可以开始完善`api`请求接口了
- ### `user.js`用户管理
需要说明的是`@`使用了`vite`别名配置,可参考完成案例[reactViteMock](https://github.com/hu-snail/reactViteMock)
```js
import request from '@/utils/request.js';
export const login = async (data) =>
  request({
    url: '/login',
    method: 'post',
    data
});

export const logout = () =>
  request({
    url: '/logout',
    method: 'post'
});
```
- ### `list.js`列表管理
```js
import request from '@/utils/request.js';

export const getResouceList = () =>
  request({
    url: '/getResouceList',
    method: 'get'
  });
```
## 完善`mockProdServe.js`代码
完成以上配置后，接下来完善`mockProdServe.js`文件，这个文件主要是处理生产环境中使用mock数据，如果不需要可以忽略此模块代码，目录路径：`src/mockProdServe.js`
```js
import { createProdMockServer } from 'vite-plugin-mock/es/createProdMockServer';
// 逐一导入您的mock.js文件
// 如果使用vite.mock.config.js，只需直接导入文件
// 可以使用 import.meta.glob功能来进行全部导入
import userMock from '../mock/user';
import listMock from '../mock/list';

export const setupProdMockServer = () => {
  createProdMockServer([...userMock, ...listMock]);
};
```
## 完善`vite-config.js`配置
完成以上操作后，到了最重要的环节，配置`vite-config.js`，完整配置如下
```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'
// 引入vite-plugin-mock
import { viteMockServe } from 'vite-plugin-mock';

const path = require('path');
// 判断环境
const isDev = process.env.NODE_ENV === 'development';

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [
    react(),
    // mock配置
    viteMockServe({
      mockPath: 'mock', // mock目录地址 demo中创建的是mock
      localEnabled: isDev, // 是否在开发环境中启用
      prodEnabled: !isDev, // 是否在生产环境中启用
      supportTs: false, // 是否支持TS
      watchFiles: true, // 监听文件
      // 添加处理生产环境文件
      injectCode: `
          import { setupProdMockServer } from './mockProdServer';
          setupProdMockServer();
        `,
      // 添加到`src/main.jsx`文件中，比较重要的一步，不然在生产环境不生效
      injectFile: path.resolve(process.cwd(), 'src/main.jsx')
    })
  ],
  resolve: {
    alias: {
      '~': path.resolve(__dirname, './'), // 根路径
      '@': path.resolve(__dirname, 'src') // src 路径
    }
  },
  server: {
    port: 3001,
    host: '0.0.0.0',
    open: true
    // proxy: {
    //   "/api": {
    //     target: "https://624659e7e3450d61b0fd6ba2.mockapi.io/api/v1",
    //     changeOrigin: true,
    //     rewrite: (path) => path.replace(/^\/dev-api/, ""),
    //   },
    // },
  },
})

```
需要主要的是`process.env.NODE_ENV`需要在`package.json`文件中修改`scripts`中的`dev`和`build`配置
```json
"scripts": {                                                 
 "dev": "cross-env NODE_ENV=development vite",  
 "build": "cross-env NODE_ENV=production vite build",
}
```
此时发现配置中多了`cross-env`参数，主要作用是解决`NODE_ENV`设置环境变量时，Windows命令提示将会阻塞(报错)。具体详细作用可以自行查阅文档。为了解决这个问题，我们需要安装`cross-env`
```sh
yarn add cross-env -D # Or npm install cross-env -D
```
另外需要主要的是`viteMockServe`配置中`injectCode`和`injectFile`两个配置项，`injectCode`意思是注入代码,将`mockProdServer.js`中的代码以注入方式添加到文件中，`injectFile`意思是注入文件，也就是注入到`src/main.jsx`文件中。

## 使用
在`App.jsx`中使用，完整代码如下：
```jsx
import { useState } from 'react'
import {getResourceList} from '@/api/list'

function App() {
  // 社区资源
  const [communityList, setCommunityList] = useState([])
  // 学习社区资源
  const [studyCommunityList, setStudyCommunityList] = useState([])
  const handleGetResourceList = () => {
    getResourceList().then(res => {
      const {list, studyList} = res.data
      setCommunityList(list)
      setStudyCommunityList(studyList)
    })
  }
  return (
    <div className="App">
      <header className="App-header">
        <img src={logo} className="App-logo" alt="logo" />
        <p>Vite + React使用Mockjs案例！！！</p>
      </header>
      <button className='btn' type="button" onClick={handleGetResourceList}>
          获取Mock资源列表
      </button>
      { communityList.length ? <h2 className='list-title'>资源列表</h2> : ''}
      <div className='list-wrap'>
       
        {
          communityList.map((item, index) => {
            return (
              <div className='item' key={index}>
                <div className="item-logo">
                  <img className='logo' src={item.logo} width="100%" alt={item.title} />
                </div>
                <div className="item-content">
                  <div className="title">{item.title}</div>
                  <div className="desc">{item.desc}</div>
                </div>
              </div>

            )
          })
        }
        {
          studyCommunityList.map((item, index) => {
            return (
              <div className='item' key={index}>
                <div className="item-logo">
                  <img src={item.logo} className="logo" width="100%" alt={item.title} />
                </div>
                <div className="item-content">
                  <div className="title">{item.title}</div>
                  <div className="desc">{item.desc}</div>
                </div>
              </div>

            )
          })
        }
      </div>
    </div>
  )
}

export default App
```
## 截图预览

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/0eaaf8e7dd02488bb7c306bc5b6e8fbb~tplv-k3u1fbpfcp-watermark.image?)
完整代码地址github地址：[reactViteMock](https://github.com/hu-snail/reactViteMock)

## 往前文章
- [🔥 基于 Vite构建react开源中后台项目，支持Mock 数据、国际化、暗夜模式切换](https://juejin.cn/post/7094444227727196196)
- [vue3最全资源库来袭，包含（web、移动、小程序、Electron、常用库、面试、文档工具）等资源](https://juejin.cn/post/7090501194228957221)
- [Vue前端直传至阿里云OSS（支持断点续传，分片上传，批量上传）](https://juejin.cn/post/7077751294223450143)
