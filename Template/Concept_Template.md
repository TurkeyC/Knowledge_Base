<%*
// Template/Concept_Template.md
tR += `---
concept: "<% tp.file.title %>"
semantic_id: "<% tp.file.folder(true).split('/').slice(1).join('-') %>-<% tp.date.now('YYYYMMDD') %>"
prerequisites: []
related: []
confidence: 0.8
difficulty: 0
source:
  - textbook: ""
    page: ""
---

## 核心定义


## 几何解释


## 典型应用


## 常见误区

%>
