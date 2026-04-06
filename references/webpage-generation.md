# 平行宇宙网页生成指南 (Webpage Generation Guide)

当用户选择"🌐 平行宇宙网页"时，使用此指南生成交互式 HTML 网页。

---

## 生成原则

**单文件原则**：所有 CSS、JS 必须内联在同一个 `.html` 文件中，无外部依赖。

**视觉风格**：宇宙感 + 人情温度
- 背景：深蓝紫（#0a0e27、#1a1a3e）
- 用户轨迹颜色：金色（#f0c040）
- 平行自我轨迹颜色：青色（#4dc9f6）
- 正文：柔白（#e8e0ff），次要文字：#8888aa

---

## 文件输出

- **文件名格式**：`平行宇宙_{YYYY-MM-DD}.html`
- **保存位置**：用户的当前 workspace 根目录
- **完成后**：调用 `preview_url` 展示页面

---

## 页面结构（6个区域）

### 1. 入场动画遮罩（Entrance Overlay）
- 星空背景 + 标题淡入
- 5秒后自动消失（或点击跳过）
- 过渡：遮罩淡出，主内容淡入

### 2. 页头（Header）
- 渐变文字标题"平行宇宙"
- 分叉点信息徽章（年份 + 选择描述）

### 3. 双轨时间线（Dual-track Timeline）
- 左轨：用户的人生（金色）
- 右轨：平行自我的人生（青色）
- 每轨 4-6 个节点，每节点包含：年份标签 + 事件标题 + 可点击展开的详情
- 滚动出现动画（IntersectionObserver）

### 4. 对话精华（Dialogue Highlights）
- 聊天气泡样式
  - 用户：右侧，金色气泡
  - 平行自我：左侧，青色气泡
- 精选 3-5 段对话
- 滚动淡入

### 5. 着陆区域（Landing Section）
- 背景过渡为暖色调
- 桥接文字
- 用户的反思（如 Phase 5 中有具体内容）

### 6. 页脚（Footer）
- "本网页由「平行人生」生成" + 日期

---

## 交互技术规格

### 星空效果
```javascript
// JS 生成 150 个星星 div，随机位置，随机 animation-duration 2-6s
// CSS: @keyframes twinkle { 透明度 0.2 <-> 1 }
```

### 入场动画
- 入场遮罩：`position: fixed; z-index: 1000; background: #0a0e27`
- 5.5 秒后：`overlay.style.opacity = '0'`，然后 `overlay.remove()`
- 点击立即跳过

### 时间线节点展开
```javascript
node.addEventListener('click', () => {
  detail.classList.toggle('expanded');
});
```

### 滚动出现
```javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) entry.target.classList.add('visible');
  });
}, { threshold: 0.1 });
```

### 响应式
- 桌面：`display: grid; grid-template-columns: 1fr 1fr`
- 移动端：`@media (max-width: 768px) { grid-template-columns: 1fr }`

### 无障碍
- `@media (prefers-reduced-motion: reduce)` — 停用所有动画

---

## 数据填充要求

使用 `assets/webpage-template.html` 作为起点，将以下内容替换为实际数据：

| 占位符 | 替换为 |
|--------|--------|
| `[分叉点年份]` | 实际年份 |
| `[分叉点描述]` | "你选择了X，平行自我选择了Y" |
| `[你的轨迹节点]` | 4-6个真实节点（年份+标题+详情） |
| `[平行自我轨迹节点]` | 4-6个真实节点 |
| `[对话精华]` | 3-5段真实对话 |
| `[桥接语句]` | Phase 5 的桥接文字 |
| `[用户反思]` | 用户在 Phase 5 的回应（如有） |

---

## 质量检查

生成后确认：
- [ ] HTML 文件完全自包含（无外部链接/CDN）
- [ ] 在 preview_url 中可正常显示
- [ ] 入场动画可正常播放
- [ ] 时间线节点可点击展开
- [ ] 对话气泡方向正确（用户右，平行自我左）
- [ ] 移动端布局正常（单列）
- [ ] 所有占位符已替换为真实内容
