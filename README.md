# Nuxt Tutorial

## 安装

* 新手可以通过 vue-cli 安装：
    * `vue init nuxt-community/starter-template <project-name>`
    * `cd <project-name>`
    * `npm install`
    * `npm run dev`
* 老鸟可以通过 npm 来开始新的项目：
    * `mkdir <project-name>`
    * `cd <project-name>`
    * `touch package.json`
    * `npm install`
    * `touch nuxt.config.js`
    * `npm run dev`
  
```
// package.json
{
    "name": "nuxt-tutorial",
    "dependencies": {
        "nuxt": "latest"
    },
    "config": {
        "nuxt": {
            "port": "3333"
        }
    },
    "scripts": {
        "dev": "nuxt",
        "build": "nuxt build",
        "start": "nuxt start"
    }
}
```

  
```
// nuxt.config.js
module.exports = {
    srcDir: 'src/'
}
```

## 项目结构
```
├── assets/
├── components/
├── layouts/
├── middleware/
├── pages/
├── plugins/
├── static/
├── store/
├── .editorconfig
├── .eslintrc.js
├── .gitignore
├── nuxt.config.js
├── package.json
```

* assets：需要通过 Webpack 处理的金泰资源文件
    * sass、less、小的图片
* static：不需要通过 Webpack 处理的静态资源文件
    * 大的图片、robots.txt、sitemap.xml
* middleware：中间件允许您定义一个自定义函数运行在一个页面或一组页面渲染之前
* layouts：个性化布局文件

## nuxt.config.js

* build：自定义 webpack 的构建配置
* cache：
* css：配置全局 CSS 文件、模块、库（每个页面都会被引入）
* dev：配置是开发模式还是生产模式
* env：配置在客户端和服务端共享的环境变量
* generate：配置生成静态站点
* head：配置 meta 信息
* loading：配置进度条
* performance：性能选项
* srcDir：设置源码目录
* ...

## 路由
在 Nuxt.js 中路由是根据 `pages` 目录自动生成的。

### 基础路由
假设 `pages` 的目录结构如下：

```
pages/
├── place
│   ├── index.vue
│   ├── spaces.vue
├── index.vue
```

那么，Nuxt.js 自动生成的路由配置如下：

```
router: {
    routes: [{
        name: 'index',
        path: '/',
        component: 'pages/index.vue'
    }, {
        name: 'place',
        path: '/place',
        component: 'pages/place/index.vue'
    }, {
        name: 'place-spaces',
        path: '/place/spaces',
        component: 'pages/place/spaces.vue'
    }]
}
```

### 动态路由
在 Nuxt.js 里面定义带参数的动态路由，需要创建对应的以下划线作为前缀的 Vue 文件 或 目录。

```
pages/
├── place
│   ├── _id.vue
├── _teambuild
│   ├── index.vue
├── index.vue
```

Nuxt.js 自动生成的路由配置如下：

```
router: {
    routes: [{
        name: 'index',
        path: '/',
        component: 'pages/index.vue'
    }, {
        name: 'place-id',
        path: '/place/:id?',
        component: 'pages/place/_id.vue'
    }, {
        name: 'teambuild',
        path: '/:teambuild',
        component: 'pages/_teambuild/index.vue'
    }]
}
```

### 其它参考文档

## 视图

### 模板
定制化默认的 html 模板，只需要在应用根目录下创建一个 app.html 的文件。

默认模板为：

```
<!DOCTYPE html>
<html {{ HTML_ATTRS }}>
    <head>
        {{ HEAD }}
    </head>
    <body {{ BODY_ATTRS }}>
        {{ APP }}
    </body>
</html>
```

### 布局
可通过添加 `layouts/default.vue` 文件来扩展应用的默认布局。

默认布局的源码如下：
```
<template>
    <nuxt/>
</template>
```

### 个性化布局
`layouts` 根目录下的所有文件都属于个性化布局文件，可以在页面组件中利用 layout 属性来引用。

例如 `layouts/blog.vue`：
```
<template>
    <div>
        <div>导航</div>
        <nuxt/>
    </div>
</template>
```

在 `pages/posts.vue` 里， 可以指定页面组件使用 blog 布局。
```
<template>
    <div>
        <h1>Hello Blog</h1>
    </div>
</template>
<script>
export default {
    layout: 'blog'
}
</script>
```

### 默认 Meta 标签
Nuxt.js 允许你在 nuxt.config.js 里定义应用所需的所有默认 meta 标签，在 head 字段里配置就可以了：

```
head: {
    meta: [
        { charset: 'utf-8' },
        { name: 'viewport', content: 'width=device-width, initial-scale=1' }
    ],
    link: [
        { rel: 'stylesheet', href: 'https://fonts.googleapis.com/css?family=Roboto' }
    ]
}
```

### 个性化特定页面的 Meta 标签

我们可以在页面中通过 head 方法来设置当前页面的头部标签。

```
<template>
    <h1>{{ title }}</h1>
</template>

<script>
export default {
    data () {
        return {
            title: 'Hello World!'
        }
  },
  head () {
        return {
            title: this.title,
            meta: [
                { hid: 'description', name: 'description', content: 'My custom description' }
            ]
        }
    }
}
</script>
```

## 异步数据
Nuxt.js 扩展了 Vue.js，增加了一个叫 asyncData 的方法，使得我们可以在设置组件的数据之前能异步获取或处理数据。

### asyncData 方法
`asyncData` 方法会在组件（限于页面组件）每次加载之前被调用。

Nuxt.js 提供了几种不同的方法来使用 asyncData 方法，你可以选择自己熟悉的一种来用：

* 返回 Promise

```
export default {
    asyncData ({ params }) {
    return axios.get(`https://my-api/posts/${params.id}`)
        .then((res) => {
        return { title: res.data.title }
        })
    }
}
```

* 使用 async或await

```
export default {
    async asyncData ({ params }) {
        let { data } = await axios.get(`https://my-api/posts/${params.id}`)
        return { title: data.title }
    }
}
```

* 使用 回调函数

```
export default {
    data () {
        return { project: 'default' }
    },
    asyncData (context) {
        return { context: context }
    }
}
```

### 上下文

```
export default {
    data () {
        return { project: 'default' }
    },
    asyncData (context) {
        return { project: 'nuxt' }
    }
}
```


