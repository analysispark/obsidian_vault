import numpy as np  
import pandas as pd  
from scipy.stats import norm  
  
np.random.seed(1234)  
  
N = 298          # 표본 수  
n_factors = 9    # 잠재요인 수  
  
# items_per_factor = 6  
  
items_per_factor = {  
    "AT": 6,  
    "RG": 8,  
    "AN": 6,  
    "PS": 6,  
    "EX": 6,     
    "SN": 6,  
    "AP": 7,  
    "AD": 6,  
    "EM": 6   
}  
  
  
latent_corr = np.array([  
    [1,.4,.5,.5,.3,.2,.4,.4,.3],  
    [.4,1,.4,.4,.2,.3,.3,.4,.3],  
    [.5,.4,1,.6,.3,.2,.4,.4,.3],  
    [.5,.4,.6,1,.3,.2,.4,.5,.3],  
    [.3,.2,.3,.3,1,.5,.2,.2,.6],  
    [.2,.3,.2,.2,.5,1,.2,.2,.4],  
    [.4,.3,.4,.4,.2,.2,1,.5,.3],  
    [.4,.4,.4,.5,.2,.2,.5,1,.3],  
    [.3,.3,.3,.3,.6,.4,.3,.3,1]  
])  
  
latent_scores = np.random.multivariate_normal(  
    mean=np.zeros(n_factors),  
    cov=latent_corr,  
    size=N  
)  
  
# 문항 생성 함수 (요인부하량 .65~.80)  
  
def generate_items(latent, n_items):  
    loadings = np.random.uniform(0.65, 0.80, n_items)  
    items = []  
    for l in loadings:  
        error = np.random.normal(0, np.sqrt(1 - l**2), N)  
        item = l * latent + error  
        items.append(item)  
    return np.column_stack(items)  
  
factor_codes = ["AT","RG","AN","PS","EX","SN","AP","AD","EM"]  
  
data_blocks = []  
for i, code in enumerate(factor_codes):  
    n_items = items_per_factor[code]  
    block = generate_items(latent_scores[:, i], n_items)  
    data_blocks.append(block)  
  
data_matrix = np.hstack(data_blocks)  
  
def to_likert(x):  
    return pd.qcut(x, 5, labels=[1,2,3,4,5])  
  
likert_data = pd.DataFrame(  
    np.apply_along_axis(to_likert, 0, data_matrix)  
)  
  
## 문항명 부여  
  
factor_codes = ["AT","RG","AN","PS","EX","SN","AP","AD","EM"]  
  
columns = []  
for code in factor_codes:  
    for i in range(1, items_per_factor[code] + 1):  
        columns.append(f"{code}{i:02d}")  
  
likert_data.columns = columns  
  
