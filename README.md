# 🏋️ 智能健身小助手

基于 HelloAgents 框架的 AI 驱动健身规划应用

## 项目介绍

智能健身小助手是一个完整的全栈应用，结合了多智能体协作、LLM 能力和现代 Web 技术。用户只需输入个人信息和健身目标，系统就能自动生成个性化的运动计划、饮食建议和进度跟踪方案。

### 核心功能

**（1）智能运动规划** - 根据体型、目标和时间生成定制化运动计划

**（2）饮食建议** - 提供营养均衡的饮食方案

**（3）进度追踪** - 记录锻炼数据和体重变化

**（4）计划编辑** - 灵活调整运动项目和强度

**（5）数据导出** - 导出为 PDF 方便保存

## 技术架构

```
┌─────────────────────────────────────────┐
│           前端层 (Vue3+TS)              │
│  Home表单 → Result页面 → 数据展示       │
└────────────────┬────────────────────────┘
                 │
        ┌────────▼────────┐
        │  FastAPI 后端   │
        │  ✓ 数据验证     │
        │  ✓ 业务逻辑     │
        └────────┬────────┘
                 │
    ┌────────────▼────────────┐
    │   多智能体系统          │
    │  ┌──────────────────┐   │
    │  │ ExerciseAgent    │   │
    │  │ NutritionAgent   │   │
    │  │ ProgressAgent    │   │
    │  │ PlannerAgent     │   │
    │  └──────────────────┘   │
    └────────────┬────────────┘
                 │
    ┌────────────▼────────────┐
    │    外部服务            │
    │  • LLM API             │
    │  • 健身数据库          │
    └────────────────────────┘
```

## 项目结构

```
hello-agents-fitness/
├── backend/                    # 后端代码
│   ├── app/
│   │   ├── agents/            # 智能体实现
│   │   │   ├── exercise_agent.py
│   │   │   ├── nutrition_agent.py
│   │   │   ├── progress_agent.py
│   │   │   └── planner_agent.py
│   │   ├── api/               # API 路由
│   │   │   ├── main.py
│   │   │   └── routes.py
│   │   ├── models/            # 数据模型
│   │   │   └── schemas.py
│   │   ├── services/          # 服务层
│   │   │   └── fitness_service.py
│   │   ├── config.py          # 配置文件
│   │   └── __init__.py
│   ├── requirements.txt       # Python 依赖
│   ├── .env.example           # 环境变量示例
│   └── run.py                 # 启动脚本
│
└── frontend/                   # 前端代码
    ├── src/
    │   ├── views/             # 页面组件
    │   │   ├── Home.vue       # 首页表单
    │   │   └── Result.vue     # 结果页面
    │   ├── services/          # API 服务
    │   │   └── api.ts
    │   ├── types/             # 类型定义
    │   │   └── index.ts
    │   ├── router/            # 路由配置
    │   │   └── index.ts
    │   ├── App.vue
    │   └── main.ts
    ├── package.json
    ├── vite.config.ts
    └── tsconfig.json
```

## 快速开始

### 环境要求

- Python 3.10+
- Node.js 16.0+
- npm 8.0+

### 后端启动

```bash
# 进入后端目录
cd backend

# 安装依赖
pip install -r requirements.txt

# 配置环境变量
cp .env.example .env
# 编辑 .env 填入 LLM API 密钥

# 启动后端服务
uvicorn app.api.main:app --reload
# 访问 http://localhost:8000/docs
```

### 前端启动

```bash
# 进入前端目录
cd frontend

# 安装依赖
npm install

# 启动前端服务
npm run dev
# 访问 http://localhost:5173
```

## 使用流程

1. **填写基本信息** - 输入年龄、体重、运动经验等
2. **选择健身目标** - 减脂、增肌、保持还是改善体质
3. **设定时间计划** - 每周运动天数和时长
4. **生成个性化方案** - AI 根据信息生成完整计划
5. **查看详细方案** - 包含运动、饮食、进度追踪
6. **调整和导出** - 编辑方案或导出为 PDF

## 核心概念

### 数据模型

- **ExerciseInfo** - 运动信息（名称、强度、时长等）
- **NutritionInfo** - 营养信息（餐型、食物、热量等）
- **ProgressRecord** - 进度记录（日期、体重、状态等）
- **WeekPlan** - 周计划（每天的运动和饮食）
- **FitnessPlan** - 完整健身计划（包含多周）

### 多智能体设计

- **ExerciseAgent** - 专门推荐运动项目
- **NutritionAgent** - 专门设计饮食方案
- **ProgressAgent** - 专门追踪和分析进度
- **PlannerAgent** - 整合所有信息生成最终计划

## 文件说明

详见各文件的具体实现代码。所有代码都包含详细注释。

## License

MIT
