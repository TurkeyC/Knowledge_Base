<%*
// templates/概念模板.md
tR += `---
concept: "<% tp.file.title %>"
semantic_id: "CAL-${tp.file.folder(true).replace(/\//g,'-')}-<% tp.date.now('YYYYMMDDHHmmss') %>"
prerequisites: 
  - "[[相关概念]]"
related:
  - "[[兄弟概念]]"
confidence: 0.9
---\n\n## 核心定义\n`
%>