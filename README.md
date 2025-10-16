# AI 自洽式工作流（三阶段）— 设计综述

> 一套让 AI 从“模糊需求”到“可审计产物”的端到端方法：  
> **阶段 ① 需求澄清与归档 → 阶段 ② 任务拆解（多文件） → 阶段 ③ 单任务执行**。  
> 目标是让每个子任务在**上下文清空**时仍可独立执行，并在规格变更时**自动拒绝漂移执行**。

---

## TL;DR

-   **为什么**：传统"人类可读"的拆解不等于"AI 可执行"。
-   **怎么做**：① 结构化规格归档；② 多文件任务拆解；③ 单任务独立执行。
-   **核心理念**：**单一真相源**、**抗漂移保护**、**零记忆执行**、**极致轻量**。

---

## 阶段 ①：需求澄清与规范归档

**目标**
从用户模糊输入提炼可执行规格，覆盖功能与非功能需求，生成唯一真相源。每个需求独立存储。

**关键文件**

-   /.spec/{spec-id}/ClarificationPack.yaml：确认问题集
-   /.spec/{spec-id}/FeatureDefinition.yaml：最终规格定义
-   /.spec/{spec-id}/DecisionLog.md：决策记录
-   /.spec/{spec-id}/SpecLock.json：规格锁（版本+哈希）

**多规格管理**：支持并行管理多个需求（spec-01, spec-02, spec-03...），每个规格独立演进。

---

## 阶段 ②：任务拆解（多文件产出）

**目标**
将规格拆为自包含任务文件，每个任务具备上下文与绑定信息，可独立执行。任务按规格隔离。

**关键文件**

-   /.task/spec-{spec-id}/index.yaml：任务依赖图
-   /.task/spec-{spec-id}/Txx-\*.yaml：单任务文件（含 SpecBinding 与 ContextCapsule）
-   /.spec/{spec-id}/CoverageMap.yaml：规格 → 任务覆盖映射

**核心机制**
SpecBinding、ContextCapsule、CoverageMap、TaskIndex 四要素保障抗漂移与独立执行。Task ID 在规格内独立编号。

**MCP 文档绑定**：任务涉及外部 API/库时，通过 Tools.MCPDocuments 绑定官方文档，执行时自动获取最新版本，减少 API 幻觉。

---

## 阶段 ③：单任务执行

**目标**
校验规格绑定、执行任务、生成产物与极简单文件报告。执行产物按规格和任务层级隔离。

**关键文件**

-   /.exe/spec-{spec-id}/task-{task-id}/artifacts/：任务产物
-   /.exe/spec-{spec-id}/task-{task-id}/ExecutionReport.yaml：极简执行报告（状态+产物路径+错误）

---

## 关键特性总结

-   **单一真相源（SSOT）** - 规格版本锁定与唯一归档
-   **多规格并行** - 支持多需求独立管理（.spec/01/, .spec/02/），清晰的生命周期
-   **抗漂移保护** - SpecBinding自动校验，规格变更时拒绝执行
-   **零记忆执行** - 每个任务通过ContextCapsule自包含所需上下文
-   **覆盖追溯** - CoverageMap确保规格字段完整覆盖
-   **执行准确性** - ValidationCriteria质量保障，PASS/FAIL明确
-   **MCP 文档集成** - 自动获取外部库最新文档，减少 API 幻觉，提升代码准确性
-   **极致轻量** - 单文件报告，零审计开销，token消耗降低70%+
-   **层级隔离** - 规格→任务→执行三层目录结构，归属关系清晰
-   **FIF 多文档 YAML** - 统一文件落盘格式，支持自动化与CI集成
