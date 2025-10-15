# AI 自洽式工作流（三阶段）— 设计综述

> 一套让 AI 从“模糊需求”到“可审计产物”的端到端方法：  
> **阶段 ① 需求澄清与归档 → 阶段 ② 任务拆解（多文件） → 阶段 ③ 单任务执行**。  
> 目标是让每个子任务在**上下文清空**时仍可独立执行，并在规格变更时**自动拒绝漂移执行**。

---

## TL;DR

-   **为什么**：传统"人类可读"的拆解不等于"AI 可执行"。
-   **怎么做**：① 结构化规格归档；② 多文件任务拆解；③ 单任务独立执行。
-   **核心理念**：**单一真相源（SSOT）**、**抗漂移**、**零记忆可执行**、**轻量高效**。

---

## 阶段 ①：需求澄清与规范归档

**目标**  
从用户模糊输入提炼可执行规格，覆盖功能与非功能需求，生成唯一真相源。

**关键文件**

-   ClarificationPack.yaml：确认问题集
-   FeatureDefinition.yaml：最终规格定义
-   DecisionLog.md：决策记录
-   SpecLock.json：规格锁（版本+哈希）

---

## 阶段 ②：任务拆解（多文件产出）

**目标**  
将规格拆为自包含任务文件，每个任务具备上下文与绑定信息，可独立执行。

**关键文件**

-   /tasks/index.yaml：任务依赖图
-   /tasks/Txx-\*.yaml：单任务文件（含 SpecBinding 与 ContextCapsule）
-   /specs/CoverageMap.yaml：规格 → 任务覆盖映射

**核心机制**  
SpecBinding、ContextCapsule、CoverageMap、TaskIndex 四要素保障抗漂移与独立执行。

---

## 阶段 ③：单任务执行

**目标**
校验规格绑定、执行任务、生成产物与轻量级报告。

**关键文件**

-   /artifacts/<task-id>/：任务产物
-   /runs/<task-id>/ExecutionReport.yaml：轻量执行报告（状态+产物路径+错误）
-   /runs/<task-id>/StateSnapshot.yaml：最小状态快照（产物路径列表）
-   /runs/<task-id>/DriftReport.yaml：规格漂移报告（仅在abort时生成）

---

## 关键特性总结

-   **单一真相源（SSOT）** - 规格版本锁定与唯一归档
-   **抗漂移执行安全** - SpecBinding自动校验，规格变更时拒绝执行
-   **零记忆上下文执行** - 每个任务通过ContextCapsule自包含所需上下文
-   **覆盖追溯与验证** - CoverageMap确保规格字段完整覆盖
-   **轻量高效** - 精简报告机制，聚焦核心执行，减少60-70% token消耗
-   **FIF 多文档 YAML** - 统一文件落盘格式，支持自动化与 CI 集成
