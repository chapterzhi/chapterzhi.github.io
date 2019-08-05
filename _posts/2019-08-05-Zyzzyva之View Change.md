---
title: Zyzzyva之View Change
date: 2019-08-01 10:52:30
categories:
- zyzzyva
tags: 
- zyzzyva
---

## 视图变化 View Changes
视图变化要能选择一个新的主节点并保证任何好客户端中已经完成的历史不被改变。之前的一些协议中，需要一个正确的副本提交一个视图变化从而停止接受除了CHECKPOINT,VIEW-CHANGE和NEW-VIEW消息以外的消息。同时，为了防止坏节点扰乱系统，除非一个好副本提交一个视图变化，不然主节点不应改变。因此，一个好的副本只会在两种情况下提交视图变化，(1) 它观测到了主节点的错误，(2) 它有一个证明，证明有 f + 1 个副本提交了视图变化。




---
**REFERENCES**

[1] Kotla, Ramakrishna . "Zyzzyva: speculative byzantine fault tolerance." Acm Sigops Symposium on Operating Systems Principles ACM, 2007.
