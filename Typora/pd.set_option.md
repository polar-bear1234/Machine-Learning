```python
# 显示所有列
pd.set_option('display.max_columns', None)
pd.set_option('display.max_columns', 5)  #最多显示5列
# 显示所有行
pd.set_option('display.max_rows', None)
pd.set_option('display.max_rows', 10)#最多显示10行
#显示小数位数
pd.set_option('display.float_format',lambda x: '%.2f'%x) #两位
#显示宽度
pd.set_option('display.width', 100)
#
import warnings
warnings.filterwarnings('ignore')  # 关闭运行时的警告
np.set_printoptions(linewidth=100, suppress=True)   # 打印numpy时设置显示宽度，并且不用科学计数法显示
pd.set_option('display.width', 100)   # pandas设置显示宽度
pd.set_option('precision', 1)   # 设置显示数值的精度
```