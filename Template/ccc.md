<%*
// 文件名：定理模板.md
tR += `---
type: "定理证明"
semantic_id: "THM-${tp.file.folder(true).replace(/\//g,'-')}-<% tp.date.now('HHmmss') %>"
prerequisites: 
  - "[[前置定理]]"
related_proofs: 
  - "[[替代证明方法::THM-...]]"
proof_method: "直接证明法"
---

## 定理陈述
<% await tp.cursor(1) %>

## 证明过程
### 步骤1：建立基础

### 步骤2：核心推导

## 应用示例
\`\`\`python
# 验证代码
<% tp.file.cursor(2) %>
\`\`\`
`;
%>