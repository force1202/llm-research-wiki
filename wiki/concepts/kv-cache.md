---
title: "KV Cache（鍵值快取機制）"
date: 2026-11-17
category: concepts
tags: [inference, optimization, kv-cache]
---

# KV Cache（鍵值快取機制）

透過快取歷史 Token 的 Key 與 Value 矩陣，將自回歸生成階段的重複注意力計算降為線性複雜度。

- 相關論文：[[attention-is-all-you-need]]
- 相關模型：[[deepseek-r1]]
