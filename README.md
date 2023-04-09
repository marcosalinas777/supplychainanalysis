# supplychainanalysis
The supply Chain is the network of production and logistics involved in producing and delivering goods to customers. And Supply Chain Analysis means analyzing various components of a Supply Chain to understand how to improve the effectiveness of the Supply Chain to create more value for customers
<br>
<br>
To analyze a company’s supply chain, we need data on the different stages of the supply chain, like data about sourcing, manufacturing, transportation, inventory management, sales and customer demographics, which can be found in the supply_chain_data.csv file
<br>
<br>
#import the necessary Python libraries and the dataset
import pandas as pd
import plotly.express as px
import plotly.io as pio
import plotly.graph_objects as go
pio.templates.default = "plotly_white"
<br>
data = pd.read_csv("supply_chain_data.csv")
print(data.head())
<br>
![image](https://user-images.githubusercontent.com/95108103/230792600-895bc7fb-983a-430f-8fac-709b5d59d179.png)
