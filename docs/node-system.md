# Agent 节点体系标准（18 个）

本文档定义 Agent 节点体系的官方标准。**后续所有 benchmark 的能力映射与缺口分析，均以本文件定义的 18 个节点为唯一标准。**

## 节点清单

### 一级节点（Worker Type）：4 个

| 一级节点（Worker Type） | 二级节点（Node Type） |
|------------------------|----------------------|
| REASONING | REASONING.INFER |
| REASONING | REASONING.DELIBERATE |
| REASONING | REASONING.REFLECT |
| REASONING | REASONING.SAMPLE |
| CONTEXT | CONTEXT.LOAD |
| CONTEXT | CONTEXT.SELECT |
| CONTEXT | CONTEXT.UPDATE |
| CONTEXT | CONTEXT.COMPRESS |
| CONTEXT | CONTEXT.RESET |
| MEMORY | MEMORY.RETRIEVE |
| MEMORY | MEMORY.WRITE |
| MEMORY | MEMORY.UPDATE |
| MEMORY | MEMORY.CONSOLIDATE |
| MEMORY | MEMORY.EVICT |
| INTERACTION | INTERACTION.ACT |
| INTERACTION | INTERACTION.OBSERVE |
| INTERACTION | INTERACTION.COMMUNICATE |
| INTERACTION | INTERACTION.OUTPUT |

- 一级节点（Worker Type）共 **4 个**：REASONING、CONTEXT、MEMORY、INTERACTION。
- 二级节点（Node Type）共 **18 个**：即上表全部。

### 二级节点（Node Type）：18 个

| 序号 | 二级节点 | 所属一级节点 |
|------|---------|-------------|
| 1 | REASONING.INFER | REASONING |
| 2 | REASONING.DELIBERATE | REASONING |
| 3 | REASONING.REFLECT | REASONING |
| 4 | REASONING.SAMPLE | REASONING |
| 5 | CONTEXT.LOAD | CONTEXT |
| 6 | CONTEXT.SELECT | CONTEXT |
| 7 | CONTEXT.UPDATE | CONTEXT |
| 8 | CONTEXT.COMPRESS | CONTEXT |
| 9 | CONTEXT.RESET | CONTEXT |
| 10 | MEMORY.RETRIEVE | MEMORY |
| 11 | MEMORY.WRITE | MEMORY |
| 12 | MEMORY.UPDATE | MEMORY |
| 13 | MEMORY.CONSOLIDATE | MEMORY |
| 14 | MEMORY.EVICT | MEMORY |
| 15 | INTERACTION.ACT | INTERACTION |
| 16 | INTERACTION.OBSERVE | INTERACTION |
| 17 | INTERACTION.COMMUNICATE | INTERACTION |
| 18 | INTERACTION.OUTPUT | INTERACTION |

## 标准约定（约束规则）

1. **唯一标准**：后续所有 benchmark 的能力映射，必须以这 18 个节点为标准，不得使用其他节点名称。
2. **禁止新增**：不自行增加新的节点名称。
3. **潜在缺失节点**：若发现某种行为无法被现有 18 个节点准确表达，只记录为「潜在缺失节点」，不直接修改节点体系。
4. **接口规范已固定**：节点名、输入输出、抽象类均已固定；本阶段任务是验证覆盖情况，不是重新设计接口。
