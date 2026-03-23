# 快速部署指南
## TikTok AI 助手网站

---

## 📁 已创建的文件

```
/Users/netjoy/website/
├── index.html          # 首页
├── onboarding.html     # 开户资料收集页
└── DEPLOY.md          # 本文件
```

---

## 🚀 方案一：Vercel 部署（推荐）

### Step 1: 注册/登录 Vercel

访问 https://vercel.com/signup 或用 GitHub 账号直接登录

### Step 2: 安装 Vercel CLI（可选）

```bash
npm install -g vercel
```

### Step 3: 部署

**方法 A: 使用 CLI**
```bash
cd /Users/netjoy/website
vercel --prod
```

首次使用会提示登录，按提示操作即可。

**方法 B: 网页上传（更简单）**

1. 访问 https://vercel.com/new
2. 点击 "Import Git Repository" 或直接拖拽文件夹
3. 选择 `website` 文件夹
4. 点击 "Deploy"

### Step 4: 获取域名

部署完成后，Vercel 会给你一个免费域名：
```
https://your-project.vercel.app
```

也可以绑定自定义域名（可选）。

---

## 🌐 方案二：Netlify 部署（备选）

### Step 1: 访问 Netlify

https://www.netlify.com/

### Step 2: 拖拽部署

1. 登录 Netlify
2. 直接把 `/Users/netjoy/website` 文件夹拖到上传区域
3. 等待部署完成

### Step 3: 获取域名

```
https://your-site.netlify.app
```

---

## 📧 配置邮件发送（Formspree）

### Step 1: 注册 Formspree

访问 https://formspree.io/ 注册账号

### Step 2: 创建表单

1. 登录后点击 "New Form"
2. 填写表单名称（如 "TikTok Onboarding"）
3. 设置接收邮箱（客服邮箱）
4. 创建成功后获取 Form ID

### Step 3: 更新代码

打开 `onboarding.html`，找到 `handleSubmit` 函数，替换：

```javascript
// 原来的代码（约第 425 行）
function handleSubmit(event) {
    event.preventDefault();
    
    // TODO: 实际部署时替换为 Formspree 或其他邮件服务
    // 这里只是演示
    alert('这是一个演示。\n\n实际部署时，这里会将数据发送到 Formspree，自动转发邮件到客服邮箱。');
    
    // ...
}
```

**替换为：**

```javascript
function handleSubmit(event) {
    event.preventDefault();
    
    const form = event.target;
    const formData = new FormData(form);
    
    // 替换为你的 Formspree Form ID
    const FORMSPREE_URL = 'https://formspree.io/f/你的 Form ID';
    
    fetch(FORMSPREE_URL, {
        method: 'POST',
        body: formData,
        headers: {
            'Accept': 'application/json'
        }
    })
    .then(response => {
        if (response.ok) {
            // 显示成功消息
            form.style.display = 'none';
            document.querySelector('.progress').style.display = 'none';
            
            const email = formData.get('email');
            document.getElementById('successContact').textContent = '📧 ' + email;
            document.getElementById('successMessage').classList.add('show');
        } else {
            alert('提交失败，请重试');
        }
    })
    .catch(error => {
        console.error('Error:', error);
        alert('提交失败，请重试');
    });
}
```

---

## 🤖 配置 AI 对话（Dify）

### Step 1: 注册 Dify

访问 https://dify.ai 注册账号

### Step 2: 创建应用

1. 登录后点击 "创建应用"
2. 选择 "聊天助手"
3. 填写应用名称（如 "TikTok AI 助手"）

### Step 3: 配置知识库

1. 点击左侧 "知识库" → "创建知识库"
2. 上传文档：
   - TikTok 开户政策
   - 常见问题 FAQ
   - 费用说明
   - 行业限制
3. 等待处理完成

### Step 4: 配置提示词

在 "提示词" 页面，填写：

```
你是一个 TikTok 广告开户助手，专业、友好、简洁。

你的职责：
1. 解答开户政策、流程、费用等问题
2. 指导用户准备资质文件
3. 识别开户意向，引导至资料收集流程

回答风格：
- 专业但不生硬
- 简洁明了，分点说明
- 适时使用 emoji 增加亲和力

如果用户询问开户，请引导他们点击"开始开户"按钮。
```

### Step 5: 获取嵌入代码

1. 点击 "发布" → "嵌入网站"
2. 选择 "Script" 方式
3. 复制生成的代码

### Step 6: 添加到网站

打开 `index.html`，在 `</body>` 前添加：

```html
<!-- Dify AI 助手嵌入 -->
<script>
window.difyChatbotConfig = {
  token: '你的 Dify App Token',
  baseUrl: 'https://api.dify.ai/v1'
}
</script>
<script src="https://udify.app/chatbot/你的 token/script.js" defer></script>
```

---

## ✅ 上线前检查清单

- [ ] 网站已部署（Vercel/Netlify）
- [ ] 域名可访问
- [ ] 表单提交测试通过
- [ ] 邮件能正常接收
- [ ] AI 对话已配置（如有）
- [ ] 所有链接正常
- [ ] 移动端显示正常
- [ ] FAQ 内容准确
- [ ] 联系方式正确

---

## 📊 数据追踪（可选）

### Google Analytics

在 `index.html` 的 `<head>` 中添加：

```html
<!-- Google Analytics -->
<script async src="https://www.googletagmanager.com/gtag/js?id=GA_MEASUREMENT_ID"></script>
<script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());
  gtag('config', 'GA_MEASUREMENT_ID');
</script>
```

### 转化事件追踪

在 `onboarding.html` 的 `handleSubmit` 函数中添加：

```javascript
// Google Analytics 事件
gtag('event', 'form_submit', {
  'event_category': 'conversion',
  'event_label': 'TikTok Onboarding'
});
```

---

## 🔧 后续优化建议

### Phase 1（上线后 1 周）
- [ ] 收集用户反馈
- [ ] 优化 FAQ 内容
- [ ] 添加更多案例

### Phase 2（上线后 2-4 周）
- [ ] 接入真正的 AI 对话（Dify/Coze）
- [ ] 添加数据分析看板
- [ ] 优化转化漏斗

### Phase 3（上线后 1-3 月）
- [ ] 多语言支持
- [ ] CRM 系统集成
- [ ] 自动化营销流程

---

## 🆘 遇到问题？

### Vercel 部署失败
- 检查 Node.js 版本（建议 v18+）
- 尝试 `vercel --debug` 查看详细错误

### 表单提交不工作
- 检查 Formspree Form ID 是否正确
- 查看浏览器控制台错误
- 确认邮箱已验证

### AI 对话不显示
- 检查 Dify token 是否正确
- 确认应用已发布
- 查看浏览器控制台错误

---

**预计上线时间：1-2 小时**

有问题随时问 Sean 🤫
