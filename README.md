# Online Note

多端公用的在线便签，支持桌面/平板/手机，团队协作、富文本编辑、云同步。

作者 **CaBa** · GitHub [@CaB520](https://github.com/CaB520)

## 功能

| 模块 | 说明 |
|---|---|
| 便签 CRUD | 创建、编辑、删除、置顶、归档 |
| 富文本编辑 | Tiptap 所见即所得，支持 Markdown 粘贴 |
| 待办清单 | 任务勾选、自动排序 |
| 标签分类 | 自定义标签、标签筛选 |
| 团队协作 | 团队便签、邀请密钥加入、权限管理 |
| 用户系统 | 邮箱注册/登录、滑动验证码、邮箱验证码 |
| 主题切换 | 浅色 / 暗黑 / 跟随系统 |
| 响应式 | 桌面三栏、平板双栏、手机单栏 + 底部导航 |
| 邮件通知 | SMTP 发信，支持 QQ/163/Gmail |
| 云同步 | 跨设备数据同步、离线缓存 |

## 技术栈

- **框架**: Next.js 16 (App Router)
- **语言**: TypeScript
- **数据库**: PostgreSQL + Prisma ORM
- **缓存**: Redis
- **认证**: NextAuth v5 (Credentials + JWT)
- **编辑器**: Tiptap (ProseMirror)
- **UI**: Tailwind CSS + shadcn/ui
- **邮件**: Nodemailer (SMTP)
- **图标**: Lucide Icons

## 快速开始

### 1. 环境要求

- Node.js 20+
- Docker (PostgreSQL + Redis)
- SMTP 邮箱（QQ/163/Gmail 均可，可选）

### 2. 启动数据库

```bash
docker compose up -d
```

### 3. 配置环境变量

```bash
cp .env.example .env
```

编辑 `.env`，填入必要配置：

```env
DATABASE_URL="postgresql://onlinenote:onlinenote_dev@localhost:5432/onlinenote"
AUTH_SECRET="生成一个随机密钥"
REDIS_URL="redis://localhost:6379"

# 邮件（可选，不配则使用 Ethereal 测试邮箱）
SMTP_HOST=smtp.qq.com
SMTP_PORT=465
SMTP_USER=你的邮箱@qq.com
SMTP_PASS=授权码
SMTP_FROM="Online Note <你的邮箱@qq.com>"
```

### 4. 初始化数据库

```bash
npm install
npx prisma generate
npx prisma db push
```

### 5. 启动开发服务器

```bash
npm run dev
```

打开 http://localhost:3000

### 6. 生产部署

```bash
npm run build
npm start
```

内网穿透时不要设 `AUTH_URL`，NextAuth 会自适应域名。

## 项目结构

```
src/
├── app/
│   ├── (auth)/              # 登录/注册页
│   ├── (main)/              # 主应用（需登录）
│   │   ├── page.tsx          # 主页 - 便签列表
│   │   ├── notes/[id]/      # 便签编辑器
│   │   ├── teams/           # 团队管理
│   │   └── settings/        # 设置页
│   └── api/                 # REST API
│       ├── auth/            # 认证
│       ├── notes/           # 便签 CRUD
│       ├── tags/            # 标签
│       ├── teams/           # 团队
│       ├── invitations/     # 邀请
│       ├── verify/          # 邮件验证码
│       └── captcha/         # 滑动验证码
├── components/
│   ├── ui/                  # shadcn/ui 组件
│   ├── auth/                # 登录/注册/验证码
│   ├── layout/              # 导航栏/侧栏/底部导航
│   ├── notes/               # 便签卡片/列表
│   └── editor/              # Tiptap 编辑器
├── hooks/                   # 自定义 Hooks
├── lib/                     # 工具库
└── types/                   # 类型定义
```

## API 端点

| 方法 | 路径 | 说明 |
|---|---|---|
| GET/POST | `/api/notes` | 便签列表 / 创建 |
| GET/PATCH/DELETE | `/api/notes/[id]` | 便签详情 / 更新 / 删除 |
| GET/POST/DELETE | `/api/tags` | 标签列表 / 创建 / 删除 |
| GET/POST | `/api/teams` | 团队列表 / 创建 |
| GET/PATCH/DELETE | `/api/teams/[id]` | 团队详情 / 更新 / 删除 |
| POST/DELETE | `/api/teams/[id]/members` | 邀请 / 移除成员 |
| PATCH | `/api/teams/[id]/invite-code` | 生成 / 刷新邀请密钥 |
| POST | `/api/teams/join` | 通过密钥加入团队 |
| GET | `/api/invitations` | 待处理邀请 |
| PATCH | `/api/invitations/[id]` | 接受 / 拒绝邀请 |
| POST | `/api/verify/send` | 发送邮箱验证码 |
| POST | `/api/verify/check` | 验证邮箱验证码 |
| GET | `/api/captcha/generate` | 生成滑动验证码 |
| POST | `/api/captcha/verify` | 验证滑动验证码 |
| PATCH | `/api/user/settings` | 更新用户设置 |

## License

MIT © 2026 CaBa
