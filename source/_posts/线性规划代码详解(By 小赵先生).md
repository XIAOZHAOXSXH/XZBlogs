---
title: 线性规划代码详解(By 小赵先生)
abbrlink: ac36e683
date: 2024-01-16 19:57:08
---

# 线性规划代码详解(By 小赵先生)

## modified\_simulation\_code\_corrected.py

**此文件用于模拟母羊的生产周期，并生成相应的图表。**

### 代码内容

```
 
 # 导入所需的库
 import numpy as np
 import matplotlib.pyplot as plt
 import math
 
 # 常数定义
 DAYS_IN_YEAR = 365
 INITIAL_PENS_AVAILABLE = 168
 
 # 基于提供数据的系数
 conception_rate = 0.85  # 受孕率
 mating_days = 20       # 交配天数
 pregnancy_days_range = [147, 150]  # 怀孕天数范围
 base_lactation_days = 40   # 基础哺乳天数
 base_resting_days = 18     # 基础休息天数
 average_lambs_per_ewe = 2.2  # 母羊的平均小羊数
 lamb_mortality_rate = 0.03   # 小羊死亡率
 pen_capacity = {'mating': 14, 'pregnant': 8, 'lactation': 6, 'fattening': 14}  # 每个围栏的容量
 pen_cost = {'unused': 1, 'rented': 3}  # 围栏的费用
 ewes_per_batch = 239  # 每批母羊的数量
 
 # 下面是模拟代码的开始部分，为了简洁，这里仅展示了前面的部分。
 
 import numpy as np
 import matplotlib.pyplot as plt
 import math
 
 # Constants
 DAYS_IN_YEAR = 365
 INITIAL_PENS_AVAILABLE = 168
 
 # Coefficients based on provided data
 conception_rate = 0.85
 mating_days = 20
 pregnancy_days_range = [147, 150]
 base_lactation_days = 40
 base_resting_days = 18
 average_lambs_per_ewe = 2.2
 lamb_mortality_rate = 0.03
 pen_capacity = {'mating': 14, 'pregnant': 8, 'lactation': 6, 'fattening': 14}
 pen_cost = {'unused': 1, 'rented': 3}
 ewes_per_batch = 239
 
 # Simulation code
 
 # Initialize variables for dynamic programming
 N = [0] * (DAYS_IN_YEAR + 1)
 P = [0] * (DAYS_IN_YEAR + 1)
 L = [0] * (DAYS_IN_YEAR + 1)
 F = [0] * (DAYS_IN_YEAR + 1)
 loss_day = [0] * (DAYS_IN_YEAR + 1)
 pens_required_day = [0] * (DAYS_IN_YEAR + 1)
 
 # Start with initial number of ewes available for mating
 N[0] = ewes_per_batch
 
 for t in range(DAYS_IN_YEAR - max(pregnancy_days_range) - base_lactation_days - base_resting_days):
     # Mating phase
     newly_pregnant_ewes = np.random.binomial(N[t], conception_rate)
     N[t + mating_days] += N[t] - newly_pregnant_ewes
     N[t] -= newly_pregnant_ewes
     
     # Pregnancy phase
     pregnancy_duration = np.random.choice(pregnancy_days_range, newly_pregnant_ewes)
     unique_durations, counts = np.unique(pregnancy_duration, return_counts=True)
     for duration, count in zip(unique_durations, counts):
         L[t + duration] += count
         P[t] -= sum(counts)
     
     # Lactation phase
     lambs_born = np.random.poisson(average_lambs_per_ewe, L[t])
     lambs_surviving = lambs_born - np.random.binomial(lambs_born, lamb_mortality_rate)
     F[t + base_lactation_days] += sum(lambs_surviving)
     N[t + base_lactation_days + base_resting_days] += L[t]
     L[t] = 0
     
     # Calculate pens required for each phase
     pens_required_day[t] = (
         math.ceil(N[t] / pen_capacity['mating']) +
         math.ceil(P[t] / pen_capacity['pregnant']) +
         math.ceil(L[t] / pen_capacity['lactation']) +
         math.ceil(F[t] / pen_capacity['fattening'])
     )
     
     # Calculate loss for the day
     if pens_required_day[t] <= INITIAL_PENS_AVAILABLE:
         loss_day[t] = (INITIAL_PENS_AVAILABLE - pens_required_day[t]) * pen_cost['unused']
     else:
         loss_day[t] = (pens_required_day[t] - INITIAL_PENS_AVAILABLE) * pen_cost['rented']
 
 # Visualization of the results
 plt.figure(figsize=(15, 6))
 
 # Plot for ewes required
 plt.subplot(1, 2, 1)
 plt.plot(pens_required_day, color='skyblue')
 plt.axhline(y=INITIAL_PENS_AVAILABLE, color='r', linestyle='--')
 plt.title("Daily Ewes Required")
 plt.xlabel("Days")
 plt.ylabel("Number of Ewes")
 plt.legend(['Ewes Required', 'Ewes Available'])
 
 # Plot for daily loss
 plt.subplot(1, 2, 2)
 plt.plot(loss_day, color='lightcoral')
 plt.title("Daily Loss in Ewes")
 plt.xlabel("Days")
 plt.ylabel("Loss Value")
 plt.tight_layout()
 plt.show()
 
 
```

### 代码详解

* **导入所需的库：**`numpy` 用于数学运算，`matplotlib.pyplot` 用于绘制图表，`math` 用于基本数学函数。
* **定义了一系列常数和系数，如 **`DAYS_IN_YEAR` 表示一年的天数，`INITIAL_PENS_AVAILABLE` 表示初始可用的围栏数量。
* `conception_rate` 是受孕率，表示母羊受孕的概率。
* `mating_days` 是交配期的天数。
* `pregnancy_days_range` 是怀孕期的天数范围。
* `base_lactation_days` 是基础哺乳天数。
* `base_resting_days` 是基础休息天数。
* `average_lambs_per_ewe` 是每只母羊平均产的小羊数量。
* `lamb_mortality_rate` 是小羊的死亡率。
* `pen_capacity` 是一个字典，描述了不同类型围栏的容量。
* `pen_cost` 是一个字典，描述了围栏的费用。
* `ewes_per_batch` 是每批母羊的数量。

### 运行结果

**上面的图表显示了模拟的结果。**

## modified\_code\_no\_visual(1)(1).py

**此文件用于模拟母羊的生产周期，但不生成可视化结果。**

### 代码内容

```
 
 # 导入所需的库
 import numpy as np
 import math
 
 # 常数定义
 DAYS_IN_YEAR = 365
 INITIAL_PENS_AVAILABLE = 168
 
 # 基于提供数据的系数
 conception_rate = 0.85  # 受孕率
 mating_days = 20       # 交配天数
 pregnancy_days_range = [147, 150]  # 怀孕天数范围
 base_lactation_days = 40   # 基础哺乳天数
 base_resting_days = 18     # 基础休息天数
 average_lambs_per_ewe = 2.2  # 母羊的平均小羊数
 lamb_mortality_rate = 0.03   # 小羊死亡率
 pen_capacity = {'mating': 14, 'pregnant': 8, 'lactation': 6, 'fattening': 14}  # 每个围栏的容量
 pen_cost = {'unused': 1, 'rented': 3}  # 围栏的费用
 ewes_per_batch = 239  # 每批母羊的数量
 
 # 下面是模拟代码的开始部分，为了简洁，这里仅展示了前面的部分。
 
 import numpy as np
 import matplotlib.pyplot as plt
 import math
 
 # Constants
 DAYS_IN_YEAR = 365
 INITIAL_PENS_AVAILABLE = 239
 
 # Coefficients based on provided data
 conception_rate = 0.85
 mating_days = 20
 pregnancy_days_range = [147, 150]
 base_lactation_days = 40
 base_resting_days = 18
 average_lambs_per_ewe = 2.2
 lamb_mortality_rate = 0.03
 pen_capacity = {'mating': 14, 'pregnant': 8, 'lactation': 6, 'fattening': 14}
 pen_cost = {'unused': 1, 'rented': 3}
 ewes_per_batch = 239
 
 # Simulation code
 
 # Initialize variables for dynamic programming
 N = [0] * (DAYS_IN_YEAR + 1)
 P = [0] * (DAYS_IN_YEAR + 1)
 L = [0] * (DAYS_IN_YEAR + 1)
 F = [0] * (DAYS_IN_YEAR + 1)
 loss_day = [0] * (DAYS_IN_YEAR + 1)
 pens_required_day = [0] * (DAYS_IN_YEAR + 1)
 
 # Start with initial number of ewes available for mating
 N[0] = ewes_per_batch
 
 for t in range(DAYS_IN_YEAR - max(pregnancy_days_range) - base_lactation_days - base_resting_days):
     # Mating phase
     newly_pregnant_ewes = np.random.binomial(N[t], conception_rate)
     N[t + mating_days] += N[t] - newly_pregnant_ewes
     N[t] -= newly_pregnant_ewes
     
     # Pregnancy phase
     pregnancy_duration = np.random.choice(pregnancy_days_range, newly_pregnant_ewes)
     unique_durations, counts = np.unique(pregnancy_duration, return_counts=True)
     for duration, count in zip(unique_durations, counts):
         L[t + duration] += count
         P[t] -= sum(counts)
     
     # Lactation phase
     lambs_born = np.random.poisson(average_lambs_per_ewe, L[t])
     lambs_surviving = lambs_born - np.random.binomial(lambs_born, lamb_mortality_rate)
     F[t + base_lactation_days] += sum(lambs_surviving)
     N[t + base_lactation_days + base_resting_days] += L[t]
     L[t] = 0
     
     # Calculate pens required for each phase
     pens_required_day[t] = (
         math.ceil(N[t] / pen_capacity['mating']) +
         math.ceil(P[t] / pen_capacity['pregnant']) +
         math.ceil(L[t] / pen_capacity['lactation']) +
         math.ceil(F[t] / pen_capacity['fattening'])
     )
     
     # Calculate loss for the day
     if pens_required_day[t] <= INITIAL_PENS_AVAILABLE:
         loss_day[t] = (INITIAL_PENS_AVAILABLE - pens_required_day[t]) * pen_cost['unused']
     else:
         loss_day[t] = (pens_required_day[t] - INITIAL_PENS_AVAILABLE) * pen_cost['rented']
 
 
 
```

### 代码详解

* **导入所需的库：**`numpy` 用于数学运算，`math` 用于基本数学函数。
* **定义了一系列常数和系数，如 **`DAYS_IN_YEAR` 表示一年的天数，`INITIAL_PENS_AVAILABLE` 表示初始可用的围栏数量。
* `conception_rate` 是受孕率，表示母羊受孕的概率。
* `mating_days` 是交配期的天数。
* `pregnancy_days_range` 是怀孕期的天数范围。
* `base_lactation_days` 是基础哺乳天数。
* `base_resting_days` 是基础休息天数。
* `average_lambs_per_ewe` 是每只母羊平均产的小羊数量。
* `lamb_mortality_rate` 是小羊的死亡率。
* `pen_capacity` 是一个字典，描述了不同类型围栏的容量。
* `pen_cost` 是一个字典，描述了围栏的费用。
* `ewes_per_batch` 是每批母羊的数量。

**此代码不生成可视化结果。**

## modified\_user\_simulation\_code(1).py

**此文件用于模拟母羊的生产周期，并生成相应的图表。**

### 代码内容

```
 
 # 导入所需的库
 import numpy as np
 import matplotlib.pyplot as plt
 import math
 
 # 常数定义
 DAYS_IN_YEAR = 365
 INITIAL_PENS_AVAILABLE = 168
 
 # 基于提供数据的系数
 conception_rate = 0.85  # 受孕率
 mating_days = 20       # 交配天数
 pregnancy_days_range = [147, 150]  # 怀孕天数范围
 base_lactation_days = 40   # 基础哺乳天数
 base_resting_days = 18     # 基础休息天数
 average_lambs_per_ewe = 2.2  # 母羊的平均小羊数
 lamb_mortality_rate = 0.03   # 小羊死亡率
 pen_capacity = {'mating': 14, 'pregnant': 8, 'lactation': 6, 'fattening': 14}  # 每个围栏的容量
 pen_cost = {'unused': 1, 'rented': 3}  # 围栏的费用
 ewes_per_batch = 239  # 每批母羊的数量
 
 # 下面是模拟代码的开始部分，为了简洁，这里仅展示了前面的部分。
 
 import numpy as np
 import matplotlib.pyplot as plt
 import math
 
 # Simulation parameters
 n_simulations = 1000
 
 # Lists to store results
 pens_required_list = []
 lamb_output_list = []
 
 # Using provided values for variables
 ewes_per_batch = 168
 conception_rate = 0.85
 pregnancy_days_range = range(147, 151)
 average_lambs_per_ewe = 2.2
 lamb_mortality_rate = 0.03
 pen_capacity = {
     'mating': 14,
     'pregnant': 8,
     'lactation': 6,
     'fattening': 14
 }
 
 # Simulate the breeding process
 for _ in range(n_simulations):
     # Mating phase
     ewes_mated = ewes_per_batch
     ewes_pregnant = np.random.binomial(ewes_mated, conception_rate)
     
     # Pregnancy phase
     pregnancy_duration = np.random.choice(pregnancy_days_range, ewes_pregnant)
     
     # Lactation phase
     lambs_born = np.random.poisson(average_lambs_per_ewe, ewes_pregnant)
     lambs_surviving = lambs_born - np.random.binomial(lambs_born, lamb_mortality_rate)
     
     # Calculate pens required for each phase
     pens_mating = math.ceil(ewes_mated / pen_capacity['mating'])
     pens_pregnant = math.ceil(ewes_pregnant / pen_capacity['pregnant'])
     pens_lactation = math.ceil(ewes_pregnant / pen_capacity['lactation'])
     pens_fattening = math.ceil(sum(lambs_surviving) / pen_capacity['fattening'])
     
     total_pens = pens_mating + pens_pregnant + pens_lactation + pens_fattening
     pens_required_list.append(total_pens)
     lamb_output_list.append(sum(lambs_surviving))
 
 # Visualization
 plt.figure(figsize=(15, 6))
 
 # Plot for pens required
 plt.subplot(1, 2, 1)
 plt.hist(pens_required_list, bins=30, color='skyblue', edgecolor='black')
 plt.title("Distribution of Pens Required")
 plt.xlabel("Number of Pens")
 plt.ylabel("Frequency")
 
 # Plot for lamb output
 plt.subplot(1, 2, 2)
 plt.hist(lamb_output_list, bins=30, color='lightcoral', edgecolor='black')
 plt.title("Distribution of Lamb Output")
 plt.xlabel("Number of Lambs")
 plt.ylabel("Frequency")
 
 plt.tight_layout()
 plt.show()
 
```

### 代码详解

* **导入所需的库：**`numpy` 用于数学运算，`matplotlib.pyplot` 用于绘制图表，`math` 用于基本数学函数。
* **定义了一系列常数和系数，如 **`DAYS_IN_YEAR` 表示一年的天数，`INITIAL_PENS_AVAILABLE` 表示初始可用的围栏数量。
* `conception_rate` 是受孕率，表示母羊受孕的概率。
* `mating_days` 是交配期的天数。
* `pregnancy_days_range` 是怀孕期的天数范围。
* `base_lactation_days` 是基础哺乳天数。
* `base_resting_days` 是基础休息天数。
* `average_lambs_per_ewe` 是每只母羊平均产的小羊数量。
* `lamb_mortality_rate` 是小羊的死亡率。
* `pen_capacity` 是一个字典，描述了不同类型围栏的容量。
* `pen_cost` 是一个字典，描述了围栏的费用。
* `ewes_per_batch` 是每批母羊的数量。

### 运行结果

**上面的图表显示了模拟的结果。**

## modified\_code\_no\_visual(1).py

**此文件用于模拟母羊的生产周期，但不生成可视化结果。**

### 代码内容

```
 
 # 导入所需的库
 import numpy as np
 import math
 
 # 常数定义
 DAYS_IN_YEAR = 365
 INITIAL_PENS_AVAILABLE = 168
 
 # 基于提供数据的系数
 conception_rate = 0.85  # 受孕率
 mating_days = 20       # 交配天数
 pregnancy_days_range = [147, 150]  # 怀孕天数范围
 base_lactation_days = 40   # 基础哺乳天数
 base_resting_days = 18     # 基础休息天数
 average_lambs_per_ewe = 2.2  # 母羊的平均小羊数
 lamb_mortality_rate = 0.03   # 小羊死亡率
 pen_capacity = {'mating': 14, 'pregnant': 8, 'lactation': 6, 'fattening': 14}  # 每个围栏的容量
 pen_cost = {'unused': 1, 'rented': 3}  # 围栏的费用
 ewes_per_batch = 239  # 每批母羊的数量
 
 # 下面是模拟代码的开始部分，为了简洁，这里仅展示了前面的部分。
 
 import numpy as np
 import matplotlib.pyplot as plt
 import math
 
 # Constants
 DAYS_IN_YEAR = 365
 INITIAL_PENS_AVAILABLE = 239
 
 # Coefficients based on provided data
 conception_rate = 0.85
 mating_days = 20
 pregnancy_days_range = [147, 150]
 base_lactation_days = 40
 base_resting_days = 18
 average_lambs_per_ewe = 2.2
 lamb_mortality_rate = 0.03
 pen_capacity = {'mating': 14, 'pregnant': 8, 'lactation': 6, 'fattening': 14}
 pen_cost = {'unused': 1, 'rented': 3}
 ewes_per_batch = 239
 
 # Simulation code
 
 # Initialize variables for dynamic programming
 N = [0] * (DAYS_IN_YEAR + 1)
 P = [0] * (DAYS_IN_YEAR + 1)
 L = [0] * (DAYS_IN_YEAR + 1)
 F = [0] * (DAYS_IN_YEAR + 1)
 loss_day = [0] * (DAYS_IN_YEAR + 1)
 pens_required_day = [0] * (DAYS_IN_YEAR + 1)
 
 # Start with initial number of ewes available for mating
 N[0] = ewes_per_batch
 
 for t in range(DAYS_IN_YEAR - max(pregnancy_days_range) - base_lactation_days - base_resting_days):
     # Mating phase
     newly_pregnant_ewes = np.random.binomial(N[t], conception_rate)
     N[t + mating_days] += N[t] - newly_pregnant_ewes
     N[t] -= newly_pregnant_ewes
     
     # Pregnancy phase
     pregnancy_duration = np.random.choice(pregnancy_days_range, newly_pregnant_ewes)
     unique_durations, counts = np.unique(pregnancy_duration, return_counts=True)
     for duration, count in zip(unique_durations, counts):
         L[t + duration] += count
         P[t] -= sum(counts)
     
     # Lactation phase
     lambs_born = np.random.poisson(average_lambs_per_ewe, L[t])
     lambs_surviving = lambs_born - np.random.binomial(lambs_born, lamb_mortality_rate)
     F[t + base_lactation_days] += sum(lambs_surviving)
     N[t + base_lactation_days + base_resting_days] += L[t]
     L[t] = 0
     
     # Calculate pens required for each phase
     pens_required_day[t] = (
         math.ceil(N[t] / pen_capacity['mating']) +
         math.ceil(P[t] / pen_capacity['pregnant']) +
         math.ceil(L[t] / pen_capacity['lactation']) +
         math.ceil(F[t] / pen_capacity['fattening'])
     )
     
     # Calculate loss for the day
     if pens_required_day[t] <= INITIAL_PENS_AVAILABLE:
         loss_day[t] = (INITIAL_PENS_AVAILABLE - pens_required_day[t]) * pen_cost['unused']
     else:
         loss_day[t] = (pens_required_day[t] - INITIAL_PENS_AVAILABLE) * pen_cost['rented']
 
 
 
```

### 代码详解

* **导入所需的库：**`numpy` 用于数学运算，`math` 用于基本数学函数。
* **定义了一系列常数和系数，如 **`DAYS_IN_YEAR` 表示一年的天数，`INITIAL_PENS_AVAILABLE` 表示初始可用的围栏数量。
* `conception_rate` 是受孕率，表示母羊受孕的概率。
* `mating_days` 是交配期的天数。
* `pregnancy_days_range` 是怀孕期的天数范围。
* `base_lactation_days` 是基础哺乳天数。
* `base_resting_days` 是基础休息天数。
* `average_lambs_per_ewe` 是每只母羊平均产的小羊数量。
* `lamb_mortality_rate` 是小羊的死亡率。
* `pen_capacity` 是一个字典，描述了不同类型围栏的容量。
* `pen_cost` 是一个字典，描述了围栏的费用。
* `ewes_per_batch` 是每批母羊的数量。

**此代码不生成可视化结果。**
