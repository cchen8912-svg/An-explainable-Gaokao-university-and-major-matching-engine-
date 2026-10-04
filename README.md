# An-explainable-Gaokao-university-and-major-matching-engine-Description：

AI-powered Chinese university and major recommendation system with career salary prediction, postgraduate requirements, employment risk analysis and ROI evaluation.
An explainable Gaokao university and major matching engine that combines admission analysis, major fit, employment prospects, salary prediction, graduate degree requirements, and education ROI modeling to support data-driven college application decisions
考生分数/位次
      ↓
院校匹配
      ↓
专业匹配
      ↓
本科毕业就业分析
      ↓
↓
就业薪资预测
↓
本科就业难度
↓
是否通常需要硕士
↓
是否通常需要博士
↓
达到目标职业所需学历年限
      ↓
机会成本/预期收益
      ↓
最终志愿匹配结果
from dataclasses import dataclass
from typing import Optional


@dataclass
class SalaryPrediction:
    major: str
    bachelor_salary: float
    master_salary: Optional[float]
    phd_salary: Optional[float]
    salary_year: int
    confidence: float


def predict_salary(
    major: str,
    city_level: str,
    degree: str,
    experience_years: int,
    data
) -> SalaryPrediction:

    base_salary = data.get(major, {}).get("base_salary", 6000)

    city_multiplier = {
        "一线": 1.25,
        "新一线": 1.10,
        "二线": 0.95,
        "三线": 0.85
    }.get(city_level, 1.0)

    experience_multiplier = 1 + min(
        experience_years * 0.06,
        0.60
    )

    degree_multiplier = {
        "本科": 1.00,
        "硕士": 1.15,
        "博士": 1.35
    }.get(degree, 1.0)

    salary = (
        base_salary
        * city_multiplier
        * experience_multiplier
        * degree_multiplier
    )

    return SalaryPrediction(
        major=major,
        bachelor_salary=salary if degree == "本科" else 0,
        master_salary=salary if degree == "硕士" else None,
        phd_salary=salary if degree == "博士" else None,
        salary_year=2026,
        confidence=0.70)
        from dataclasses import dataclass
from typing import Optional


@dataclass
class SalaryPrediction:
    major: str
    bachelor_salary: float
    master_salary: Optional[float]
    phd_salary: Optional[float]
    salary_year: int
    confidence: float


def predict_salary(
    major: str,
    city_level: str,
    degree: str,
    experience_years: int,
    data
) -> SalaryPrediction:

    base_salary = data.get(major, {}).get("base_salary", 6000)

    city_multiplier = {
        "一线": 1.25,
        "新一线": 1.10,
        "二线": 0.95,
        "三线": 0.85
    }.get(city_level, 1.0)

    experience_multiplier = 1 + min(
        experience_years * 0.06,
        0.60
    )

    degree_multiplier = {
        "本科": 1.00,
        "硕士": 1.15,
        "博士": 1.35
    }.get(degree, 1.0)

    salary = (
        base_salary
        * city_multiplier
        * experience_multiplier
        * degree_multiplier
    )

    return SalaryPrediction(
        major=major,
        bachelor_salary=salary if degree == "本科" else 0,
        master_salary=salary if degree == "硕士" else None,
        phd_salary=salary if degree == "博士" else None,
        salary_year=2026,
        confidence=0.70)
专业
+ 城市
+ 学历
+ 学校层次
+ 行业
+ 工作年限
+ 历史招聘数据
+ 宏观就业数据
+ 专业供需
         ↓
    薪资预测模型
         ↓
P25 / P50 / P75
{"major": "计算机科学与技术",
  "city": "杭州",
  "degree": "本科",
  "salary_prediction": { "p25": 7000, "p50": 9500,"p75": 14000}
Graduate Degree Requirement Analyzer
from enum import Enum
from dataclasses import dataclass


class DegreeRequirement(Enum):
    BACHELOR = "本科通常可就业"
    MASTER_PREFERRED = "硕士明显更有利"
    MASTER_COMMON = "硕士较常见"
    DOCTOR_COMMON = "博士较常见"
    DOCTOR_REQUIRED = "博士是主要就业门槛"


@dataclass
class DegreeRequirementResult:
    major: str
    requirement: DegreeRequirement
    bachelor_employment_rate: float
    master_advantage: float
    phd_advantage: float
    explanation: str
    就业薪资
就业概率
学历要求
继续深造概率
就业城市
行业集中度
替代风险
def employment_score(
    salary_score: float,
    employment_rate: float,
    degree_requirement: float,
    industry_demand: float,
    competition: float
) -> float:

    score = (
        salary_score * 0.30
        + employment_rate * 0.30
        + industry_demand * 0.20
        + degree_requirement * 0.10
        + competition * 0.10)
       return round(score, 2)
                     高考志愿
                  ↓
              本科四年
                  ↓
        ┌─────────┴─────────┐
        ↓                   ↓
   本科直接就业          继续读硕士
        ↓                   ↓
    起薪预测            硕士起薪预测
        ↓                   ↓
    3年薪资              3年薪资
        ↓                   ↓
    5年薪资              5年薪资
        └─────────┬─────────┘
                  ↓
             长期职业路径
                  ↓
           是否需要博士
                  ↓
          职业上限/岗位范围
          ━━━━━━━━━━━━━━━━━━━━━━━━━━━━
志愿匹配结果
━━━━━━━━━━━━━━━━━━━━━━━━━━━━

学校：XXX大学
专业：电子信息工程

高考匹配度：91.4
录取风险：稳

【本科就业】
本科直接就业：★★★★★
就业市场需求：★★★★☆
本科就业门槛：较低

【学历提醒】
硕士：不是普遍硬门槛
博士：一般不是企业就业必要条件

【薪资预测】
本科毕业：
P25：¥7,000
P50：¥9,000
P75：¥13,000

硕士毕业：
P25：¥9,000
P50：¥12,000
P75：¥17,000

【职业方向】
电子信息 → 芯片/通信/硬件/嵌入式
→ AI硬件/汽车电子/工业控制

【主要风险】
1. 部分研发岗位硕士占比提高
2. 行业技术迭代速度较快
3. 城市选择会明显影响薪资

【系统结论】
本科具有直接就业路径；
硕士主要用于扩大研发岗位选择范围，
并非所有就业岗位的必要条件。
def education_roi(
    undergraduate_years: int,
    graduate_years: int,
    bachelor_salary: float,
    master_salary: float,
    tuition_cost: float,
    living_cost: float
) -> float:

    opportunity_cost = (
        bachelor_salary * graduate_years
        + tuition_cost
        + living_cost
    )

    annual_gain = master_salary - bachelor_salary

    if annual_gain <= 0:
        return 0

    return opportunity_cost / annual_gain
    硕士额外投入：2.5年
预计教育成本：¥X
本科提前就业机会成本：¥Y
硕士薪资中位数增量：¥Z
估计回收周期：N年
1. Admission Matcher
   高考录取匹配

2. Major Matcher
   专业兴趣/选科/职业方向匹配

3. Employment Analyzer
   就业难度与行业需求

4. Salary Predictor
   薪资区间预测 P25/P50/P75

5. Degree Requirement Analyzer
   本科/硕士/博士学历门槛分析

6. Education ROI Engine
   本科 vs 硕士 vs 博士长期收益比较
   Recommendation Engine
        ↓
冲 / 稳 / 保
        ↓
学校 + 专业
        ↓
就业
        ↓
薪资
        ↓
学历路径
        ↓
长期职业路径
gaokao-volunteer-matcher/
├── main.py                    # 程序入口
├── README.md                  # GitHub 项目说明
├── requirements.txt
├── LICENSE
│
├── data/
│   ├── admission_history.csv  # 示例录取数据
│   └── career_profiles.csv    # 示例就业/薪资/学历数据
│
├── src/
│   ├── models.py              # 数据模型
│   ├── data_loader.py         # CSV数据读取
│   ├── scoring.py             # 位次、风险、综合评分
│   ├── career.py              # 薪资预测、硕博提醒、ROI
│   └── matcher.py             # 核心志愿匹配引擎
│
└── tests/
    ├── test_scoring.py
    ├── test_career.py
    └── test_matcher.py
   6 passed in 0.06s
