# hypha-agent-node-benchmark

对 **Hypha Agent 节点体系** 进行能力基准测试的实验仓库。

## 仓库用途

- 以主流评测基准对标、评估 Hypha Agent 节点体系（18 个节点）的能力表现。
- 建立「节点 → benchmark 任务」的映射关系，分析节点体系在各基准上的覆盖度与缺口。

## 节点体系

Agent 节点体系共 **4 个一级节点（Worker Type）、18 个二级节点（Node Type）**：

| 一级节点 | 二级节点 |
|---------|---------|
| REASONING | INFER / DELIBERATE / REFLECT / SAMPLE |
| CONTEXT | LOAD / SELECT / UPDATE / COMPRESS / RESET |
| MEMORY | RETRIEVE / WRITE / UPDATE / CONSOLIDATE / EVICT |
| INTERACTION | ACT / OBSERVE / COMMUNICATE / OUTPUT |

完整定义见 [docs/node-system.md](docs/node-system.md)。

## 实验目的

1. 选定基准套件：ToolBench（工具调用）、BigBench（通用推理）、lm-eval（语言模型评测）、OpenCompass（综合评测）。
2. 对 18 个节点逐一做能力归类，映射到各基准的任务类型（见 `mappings/`）。
3. 通过缺口分析（见 `docs/node-gap-analysis.md`），识别节点体系在现有基准中无法覆盖或覆盖不足的能力，为后续评估与改进提供依据。

## 目录结构

```
hypha-agent-node-benchmark/
├── README.md
├── docs/                    # 各基准与环境说明、节点体系、缺口分析
├── experiments/             # 各基准的实验脚本与结果（toolbench/bigbench/lm-eval/opencompass）
├── mappings/                # 节点 → benchmark 任务映射
└── scripts/                 # 通用脚本
```

## 当前阶段范围（Phase 1）

当前处于**第一阶段：实验仓库初始化 + ToolBench 环境准备**。本阶段仅完成：

- 仓库初始化与文档骨架搭建
- 节点体系定义（`docs/node-system.md`）
- ToolBench 仓库 clone 与 Conda 环境准备

**本阶段不做**：运行 benchmark、下载模型、修改 ToolBench 源码、自行增改节点体系。

## 阶段规划

- **Phase 1（当前）**：仓库初始化 + ToolBench 环境准备
- **Phase 2+**：依次接入 BigBench、lm-eval、OpenCompass，完成节点映射与缺口分析
