# 链路图画法：简单用文字，复杂用 Mermaid

## 怎么选

| 场景 | 推荐 |
|------|------|
| 链路短、基本单向、分支少 | **纯文字** `text` 树 |
| **多模块来回、事件/MQ/回调多段进场** | **Mermaid `sequenceDiagram`** |
| 单服务内 if-else 特别深 | 可补 `flowchart`，或文字树 |

原则：复杂交互优先 Mermaid，Review 更清楚；简单改动不必强上图语法。

预览：Cursor/VS Code Markdown Preview，或贴到 [mermaid.live](https://mermaid.live)。

---

## 1. 复杂来回：Mermaid 时序图（推荐）

`->>` 同步调用，`--)` 异步（事件/MQ）。`alt/else` 画分支。`Note over` 写服务内步骤。

```mermaid
sequenceDiagram
    actor Ops as 运营前端
    participant Admin as yanxuan-admin
    participant Onb as merchant-biz·Onboarding
    participant Mer as merchant-biz·Merchant
    participant Stl as 结算/中台
    participant Dep as merchant-biz·Deposit
    participant Pay as 支付中台

    Ops->>Admin: submit-review(通过)
    Admin->>Onb: Feign submit-review
    Note over Onb: 校验 PENDING；写 APPROVED；加锁
    Onb-->>Admin: code=0
    Admin-->>Ops: 审核成功
    Onb--)Mer: OnboardingApproved（async）

    Note over Mer: 建 merchant_info / 资质
    Mer->>Stl: 开户申请
    Stl-->>Mer: 受理成功

    Stl--)Dep: 开户成功 MQ/回调
    Note over Dep: 读规则/余额；判断是否建任务
    alt 不需要或余额已够
        Dep->>Dep: 跳过
    else 需要建任务
        Dep->>Dep: 先落库进行中任务
        Dep->>Pay: 划扣
        Pay-->>Dep: payNo（可空）
        Dep->>Dep: 回写 payNo
    end

    Pay--)Dep: 划扣结果回调（可选）
    alt 成功
        Dep->>Dep: 任务 COMPLETED
    else 失败
        Dep->>Dep: 保持进行中，等对账 Job
    end
```

---

## 2. 简单链路：纯文字即可

```text
[前端] → [BFF] → [biz]
  ├─ 校验；加锁
  ├─ 分支：已存在？→ 0401
  ├─ 调 [风控] ↓ … ↑ 通过/失败
  └─ 写库 → ↑ 成功
```

同一次请求里来回两次，也可以文字：

```text
[BFF]
  └─ ① ↓ [biz] getDetail ↑
  └─ ② ↓ [biz] listLogs ↑
  └─ 拼装 VO → 前端
```

（若再叠异步三段进场，就改用上面的 Mermaid。）

---

## 3. 单服务深分支：flowchart 或文字

```mermaid
flowchart TD
    Start([createIfNeeded]) --> Lock{加锁成功?}
    Lock -->|否| Fail[请勿重复提交]
    Lock -->|是| Need{需要入驻保证金?}
    Need -->|否| Skip1[跳过]
    Need -->|是| Enough{余额已够?}
    Enough -->|是| Skip2[不建任务]
    Enough -->|否| Insert[落库任务] --> Pay[划扣] --> End([结束])
    Skip1 --> End
    Skip2 --> End
    Fail --> End
```

---

## 4. 和 easy-spec 的关系

- 模版：`templates/01-tech-design.md`  
- Agent：简单用文字；**复杂多模块来回默认出 Mermaid sequenceDiagram**  
