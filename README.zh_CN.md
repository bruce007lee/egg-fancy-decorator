# egg-decorator

[![NPM version][npm-image]][npm-url]
[![Test coverage][codecov-image]][codecov-url]
[![npm download][download-image]][download-url]

[npm-image]: https://img.shields.io/npm/v/egg-fancy-decorator.svg?style=flat-square
[npm-url]: https://npmjs.org/package/egg-fancy-decorator
[codecov-image]: https://img.shields.io/codecov/c/github/bruce007lee/egg-fancy-decorator.svg?style=flat-square
[codecov-url]: https://codecov.io/github/bruce007lee/egg-fancy-decorator?branch=main
[david-url]: https://david-dm.org/eggjs/egg-fancy-decorator
[snyk-image]: https://snyk.io/test/npm/egg-fancy-decorator/badge.svg?style=flat-square
[snyk-url]: https://snyk.io/test/npm/egg-fancy-decorator
[download-image]: https://img.shields.io/npm/dm/egg-fancy-decorator.svg?style=flat-square
[download-url]: https://npmjs.org/package/egg-fancy-decorator

<!--
Description here.
-->

为 egg 框架提供一些有用的修饰器

- @RequestMapping: 和`spring-boot`中的`@RequestMapping`用法类似
- @ResponseBody: 和`spring-boot`中的`@ResponseBody`用法类似


## 依赖说明

### 依赖的 egg 版本

| egg-decorator 版本 | egg 1.x |
| ------------------ | ------- |
| 1.x                | 😁      |
| 0.x                | ❌      |

## 开启插件

```js
// config/plugin.js
exports.fancyDecorator = {
  enable: true,
  package: 'egg-fancy-decorator',
};
```

## 使用场景

原来在 egg 中使用 router 和 controller 的方式

```typescript
// router.ts
import { Application } from 'egg';

export default (app: Application) => {
  const { controller, router } = app;
  router.get('/api/project/list', controller.project.list);
  router.post('/api/project/find', controller.project.find);
  router.post('/api/project/find2', controller.project.find);
};
```

```typescript
// controller/test.ts
import { Controller } from 'egg';

export default class TestController extends Controller {
  public async list() {
    const { ctx } = this;
    ctx.body = await ctx.service.project.list();
  }

  public find() {
    const { ctx } = this;
    ctx.body = 'test find ' + ctx.request.href;
  }
}
```

通过 fancy-decorator 插件的修饰器可以简化成(和 java 的 spring-boot 中使用类似 😀):

```typescript
// controller/test.ts
// 如果你使用 RequestMapping 和 ResponseBody, router.js 中的路由配置可以直接省略。
import { Controller } from 'egg';
import { RequestMapping, ResponseBody, RequestMethod } from 'egg-fancy-decorator';

export default class TestController extends Controller {
  /**
   * RequestMapping 简单的方式为方法直接定义个路由
   * ResponseBody 可以简化返回的处理
   */
  @RequestMapping('/api/project/list')
  @ResponseBody
  public async list() {
    const { ctx } = this;
    return await ctx.service.project.list();
  }

  /**
   * 一个可以定义多个路由路径或请求的方式
   * method的设置可省略，默认是get的方式
   */
  @RequestMapping({ value: ['/api/project/find', '/api/project/find2'], method: RequestMethod.POST })
  @ResponseBody
  public find() {
    const { ctx } = this;
    return 'test find ' + ctx.request.href;
  }
}
```

## 详细配置

请到 [config/config.default.js](config/config.default.js) 查看详细配置项说明。

## 提问交流

请到 [issues](https://github.com/bruce007lee/egg-fancy-decorator/issues) 异步交流。

## License

[MIT](LICENSE)