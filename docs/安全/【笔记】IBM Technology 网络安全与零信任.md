---
tags: [Notebooks/Study]
title: 【笔记】IBM Technology 网络安全与零信任
created: '2022-04-25T08:11:52.594Z'
modified: '2022-04-25T12:55:23.563Z'
---

# 【笔记】IBM Technology 网络安全与零信任

今日看了一个转载IBM的视频，https://www.bilibili.com/video/BV1R34y1v7tX，讲网络安全趋势的，感觉很有帮助，所以做了一下笔记

---

网络安全中的三个趋势：实施零知识模型、改进如何进行威胁管理、如何支持组织现代化

## Zero Trust
如果确定敏感数据处于保护，or存储在什么位置？
right user，right access，right data，right reason
各种控制措施本质上都是在管理上面这些concept
措施介绍：
right user：
- identity governance身份治理
  - who has access to what
- identity analytics 身份分析
  - know “who has access to what”是否有意义，这群人应该可以访问这些东西吗？
- privileged account management特权账户管理（insider threats内部威胁）
  - 19years after sarbanes-oxley，short thrift

right access：
- access management 访问管理
  - 此人是否能访问此应用程序
- adaptive authentication自适应身份认证（whitest hot control）
  - 在混合云中，每次有人想访问敏感信息时，都应该围绕它制定风险评分，eg：人/设备/日期我是否能识别？自适应身份验证允许设置我使用什么级别的多因素身份认证，来实际允许某人进入
  - 从银行业获取欺诈检测算法（the fraud detection algorithms），并将他们结合到身份和访问管理堆栈（identity and management stack），以允许某人执行这种高级功能（说的应该是自适应）

right data：
- D+C，discovery and classification，发现与分类
  - 确保我们知道的敏感数据都在本地或云供应商中的位置；一旦知道位置，我需要将其锁定，随后就进入了️
- encryption，加密（popular）
- data and file activity monitoring数据和文件活动监控
  - 使用加密，当可以访问的时候，也可以限制访问
  - 可以访问，但不能访问某一特定数据集合
- key management密钥管理
  - 确保可以保护盖数据的加密密钥并进行密钥管理

right reason（wants but struggle）
- data risk insights
  - if we go to a cloud-based architecture, 您可以查看长时间使用的大量数据，并找到您第一次错过的东西
- handle transactional fraud处理交易欺诈
- configuration and management配置和管理
  - 在零信任环境中，这意味着在混合云环境中，您正在应用从不信任、始终验证的零信任模型，所以需要担心三种配置管理
    - 设备 devices
    - 网络 network
    - 云原生堆栈 configuration management of the cloud native stack
      - 在运DevOps项目时会将工作负载和敏感数据放到可能的多个云中

## 威胁管理
信任算法，big influencers，如何实施控制 —— 如何实际进行威胁管理（threat management）
**Threat Management**
find-》confirm-》fix
翻译：
大海捞针/在针堆中找到针-》确认他们是否足够敏锐以采取行动-》修复泥发现的东西

find：
- SIEM工具/安全信息事件管理：手机、规范化、关联、报告、监控和记录
- 实时网络流量分析
- 用户行为分析

confirm：
- search（no sense）
- AI 人工智能

fix：
- null set 空集
  - 遇到了危险却没有指定相应的应对计划
  - 跨组织协作计划以应对网络事件
- grow awareness 通过网络靶场活动（cyber range activities）等方式提高以实，进行实弹演习
- 制定自动化事件响应手册（the incident playbooks）


## modernization 现代化数字化转型
即，how do you support the modernization of your organization
当有新的威胁和东西来源时，原先十几年各大组织的办法是：“收集所有的数据，然后放入使用的任何技术中（安全分析平台）”，但是该方法不能扩展到混合云！
为什么？
因为使用混合云时，很多网络数据实在一/多个公共云提供商（public cloud providers）中生成的，如果使用上方法，需要向云提供商支付出口费用（egress fee），然后才能将数据取出并放入自己的工具中，甚至可能根据所使用的工具支付费用
所以这证明了，上述方法无法扩展，that approach does not scale

寻找替代方法，然后意识到了这里缺少了一个范围（a missing scope）
- 与其强迫所有东西都进入同一个地方，即使是来自公共云提供商，不如采取联合方法（faderated approach）来进行威胁调查和应用威胁情报
  - 出现新指标，不要等数据传输到自己的安全分析平台，而是询问安全分析平台，安全工具，云供应商等，然后当场做出决定。这样更简单、快速且有效。
  - 本步骤与【fix】联合起来，对威胁管理能力产生重大影响
  - 现代化您的方法，方法能提供网络服务给你的现代化组织，因为这一切都建立在云原生微服务上


