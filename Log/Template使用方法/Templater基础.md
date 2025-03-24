Obsidian 的 **Templater** 插件是一个强大的模板工具，支持动态内容、变量替换甚至 JavaScript 脚本，比 Obsidian 自带的模板功能更灵活。以下是详细的使用指南：

---

### **1. 安装 Templater**
- 打开 Obsidian → 点击左侧边栏的「设置」⚙️。
- 进入「社区插件」→ 关闭安全模式 → 点击「浏览」搜索 "Templater" → 安装并启用。
- **（可选）** 在插件列表中启用 Templater。

---

### **2. 基础配置**
1. **设置模板文件夹**：
   - 进入 Templater 插件设置 → 在「Template folder location」中指定一个文件夹（如 `Templates`）。
   - 在此文件夹内创建的 `.md` 文件会被识别为模板。

2. **启用 JavaScript 执行**（可选）：
   - 在 Templater 设置中打开「Enable System Commands」，允许模板中运行 JavaScript 或系统命令（需谨慎）。

---

### **3. 创建模板**
1. 在模板文件夹（如 `Templates`）中新建一个 `.md` 文件（例如 `Daily Note.md`）。
2. **编写模板内容**：
   - **静态内容**：普通文本（如标题、固定结构）。
   - **动态变量**：用 `<% tp.date.now() %>` 等语法插入动态值。
   - **JavaScript**：用 `<% ... %>` 包裹代码，实现复杂逻辑。

**示例模板：**
```markdown
# <% tp.file.title %>
创建日期: <% tp.date.now("YYYY-MM-DD HH:mm") %>

## 今日目标
- [ ] 任务1
- [ ] 任务2

<%*
// 使用 JavaScript 动态生成内容
const tags = ["#work", "#meeting"];
tR += `标签: ${tags.join(" ")}`;
%>
```

---

### **4. 常用 Templater 语法**
- **变量替换**：
  - `{{title}}`：当前文件名（需 Obsidian 变量支持）。
  - `{{date}}`：当前日期（需配合 Templater 函数）。

- **日期函数**：
  ```markdown
  <% tp.date.now("YYYY-MM-DD", 0, tp.file.title, "YYYY-MM-DD") %>  // 格式化日期
  <% tp.date.tomorrow("YYYY-MM-DD") %>                            // 明天
  ```

- **文件操作**：
  - `tp.file.create_new()`：创建新文件。
  - `tp.file.rename("新文件名")`：重命名当前文件。

- **用户输入**：
  ```javascript
  <%* const project = await tp.system.prompt("项目名称"); %>
  项目：<%= project %>
  ```

---

### **5. 使用模板**
- **方式 1**：命令面板（`Ctrl/Cmd + P`）→ 输入「Templater: Insert template」→ 选择模板。
- **方式 2**：为常用模板设置快捷键（在 Obsidian 快捷键设置中绑定）。
- **方式 3**：结合「Daily Notes」等插件自动插入模板。

---

### **6. 高级功能**
1. **JavaScript 脚本**：
   - 在 `<%* ... %>` 内写 JavaScript，可操作文件、弹窗交互等。
   ```javascript
   <%*
   const books = ["Book A", "Book B"];
   const randomBook = books[Math.floor(Math.random() * books.length)];
   tR += `**今日推荐书籍**: ${randomBook}`;
   %>
   ```

2. **系统命令**（需在设置中启用）：
   ```javascript
   <%* await tp.system.shell("echo 'Hello' >> log.txt") %>  // 执行系统命令
   ```

3. **捕获用户输入**：
   ```javascript
   <%* 
   const name = await tp.system.prompt("请输入姓名");
   tR += `欢迎，${name}！`;
   %>
   ```

---

### **7. 应用场景示例**
- **日记模板**：自动插入日期、天气、每日问题。
- **读书笔记**：生成统一结构，链接到书籍元数据。
- **项目规划**：动态插入任务列表和截止日期。

---

### **8. 注意事项**
- 执行系统命令可能存在安全风险，建议仅在信任的库中启用。
- 变量和函数需注意大小写（如 `tp.date.now()` 不是 `tp.Date.Now()`）。
- 若模板不生效，检查模板文件夹路径是否正确，或重启 Obsidian。

---

通过 Templater，你可以将重复的笔记结构自动化，节省时间并保持一致性。遇到问题时，可查阅 [Templater 官方文档](https://silentvoid13.github.io/Templater/) 或社区教程。