---
name: cm-dev:core
description: "HarmonyOS 参考信息中心 — 环境配置、编译错误速查、ArkTS 规范、组件索引"
---

# cm-dev:core — 参考信息

## 环境配置

| 缺少 | 操作 |
|------|------|
| hvigor | `npm install -g @ohos/hvigor` |
| DevEco Studio | https://developer.huawei.com/consumer/cn/deveco-studio/ |
| hvigorw 路径 | `/Applications/DevEco-Studio.app/Contents/tools/hvigor/bin/hvigorw` |

## 编译错误速查

| 错误 | 原因 | 修复 |
|------|------|------|
| `Classes cannot be used as objects` | class 当对象赋值 | 用实例或接口 |
| `UI component 'Divider' cannot be used` | Divider 在 ForEach 内同级 | 包在 Column 内 |
| `Cannot find name 'CmColors'` | 缺少 import | 添加 import |
| `'getContext' has been deprecated` | API 废弃 | 用 `this.context` 或传参 UIContext |
| `Property does not exist on type` | 接口定义不完整 | 检查接口字段和 import |
| `Object is possibly 'undefined'` | 可选链没处理 | 用 `?.` 或守卫条件 |
| `'back' has been deprecated` | NavPathStack.back() 废弃 | 用 `NavPathStack.pop()` |
| `Duplicate file names detected` | 文件名重复 | 检查重名文件 |

## ArkTS 限制速查

- 无 `any`/`unknown` — 用明确类型
- 无 `for...in` — 用 `for` 循环
- 无 `obj["field"]` — 用 `obj.field`
- 无 destructuring — 逐字段赋值
- 无 nested function — 用 lambda
- 无 `var` — 用 `const`/`let`
- Divider 不能在 ForEach 内作为同级节点 — 包在 Column 内
- V2 状态管理：`@Local` / `@Param` / 禁止 V1

## 开发原则（铁律）

1. **能用 ark-uikit 的组件，绝对用 ark-uikit**
2. **没有的组件才回退到原生 ArkUI**
3. **所有应用共享同一套 UIKit**
4. **新增组件必须先加到 ark-uikit**
