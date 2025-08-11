import pandas as pd

# 定义16种MBTI类型
types = ["INTJ", "ENTJ", "INFJ", "ENFJ",
         "ENTP", "ENFP", "INTP", "INFP",
         "ISTJ", "ESTJ", "ISFJ", "ESFJ",
         "ESTP", "ISTP", "ESFP", "ISFP"]

# 生成所有两两组合（不重复）
pairs = []
for i in range(len(types)):
    for j in range(i + 1, len(types)):
        pairs.append((types[i], types[j]))

# 定义简单的冲突分析模板函数
def analyze_conflict(t1, t2):
    # 基于功能群组做简单标签
    Ni_group = {"INTJ", "ENTJ", "INFJ", "ENFJ"}
    Ne_group = {"ENTP", "ENFP", "INTP", "INFP"}
    Si_group = {"ISTJ", "ESTJ", "ISFJ", "ESFJ"}
    Se_group = {"ESTP", "ISTP", "ESFP", "ISFP"}

    def group(t):
        if t in Ni_group:
            return "Ni"
        if t in Ne_group:
            return "Ne"
        if t in Si_group:
            return "Si"
        if t in Se_group:
            return "Se"

    g1, g2 = group(t1), group(t2)

    # 核心冲突简述
    if {g1, g2} == {"Ni", "Ne"}:
        core_conflict = "战略聚焦 vs 发散探索"
    elif {g1, g2} == {"Fi", "Fe"}:  # 这里没做Fi/Fe判断，简化省略
        core_conflict = "价值内向 vs 和谐外向"
    elif {g1, g2} == {"Si", "Ne"}:
        core_conflict = "稳健守成 vs 追求新奇"
    elif {g1, g2} == {"Si", "Se"}:
        core_conflict = "传统规则 vs 即时行动"
    elif {g1, g2} == {"Ni", "Se"}:
        core_conflict = "远景规划 vs 现场应变"
    else:
        core_conflict = "思路差异与优先级冲突"

    # 预防流程
    opening = "明确彼此工作优先级与底线规则"
    midterm = "定期交流进度并澄清预期变化"
    resolution = "用共同目标而非个人偏好裁决"

    return core_conflict, opening, midterm, resolution

# 生成表格
data = []
for t1, t2 in pairs:
    core_conflict, opening, midterm, resolution = analyze_conflict(t1, t2)
    data.append({
        "组合": f"{t1} × {t2}",
        "核心冲突": core_conflict,
        "开局建议": opening,
        "中期监控": midterm,
        "冲突裁决锚点": resolution
    })

df = pd.DataFrame(data)

import caas_jupyter_tools
caas_jupyter_tools.display_dataframe_to_user(name="MBTI组合冲突预防手册", dataframe=df)