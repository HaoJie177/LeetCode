subgraph "1. 待处理 (缺翻译)"
    direction TB
    A -- "系统/管理员确认" --> B(PENDING_SEND / 未提翻)
    B -- "查询OTS/SR: 无记录" --> B1[等待手动提翻]
    B -- "查询OTS/SR: UX确认中" --> B2["高亮: UX确认中<br>(停在'缺翻译'面板)" ]
    B1 -- "管理员手动发起" --> C
    B2 -- "UX确认完毕, 自动/手动发起" --> C
end

subgraph "2. 翻译"
    direction TB
    B -- "查询OTS/SR: 已在翻译" --> C(IN_TRANSLATION / 翻译中)
end

subgraph "3. 合入"
    direction TB
    C -- "翻译已回稿" --> D(PENDING_MERGE / 待合入)
    D -- "自动/手动关联MR" --> E(IN_MERGE / 合入中)
    E -- "MR已合并<br>代码校验一致" --> F[COMPLETED / 已完成]
end

subgraph " "
    direction TB
    F --> End1[结束: 已闭环]
end

subgraph "分支: 忽略流程 (Flow: Ignore)"
    direction TB
    A -- "开发提请忽略<br>(如:无需翻译)" --> G(PRE_IGNORED / 待确认忽略)
    B -- "开发提请忽略" --> G
    H[代码中删除] -- "系统扫描" --> G
    G -- "管理员审核通过" --> I[IGNORED / 已忽略]
    I --> End2[结束: 已闭环]
end

subgraph "分支: 变更流程 (Flow: Stale)"
    direction TB
    C -- "源文案变更" --> S(STALE / 待重翻)
    D -- "源文案变更" --> S
    E -- "源文案变更" --> S
    F -- "源文案变更" --> S
    S -- "自动/手动发起重翻" --> C
end

%% 样式
classDef default fill:#F8F9FA,stroke:#333,stroke-width:2px;
classDef state fill:#EBF5FF,stroke:#007BFF,stroke-width:1px;
classDef end fill:#E6FFEE,stroke:#28A745,stroke-width:1px;
classDef ignore fill:#FFF5E6,stroke:#FFA500,stroke-width:1px;
classDef stale fill:#FDEDEC,stroke:#DC3545,stroke-width:1px;

class A,B,B1,B2 state;
class C state;
class D,E state;
class F,End1,End2 end;
class G,I ignore;
class S stale;
