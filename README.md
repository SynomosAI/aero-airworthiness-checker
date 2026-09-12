# aero-airworthiness-checker

> **状态：RESERVED（占位 · 开放认领）** — 本仓已按平台协议规范建好行业接入四件套骨架，
> 等待具备本域资质的运营方认领并填充真实规则。

把「这架航空器适不适航、维修单位有没有资质」拆成可核验的属性，让 AI 只做提示，不下放行结论。

## 这个域管什么

航空器适航状态、维修单位资质与放行签署核验

适航是航空安全的底线，证件、维修单位批准、放行签署、人员执照环环相扣。AI 能做的是把各环节状态摆清楚，适航与放行权只属于审定部门和授权人员。

## 域标识

| 项 | 值 |
|---|---|
| 域 ID | `aero`（全局唯一，一经分配不复用） |
| 域名称 | 航空航天 · 适航与维修资质 |
| Profile 版本 | `domain/1.0` |
| 当前状态 | `RESERVED` |
| 占位时间 | 2026-09-12 |

## 属性清单

| 属性键 | 类型 | 说明 |
|---|---|---|
| `aero.airworthiness_status` | enum | 适航证件当前状态，以适航审定部门结论为准 · 取值 valid/suspended/revoked/expired/pending/not_required |
| `aero.maintenance_org_approval` | enum | 维修单位批准范围与当前有效性 · 取值 approved/limited/unapproved/not_applicable |
| `aero.released_to_service` | boolean | 维修后是否已完成放行签署，须由持照人员签署 |
| `aero.personnel_license` | enum | 维修或飞行人员执照状态 · 取值 valid/expired/suspended/not_applicable |
| `aero.flight_authorization` | enum | 空域与飞行任务批准状态 · 取值 authorized/pending/denied/not_applicable |

## 本域红线（不可逾越，机器可读）

1. 适航状态以适航证件与审定部门结论为准，AI 不得自行认定
2. 不得输出适航合格、可放行或可执行飞行任务的放行类表述，放行须由授权人员签署
3. 不得生成或代拟维修放行签署、适航证件或飞行任务批准文件

> 红线在 `gate-map.json` 中均有对应阻断规则。平台校验器会检查「每条红线都有规则覆盖」，
> 缺失即校验失败——**制度与系统不允许不同步**。

## 行业接入四件套

| 文件 | 作用 |
|---|---|
| `domain.manifest.json` | 本域声明：属性清单、签发方要求、有效期、红线 |
| `gate-map.json` | 本域「什么动作要多少摩擦」：silent / warn / confirm / block / require-owner |
| `privacy.json` | 本域隐私声明：默认关闭、最小必要、可撤回、可删除 |
| `checker` | 本域核验器（MCP 工具，**只出示核验，不下判定**） |

## 核心原则

**平台只当擂台，不当货架。** 本域的核验器只回答「这条声明是否可核验、缺什么要件」，
不回答「这件事是否合规、该不该做」。判定权在本域的资质方、监管方与人。

**隐私是准入条件，不是整改事项。** 缺失 `privacy.json` 或任一必填字段不符，
符合性校验直接失败——不是警告，是拒绝接入。

## 参考依据

- 中华人民共和国民用航空法
- CCAR-21 民用航空产品和零部件合格审定规定
- CCAR-145 民用航空器维修单位合格审定规定

> 上列依据仅用于说明本域属性的来源与口径，不构成法律意见。具体适用以现行有效文本与主管部门解释为准。

## 认领方式

本域面向具备相应资质的机构开放。认领后请：

1. Fork 本仓，填注 `operator` 与 `checker_endpoint`
2. 按本域现行有效规则校准属性取值与红线表述
3. 跑平台侧校验器自测（五项判据全过方可提交）
4. 提 PR，附资质证明与规则依据

## 许可与署名

代码与配置按 MIT 许可使用。文档的知识版权归 SynomosAI 所有。

© 2026 SynomosAI. All rights reserved.
