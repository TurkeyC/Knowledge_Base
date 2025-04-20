以下是 **Templater模板的完整使用教程**，结合你正在构建的微积分知识库，我将分步骤详细说明：

---

### 一、基础配置流程

#### 1. 安装与启用
1. 在Obsidian设置中打开**第三方插件**
2. 浏览社区插件市场搜索 **Templater**
3. 安装后启用并重启Obsidian

![Templater安装示意图](https://i.imgur.com/5xKjvLp.png)

#### 2. 设置模板文件夹
```yaml
# 设置 → Templater → 模板文件夹位置
模板存储路径: 0_元数据/模板
```

---

### 二、核心模板设计示例

#### 1. 基础概念模板
````markdown
<%*
// 文件名：概念模板.md
// 路径：0_元数据/模板/概念模板.md

/*---- 自动生成语义ID ----*/
const pathSegments = tp.file.folder(true).split('/');
const unitCode = pathSegments[1].split('_')[0].replace('Unit','U');
const chapterCode = pathSegments[2].split('_')[0].replace('Chapter','C');
const semanticId = `CAL-${unitCode}-${chapterCode}-${tp.date.now('YYYYMMDDHHmmss')}`;

/*---- 构建YAML Front Matter ----*/
tR += `---
concept: "${tp.file.title}"
semantic_id: "${semanticId}"
prerequisites: 
  - "[[待补充关联概念]]"
related: []
domain: "${pathSegments[1].split('_')[1]}" 
confidence: 0.8
difficulty: 0
source:
  - textbook: "同济高等数学第七版"
    page: ""
---

## 核心定义
<% await tp.cursor() %>

## 几何解释


## 典型应用


## 常见误区
\`\`\`dataview
TABLE related FROM #错误案例 WHERE contains(related_concept, this.semantic_id)
\`\`\`
`;
%>
````

#### 2. 定理证明模板
```markdown
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
```

---

### 三、实战使用演示

#### 场景：创建"洛必达法则"新条目

1. **调用模板**
   - 按<kbd>Ctrl/Cmd</kbd>+<kbd>P</kbd>打开命令面板
   - 输入并选择**Templater: Insert Template**
   - 选择"定理模板.md"

2. **自动生成内容**
```markdown
---
type: "定理证明"
semantic_id: "THM-Unit03-Chapter05-LHospital-142536"
prerequisites: 
  - "[[柯西中值定理::THM-U03-C04-CAUCHY]]"
related_proofs: 
  - "[[泰勒展开证明法::THM-U03-C05-TAYLOR]]"
proof_method: "直接证明法"
---

## 定理陈述
当x→a时，若f(x)/g(x)为0/0或∞/∞型未定式，则：
$$\lim_{x \to a}\frac{f(x)}{g(x)} = \lim_{x \to a}\frac{f'(x)}{g'(x)}$$

## 证明过程
### 步骤1：建立基础
构造辅助函数F(x)=f(x)-f(a)...

### 步骤2：核心推导
应用[[柯西中值定理]]可得...

## 应用示例
```python
# 验证代码
from sympy import limit, Symbol
x = Symbol('x')
f = x**2 - 1
g = x - 1
print(limit(f/g, x, 1))  # 应输出2
```
```

---

### 四、高级功能详解

#### 1. 动态插入光标
```markdown
<%*
// 在模板中预设多个光标位置
tR += `## 核心定义
<% tp.cursor(1) %>

## 几何解释
<% tp.cursor(2) %>
`;
%>
```
使用时按<kbd>Tab</kbd>键在不同光标位跳转

#### 2. 自动填充元数据
```javascript
// 自动生成难度系数
const difficultyMap = {
  "Unit01": 2.5,
  "Unit02": 3.8,
  "Unit03": 4.2
};
const unitKey = pathSegments[1].split('_')[0];
tR += `difficulty: ${difficultyMap[unitKey]}\n`;
```

#### 3. 智能关联推荐
```javascript
// 自动推荐相关概念
const relatedConcepts = await tp.user.getSimilarConcepts(tp.file.title);
tR += `related:\n${relatedConcepts.map(c => `  - "[[${c}]]"`).join('\n')}`;
```

---

### 五、模板定制技巧

#### 1. 学科编码定制
```javascript
// 修改semantic_id生成规则
const subjectCode = {
  '微积分': 'CAL',
  '线性代数': 'LAG',
  '概率论': 'PRO'
};
const currentSubject = tp.file.folder(true).split('/')[0];
tR += `semantic_id: "${subjectCode[currentSubject]}-${unitCode}..."`;
```

#### 2. 多类型模板切换
```markdown
<%*
// 动态选择模板类型
const noteType = await tp.system.suggester(
  ["概念", "定理", "例题"],
  ["概念模板.md", "定理模板.md", "例题模板.md"]
);
await tp.file.include(noteType);
%>
```

---

### 六、常见问题解决

| 问题现象                 | 解决方案                                  |
|--------------------------|-----------------------------------------|
| 模板不显示               | 检查模板文件夹路径是否正确                 |
| 变量未替换               | 确认使用<% %>包裹JS代码                   |
| 中文乱码                 | 在模板首行添加`---\nencoding: utf-8\n---` |
| 无法识别tp.file.folder   | 更新Templater到最新版本                   |
| 自动生成的ID过长         | 修改日期格式为`tp.date.now('MMDDHHmm')`   |

---

### 七、效率提升技巧

1. **快捷键绑定**
   ```yaml
   # 设置 → 快捷键 → 添加：
   - 命令: Templater: 插入模板
     快捷键: Ctrl+Alt+T
   ```

2. **模板片段复用**
   ```markdown
   <%*
   // 公共公式片段库
   const formulaLib = {
     derivative: "$$f'(x) = \\lim_{h \\to 0} \\frac{f(x+h)-f(x)}{h}$$",
     integral: "$$\\int_a^b f(x)dx = F(b) - F(a)$$"
   };
   tR += formulaLib.derivative;
   %>
   ```

3. **批量创建工具**
   ```javascript
   // 批量生成章节模板
   for(let i=1; i<=5; i++) {
     await tp.file.create_new(
       `Chapter0${i}_章节名.md`, 
       await tp.file.include("概念模板.md"),
       tp.file.folder
     );
   }
   ```

---

通过这种深度定制的模板系统，你可以实现：
1. **一键生成标准结构**：3秒创建符合规范的知识条目
2. **智能元数据填充**：自动关联前置知识
3. **动态内容组装**：根据学科自动调整编码规则
4. **错误预防机制**：内置数据校验逻辑

建议将常用模板与[[QuickAdd]]插件结合，实现更复杂的工作流自动化。例如设置快捷键自动创建错题本条目并关联到相应知识点。