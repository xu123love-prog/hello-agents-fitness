# 智能健身小助手 - 完整框架指南

这是一份详细的实现指南，帮助你按照旅行助手的架构模式构建智能健身小助手。

## 📋 文件结构

```
hello-agents-fitness/
├── backend/
│   ├── app/
│   │   ├── agents/
│   │   │   ├── __init__.py
│   │   │   ├── exercise_agent.py          ✅ 运动推荐专家
│   │   │   ├── nutrition_agent.py         ✅ 营养建议专家
│   │   │   ├── progress_agent.py          ✅ 进度追踪专家
│   │   │   └── planner_agent.py           ✅ 整体规划专家
│   │   ├── api/
│   │   │   ├── __init__.py
│   │   │   ├── main.py                    ✅ FastAPI 应用入口
│   │   │   └── routes.py                  ✅ API 路由定义
│   │   ├── models/
│   │   │   ├── __init__.py
│   │   │   └── schemas.py                 ✅ Pydantic 数据模型
│   │   ├── config.py                      ✅ 配置管理
│   │   └── __init__.py
│   ├── requirements.txt                   ✅ Python 依赖
│   ├── .env.example                       ✅ 环境变量示例
│   └── run.py                             ✅ 启动脚本
│
└── frontend/
    ├── src/
    │   ├── views/
    │   │   ├── Home.vue                   ✅ 首页表单
    │   │   └── Result.vue                 ✅ 结果展示页
    │   ├── services/
    │   │   └── api.ts                     ✅ API 调用封装
    │   ├── types/
    │   │   └── index.ts                   ✅ TypeScript 类型定义
    │   ├── router/
    │   │   └── index.ts                   ✅ 路由配置
    │   ├── App.vue
    │   └── main.ts
    ├── package.json
    ├── vite.config.ts
    └── tsconfig.json
```

## 🔧 实现步骤

### 第1步：后端数据模型（✅ 已提供）

**文件：`backend/app/models/schemas.py`**

包含以下Pydantic模型：
- `UserProfile` - 用户基本信息
- `Exercise` - 单个运动项目
- `DayExercisePlan` - 单日运动计划
- `Meal` - 单顿饭
- `DayNutritionPlan` - 单日营养计划
- `ProgressRecord` - 进度记录
- `FitnessPlan` - 完整健身计划
- `FitnessPlanRequest` - 请求模型
- `FitnessPlanResponse` - 响应模型

### 第2步：智能体实现

#### 2.1 ExerciseAgent（运动推荐专家）

**文件：`backend/app/agents/exercise_agent.py`**

```python
from hello_agents import SimpleAgent
from app.config import get_settings

EXERCISE_AGENT_PROMPT = """你是运动推荐专家。

根据用户的以下信息生成运动计划：
- 用户体型和当前体重
- 运动等级和目标（减脂/增肌/改善体质）
- 每周可用运动天数和单次时长
- 伤病史

请为每一天生成具体的运动项目，包括：
1. 运动名称和类别（有氧/无氧/柔韧性）
2. 运动时长（分钟）
3. 强度等级（低/中/高）
4. 预估消耗热量
5. 详细描述和姿态注意事项
6. 所需器材

输出格式为JSON数组。
"""

class ExerciseAgent(SimpleAgent):
    def __init__(self, llm=None):
        # TODO: 初始化 SimpleAgent
        # - 使用 HelloAgentsLLM
        # - 设置 system_prompt 为 EXERCISE_AGENT_PROMPT
        pass
    
    def recommend_exercises(self, user_info: dict, duration_weeks: int) -> str:
        """推荐运动计划"""
        # TODO: 构建查询并调用 run() 方法
        # 返回 LLM 的运动推荐结果
        pass
```

#### 2.2 NutritionAgent（营养建议专家）

**文件：`backend/app/agents/nutrition_agent.py`**

```python
from hello_agents import SimpleAgent

NUTRITION_AGENT_PROMPT = """你是营养建议专家。

根据以下信息提供营养计划：
- 用户年龄、体重、身高
- 运动强度和频率
- 健身目标（减脂/增肌）
- 饮食限制（素食/过敏等）

请为每一天生成营养计划，包括：
1. 每餐的食物建议
2. 每餐的热量和营养成分（蛋白质/碳水/脂肪）
3. 每日总热量摄入
4. 建议饮水量
5. 补充剂建议

输出格式为JSON。
"""

class NutritionAgent(SimpleAgent):
    def __init__(self, llm=None):
        # TODO: 初始化 SimpleAgent
        pass
    
    def recommend_nutrition(self, user_info: dict, exercise_plan: str) -> str:
        """推荐营养计划"""
        # TODO: 基于运动计划生成营养建议
        pass
```

#### 2.3 ProgressAgent（进度追踪专家）

**文件：`backend/app/agents/progress_agent.py`**

```python
from hello_agents import SimpleAgent

PROGRESS_AGENT_PROMPT = """你是进度追踪专家。

根据用户的初始状态和目标，分析和预���进度。

请生成：
1. 每周的预期进度（体重变化、性能提升等）
2. 检查点和里程碑
3. 进度评估标准
4. 动力激励建议

输出格式为JSON。
"""

class ProgressAgent(SimpleAgent):
    def __init__(self, llm=None):
        # TODO: 初始化 SimpleAgent
        pass
    
    def generate_progress_tracking(self, user_info: dict, plan: str) -> str:
        """生成进度追踪方案"""
        # TODO: 生成进度追踪建议
        pass
```

#### 2.4 PlannerAgent（整体规划专家）

**文件：`backend/app/agents/planner_agent.py`**

```python
from hello_agents import SimpleAgent
from app.models.schemas import FitnessPlan, FitnessPlanRequest

PLANNER_AGENT_PROMPT = """你是健身规划专家。

整合以下信息生成完整的健身计划：
1. 运动推荐
2. 营养建议
3. 进度追踪方案

请生成一个综合的、可行的健身计划。
输出格式必须是严格的JSON格式。
"""

class FitnessPlannerAgent:
    """健身规划系统 - 协调多个Agent"""
    
    def __init__(self):
        # TODO: 初始化所有 Agent
        self.exercise_agent = None
        self.nutrition_agent = None
        self.progress_agent = None
        self.planner_agent = None
    
    def generate_fitness_plan(self, request: FitnessPlanRequest) -> FitnessPlan:
        """
        生成完整的健身计划
        
        步骤：
        1. 调用 ExerciseAgent 生成运动计划
        2. 调用 NutritionAgent 生成营养计划
        3. 调用 ProgressAgent 生成进度追踪方案
        4. 调用 PlannerAgent 整合所有信息
        5. 解析 JSON 并创建 FitnessPlan 对象
        """
        # TODO: 实现多步骤协作流程
        pass
    
    def _build_planner_query(self, request, exercise_result, nutrition_result, progress_result) -> str:
        """构建最终规划Agent的查询"""
        # TODO: 整合所有信息到一个清晰的查询字符串
        pass
    
    def _parse_fitness_plan(self, json_response: str) -> FitnessPlan:
        """将JSON响应转换为FitnessPlan对象"""
        # TODO: 解析 JSON 并进行数据验证
        pass
```

### 第3步：API 路由

**文件：`backend/app/api/main.py`**

```python
from fastapi import FastAPI
from fastapi.middleware.cors import CORSMiddleware
from app.api import routes

app = FastAPI(
    title="🏋️ 智能健身小助手",
    description="基于AI的个性化健身规划系统",
    version="1.0.0"
)

# 配置 CORS
app.add_middleware(
    CORSMiddleware,
    allow_origins=["*"],
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)

# 包含路由
app.include_router(routes.router)

if __name__ == "__main__":
    # TODO: 使用 uvicorn 运行应用
    pass
```

**文件：`backend/app/api/routes.py`**

```python
from fastapi import APIRouter, HTTPException
from app.models.schemas import FitnessPlanRequest, FitnessPlanResponse, FitnessPlan
from app.agents.planner_agent import FitnessPlannerAgent

router = APIRouter(prefix="/api", tags=["fitness"])

# 初始化规划系统
planner = FitnessPlannerAgent()

@router.post("/fitness/plan", response_model=FitnessPlanResponse)
async def create_fitness_plan(request: FitnessPlanRequest) -> FitnessPlanResponse:
    """
    生成个性化健身计划
    
    请求参数包含：
    - 用户基本信息（年龄、体重、身高）
    - 运动目标（减脂/增肌/改善体质）
    - 可用时间和强度
    
    返回完整的健身计划：
    - 每周的运动安排
    - 每日的营养建议
    - 进度追踪指标
    """
    try:
        # TODO: 调用规划系统
        plan = planner.generate_fitness_plan(request)
        return FitnessPlanResponse(
            success=True,
            plan=plan,
            message="健身计划生成成功！"
        )
    except Exception as e:
        return FitnessPlanResponse(
            success=False,
            message=f"生成失败: {str(e)}"
        )

@router.get("/health")
async def health_check():
    """健康检查"""
    return {"status": "ok", "message": "服务正常运行"}
```

### 第4步：前端类型定义

**文件：`frontend/src/types/index.ts`**

```typescript
// 用户信息
export interface UserProfile {
  name: string
  age: number
  height: number
  current_weight: number
  target_weight: number
  fitness_level: string
  fitness_goals: string[]
  available_days_per_week: number
  workout_duration_minutes: number
  injury_history?: string
}

// 运动项目
export interface Exercise {
  name: string
  category: string
  duration_minutes: number
  calories_burned: number
  intensity: string
  description: string
  equipment?: string
  difficulty: string
}

// 单日运动计划
export interface DayExercisePlan {
  day: number
  weekday: string
  exercises: Exercise[]
  total_duration: number
  total_calories: number
  rest_day: boolean
  tips?: string
}

// 营养信息
export interface Meal {
  meal_type: string
  name: string
  foods: string[]
  calories: number
  protein_grams: number
  carbs_grams: number
  fat_grams: number
  description?: string
}

// 单日营养计划
export interface DayNutritionPlan {
  day: number
  meals: Meal[]
  total_calories: number
  total_protein: number
  total_carbs: number
  total_fat: number
  hydration_liters: number
  notes?: string
}

// 完整健身计划
export interface FitnessPlan {
  user_profile: UserProfile
  duration_weeks: number
  start_date: string
  end_date: string
  exercise_plans: DayExercisePlan[]
  nutrition_plans: DayNutritionPlan[]
  overall_suggestion: string
  expected_results?: string
}

// 请求类型
export interface FitnessPlanRequest {
  name: string
  age: number
  height: number
  current_weight: number
  target_weight: number
  fitness_level: string
  fitness_goals: string[]
  available_days_per_week: number
  workout_duration_minutes: number
  duration_weeks: number
  injury_history?: string
  dietary_restrictions?: string
}

// 响应类型
export interface FitnessPlanResponse {
  success: boolean
  plan?: FitnessPlan
  message?: string
}
```

### 第5步：前端 API 服务

**文件：`frontend/src/services/api.ts`**

```typescript
import axios from 'axios'
import type { FitnessPlanRequest, FitnessPlan, FitnessPlanResponse } from '../types'

const api = axios.create({
  baseURL: 'http://localhost:8000/api',
  timeout: 180000, // 3分钟超时（AI生成需要时间）
  headers: {
    'Content-Type': 'application/json'
  }
})

// 请求拦截器
api.interceptors.request.use(
  config => {
    console.log('📤 发送请求：', config.url)
    return config
  },
  error => Promise.reject(error)
)

// 响应拦截器
api.interceptors.response.use(
  response => {
    console.log('📥 收到响应：', response.status)
    return response
  },
  error => {
    console.error('❌ 请求失败：', error)
    return Promise.reject(error)
  }
)

/**
 * 生成健身计划
 */
export const generateFitnessPlan = async (
  request: FitnessPlanRequest
): Promise<FitnessPlanResponse> => {
  const response = await api.post<FitnessPlanResponse>('/fitness/plan', request)
  return response.data
}

/**
 * 健康检查
 */
export const healthCheck = async () => {
  const response = await api.get('/health')
  return response.data
}
```

### 第6步：前端页面

**文件：`frontend/src/views/Home.vue`**

```vue
<template>
  <div class="home-container">
    <div class="page-header">
      <h1 class="page-title">🏋️ 智能健身小助手</h1>
      <p class="page-subtitle">基于AI的个性化健身规划</p>
    </div>
    
    <a-card class="form-card">
      <a-form :model="formData" @finish="handleSubmit" layout="vertical">
        <!-- 基本信息部分 -->
        <a-divider orientation="left">基本信息</a-divider>
        
        <a-row :gutter="16">
          <a-col :span="12">
            <a-form-item label="姓名" name="name" :rules="[{ required: true }]">
              <a-input v-model:value="formData.name" placeholder="请输入姓名" />
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="年龄" name="age" :rules="[{ required: true }]">
              <a-input-number v-model:value="formData.age" :min="18" :max="100" />
            </a-form-item>
          </a-col>
        </a-row>
        
        <!-- 身体数据部分 -->
        <a-divider orientation="left">身体数据</a-divider>
        
        <a-row :gutter="16">
          <a-col :span="8">
            <a-form-item label="身高(cm)" name="height" :rules="[{ required: true }]">
              <a-input-number v-model:value="formData.height" :min="100" :max="250" />
            </a-form-item>
          </a-col>
          <a-col :span="8">
            <a-form-item label="当前体重(kg)" name="current_weight" :rules="[{ required: true }]">
              <a-input-number v-model:value="formData.current_weight" :min="20" :max="300" />
            </a-form-item>
          </a-col>
          <a-col :span="8">
            <a-form-item label="目标体重(kg)" name="target_weight" :rules="[{ required: true }]">
              <a-input-number v-model:value="formData.target_weight" :min="20" :max="300" />
            </a-form-item>
          </a-col>
        </a-row>
        
        <!-- 健身目标和等级 -->
        <a-divider orientation="left">健身计划</a-divider>
        
        <a-row :gutter="16">
          <a-col :span="12">
            <a-form-item label="运动等级" name="fitness_level" :rules="[{ required: true }]">
              <a-select v-model:value="formData.fitness_level" placeholder="选择等级">
                <a-select-option value="beginner">初级 - 完全新手</a-select-option>
                <a-select-option value="intermediate">中级 - 有一定经验</a-select-option>
                <a-select-option value="advanced">高级 - 经验丰富</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="健身目标" name="fitness_goals" :rules="[{ required: true }]">
              <a-select v-model:value="formData.fitness_goals" mode="multiple" placeholder="可选多个目标">
                <a-select-option value="lose_weight">减脂塑形</a-select-option>
                <a-select-option value="build_muscle">增肌强化</a-select-option>
                <a-select-option value="improve_health">改善体质</a-select-option>
                <a-select-option value="maintain">维持保健</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
        </a-row>
        
        <!-- 时间和强度 -->
        <a-row :gutter="16">
          <a-col :span="8">
            <a-form-item label="每周运动天数" name="available_days_per_week" :rules="[{ required: true }]">
              <a-select v-model:value="formData.available_days_per_week">
                <a-select-option v-for="i in 7" :key="i" :value="i">{{ i }}天</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
          <a-col :span="8">
            <a-form-item label="每次时长(分钟)" name="workout_duration_minutes" :rules="[{ required: true }]">
              <a-select v-model:value="formData.workout_duration_minutes">
                <a-select-option value="30">30分钟</a-select-option>
                <a-select-option value="45">45分钟</a-select-option>
                <a-select-option value="60">60分钟</a-select-option>
                <a-select-option value="90">90分钟</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
          <a-col :span="8">
            <a-form-item label="计划周期" name="duration_weeks" :rules="[{ required: true }]">
              <a-select v-model:value="formData.duration_weeks">
                <a-select-option value="4">4周</a-select-option>
                <a-select-option value="8">8周</a-select-option>
                <a-select-option value="12">12周</a-select-option>
              </a-select>
            </a-form-item>
          </a-col>
        </a-row>
        
        <!-- 特殊信息 -->
        <a-divider orientation="left">补充信息</a-divider>
        
        <a-row :gutter="16">
          <a-col :span="12">
            <a-form-item label="伤病史" name="injury_history">
              <a-textarea 
                v-model:value="formData.injury_history" 
                placeholder="如有旧伤或不适请说明，如腰椎间盘突出、膝盖问题等"
                :rows="2"
              />
            </a-form-item>
          </a-col>
          <a-col :span="12">
            <a-form-item label="饮食限制" name="dietary_restrictions">
              <a-textarea 
                v-model:value="formData.dietary_restrictions" 
                placeholder="如素食、过敏、宗教信仰等"
                :rows="2"
              />
            </a-form-item>
          </a-col>
        </a-row>
        
        <!-- 提交按钮 -->
        <a-form-item>
          <a-button type="primary" html-type="submit" size="large" :loading="loading" block>
            🚀 生成个性化计划
          </a-button>
        </a-form-item>
        
        <!-- 加载进度条 -->
        <a-form-item v-if="loading">
          <a-progress :percent="loadingProgress" status="active" :show-info="true" />
          <p style="margin-top: 16px; text-align: center; font-size: 14px; color: #666;">
            {{ loadingStatus }}
          </p>
        </a-form-item>
      </a-form>
    </a-card>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'
import { useRouter } from 'vue-router'
import { message } from 'ant-design-vue'
import { generateFitnessPlan } from '@/services/api'
import type { FitnessPlanRequest } from '@/types'

const router = useRouter()
const loading = ref(false)
const loadingProgress = ref(0)
const loadingStatus = ref('')

const formData = ref<FitnessPlanRequest>({
  name: '',
  age: 25,
  height: 170,
  current_weight: 70,
  target_weight: 65,
  fitness_level: 'beginner',
  fitness_goals: ['lose_weight'],
  available_days_per_week: 4,
  workout_duration_minutes: 60,
  duration_weeks: 12,
  injury_history: '',
  dietary_restrictions: ''
})

const handleSubmit = async () => {
  // TODO: 实现表单提交逻辑
  // 1. 显示加载状态
  // 2. 模拟进度条更新
  // 3. 调用 API 生成计划
  // 4. 导航到结果页面
}
</script>

<style scoped>
.home-container {
  max-width: 1000px;
  margin: 0 auto;
  padding: 40px 20px;
}

.page-header {
  text-align: center;
  margin-bottom: 40px;
}

.page-title {
  font-size: 36px;
  font-weight: bold;
  margin-bottom: 10px;
  color: #333;
}

.page-subtitle {
  font-size: 16px;
  color: #666;
}

.form-card {
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
}
</style>
```

**文件：`frontend/src/views/Result.vue`**

```vue
<template>
  <div class="result-container" v-if="fitnessPlan">
    <!-- 页面头部 -->
    <div class="result-header">
      <h1>✅ 您的个性化健身计划已生成</h1>
      <a-button type="primary" @click="goHome">← 返回首页</a-button>
    </div>
    
    <!-- 用户信息卡片 -->
    <a-card title="👤 用户信息" style="margin-bottom: 20px;">
      <a-row :gutter="16">
        <a-col :span="6">
          <a-statistic title="姓名" :value="fitnessPlan.user_profile.name" />
        </a-col>
        <a-col :span="6">
          <a-statistic title="年龄" :value="fitnessPlan.user_profile.age" suffix="岁" />
        </a-col>
        <a-col :span="6">
          <a-statistic title="当前体重" :value="fitnessPlan.user_profile.current_weight" suffix="kg" />
        </a-col>
        <a-col :span="6">
          <a-statistic title="目标体重" :value="fitnessPlan.user_profile.target_weight" suffix="kg" />
        </a-col>
      </a-row>
    </a-card>
    
    <!-- 运动计划 -->
    <a-card title="🏋️ 运动计划" style="margin-bottom: 20px;">
      <a-collapse>
        <a-collapse-panel 
          v-for="day in fitnessPlan.exercise_plans" 
          :key="day.day"
          :header="`${day.weekday} - 第${day.day}天`"
        >
          <div v-if="!day.rest_day">
            <p>⏱️ 总时长: {{ day.total_duration }} 分钟</p>
            <p>🔥 消耗热量: {{ day.total_calories }} kcal</p>
            <a-divider />
            <div v-for="(exercise, index) in day.exercises" :key="index" style="margin-bottom: 16px;">
              <h4>{{ index + 1 }}. {{ exercise.name }}</h4>
              <p>📊 强度: {{ exercise.intensity }} | ⏱️ {{ exercise.duration_minutes }}分钟 | 🔥 {{ exercise.calories_burned }}kcal</p>
              <p>{{ exercise.description }}</p>
            </div>
          </div>
          <div v-else>
            <a-empty description="休息日" />
          </div>
        </a-collapse-panel>
      </a-collapse>
    </a-card>
    
    <!-- 营养计划 -->
    <a-card title="🍽️ 营养计划" style="margin-bottom: 20px;">
      <a-collapse>
        <a-collapse-panel 
          v-for="day in fitnessPlan.nutrition_plans" 
          :key="day.day"
          :header="`第${day.day}天 - ${day.total_calories} kcal`"
        >
          <a-row :gutter="16" style="margin-bottom: 16px;">
            <a-col :span="6">
              <a-statistic title="蛋白质" :value="day.total_protein" suffix="g" />
            </a-col>
            <a-col :span="6">
              <a-statistic title="碳水" :value="day.total_carbs" suffix="g" />
            </a-col>
            <a-col :span="6">
              <a-statistic title="脂肪" :value="day.total_fat" suffix="g" />
            </a-col>
            <a-col :span="6">
              <a-statistic title="饮水" :value="day.hydration_liters" suffix="L" />
            </a-col>
          </a-row>
          <a-divider />
          <div v-for="(meal, index) in day.meals" :key="index" style="margin-bottom: 16px;">
            <h4>{{ meal.meal_type }} - {{ meal.name }}</h4>
            <p>🍴 {{ meal.foods.join(' + ') }}</p>
            <p>热量: {{ meal.calories }}kcal | 蛋白质: {{ meal.protein_grams }}g</p>
          </div>
        </a-collapse-panel>
      </a-collapse>
    </a-card>
    
    <!-- 总体建议 -->
    <a-card title="💡 总体建议" style="margin-bottom: 20px;">
      <p>{{ fitnessPlan.overall_suggestion }}</p>
    </a-card>
    
    <!-- 操作按钮 -->
    <a-space style="width: 100%; margin-bottom: 20px;">
      <a-button type="primary" @click="editPlan">✏️ 编辑计划</a-button>
      <a-button @click="exportPDF">📥 导出PDF</a-button>
      <a-button @click="goHome">← 返回首页</a-button>
    </a-space>
  </div>
  <a-empty v-else description="未找到计划" />
</template>

<script setup lang="ts">
import { ref, onMounted } from 'vue'
import { useRouter, useRoute } from 'vue-router'
import type { FitnessPlan } from '@/types'

const router = useRouter()
const route = useRoute()
const fitnessPlan = ref<FitnessPlan | null>(null)

onMounted(() => {
  // TODO: 从路由状态获取计划数据
  // fitnessPlan.value = route.state?.fitnessPlan
})

const goHome = () => {
  router.push('/')
}

const editPlan = () => {
  // TODO: 实现编辑功能
}

const exportPDF = () => {
  // TODO: 使用 html2canvas 和 jsPDF 导出
}
</script>

<style scoped>
.result-container {
  max-width: 1200px;
  margin: 0 auto;
  padding: 40px 20px;
}

.result-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 30px;
}

.result-header h1 {
  font-size: 28px;
  margin: 0;
}
</style>
```

## 🚀 运行流程

### 后端启动

```bash
cd backend
pip install -r requirements.txt
cp .env.example .env
# 编辑 .env 填入 LLM API 密钥
uvicorn app.api.main:app --reload
```

访问 http://localhost:8000/docs 查看 API 文档

### 前端启动

```bash
cd frontend
npm install
npm run dev
```

访问 http://localhost:5173 使用应用

## 📝 实现清单

- [ ] 后端数据模型（schemas.py） - ✅ 已提供
- [ ] ExerciseAgent 实现
- [ ] NutritionAgent 实现
- [ ] ProgressAgent 实现
- [ ] PlannerAgent 实现
- [ ] API 路由（main.py 和 routes.py）
- [ ] 前端类型定义
- [ ] 前端 API 服务
- [ ] Home.vue 表单页面
- [ ] Result.vue 结果展示页
- [ ] 路由配置
- [ ] 样式美化
- [ ] 导出功能

## 💡 关键技术点

1. **多Agent协作** - 4个专业Agent分工合作
2. **Pydantic数据验证** - 确保数据类型安全
3. **异步处理** - 使用 FastAPI 的异步特性
4. **前后端分离** - Vue3 + TypeScript 调用 REST API
5. **Progress Tracking** - 用户可以跟踪进度

## 🎯 下一步

1. 实现所有Agent类
2. 完善API路由
3. 开发前端页面
4. 集成LLM API
5. 测试整个流程
6. 部署到生产环境
