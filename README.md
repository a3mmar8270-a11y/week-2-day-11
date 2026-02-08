import pandas as pd
import numpy as np

# 1) Load numeric data with outliers
df = pd.read_csv("/mnt/data/day11_income (1).csv")

# نفترض أن العمود الرقمي اسمه income
s = df["income"]


# 2) Implement percentile capping (winsorization)
def winsorize_series(s, lower_q=0.01, upper_q=0.99):
    lower = s.quantile(lower_q)
    upper = s.quantile(upper_q)
    return s.clip(lower=lower, upper=upper)


# 3) Implement removal strategy
def remove_upper_tail(s, upper_q=0.99):
    upper = s.quantile(upper_q)
    return s[s <= upper]


# 4) Compare summary stats before/after
winsorized = winsorize_series(s)
removed = remove_upper_tail(s)

summary_stats = pd.DataFrame({
    "Original": s.describe(),
    "Winsorized": winsorized.describe(),
    "Upper Tail Removed": removed.describe()
})

print(summary_stats)
