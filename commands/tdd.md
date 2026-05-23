---
name: cm-dev:tdd
description: "HarmonyOS TDD 循环 — RED-GREEN-REFACTOR。无测试代码 → 无实现代码。ArkTS 测试规范见 arkts/testing.md"
---

# cm-dev:tdd — TDD 循环

## 铁律

```
无测试代码 → 无实现代码
没看到测试失败 → 不算 TDD
```

## RED-GREEN-REFACTOR

```dot
digraph tdd_cycle {
    node [shape=box, style=rounded];
    red [label="RED\n写失败测试" fillcolor="#ffcccc" style=filled,rounded];
    verify_red [label="编译验证\n确认测试失败" shape=diamond];
    green [label="GREEN\n最小实现" fillcolor="#ccffcc" style=filled,rounded];
    verify_green [label="编译验证\n确认测试通过" shape=diamond];
    refactor [label="REFACTOR\n清理，保持 GREEN" fillcolor="#cce5ff" style=filled,rounded];
    done [label="✓ 下一轮 / verify"];

    red -> verify_red;
    verify_red -> green [label="失败 ✓"];
    verify_red -> red [label="没失败?\n测试没测到点" style=dashed];
    green -> verify_green;
    verify_green -> refactor [label="通过 ✓"];
    verify_green -> green [label="没通过?\n实现不对" style=dashed];
    refactor -> done;
}
```

### RED — 写失败测试

```typescript
it('描述具体行为', 0, () => {
  const input = ...
  const result = service.method(input)
  expect(result).assertEqual(expected)
})
```

**验证 RED（不可跳过！）**
```bash
hvigor --mode module -p module=entry -p product=default assembleHap
```
确认：编译失败，因为**功能未实现**（不是语法错误）。

### GREEN — 最小实现

只写通过测试的代码。不加 YAGNI 功能。

**验证 GREEN**
```bash
hvigor --mode module -p module=entry -p product=default assembleHap
```
确认：编译通过，测试通过。

### REFACTOR — 清理

不改变功能，优化代码结构。保持测试 GREEN。

---

## Bug 修复 TDD

1. 写复现 bug 的测试 → 验证 RED
2. 修复 → 验证 GREEN
3. 测试永久保留作回归用例

## 常见借口（都不接受）

| 借口 | 现实 |
|------|------|
| "太简单了不需要测试" | 简单代码也会崩 |
| "我之后补测试" | 之后补的证明不了什么 |
| "先写实现再看测试" | 后写测试是验证实现，不是验证需求 |
| "我已经手动测过了" | 手动不可复现不可回归 |
| "删掉 X 小时工作太浪费" | 沉没成本，留着不可信代码更浪费 |

## 引导到下一步

Task 完成后使用 `cm-dev:verify` 执行自检。所有 Task 完成后提 PR。
