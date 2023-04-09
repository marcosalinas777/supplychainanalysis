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

<br>
#let's have a look at the descriptive statistics of the set
print(data.describe())
<br>
![image](https://user-images.githubusercontent.com/95108103/230793762-a46a0f0d-df73-414a-b815-7b707867631c.png)



<br>
#let's look at the relationship between the price of the products and the revenue generated by them:
fig = px.scatter(data, x="Price", y="Revenue generated", color="Product type", hover_data=["Number of products sold"], trendline="ols")
fig.show()
<br>
![image](https://user-images.githubusercontent.com/95108103/230792707-2b66c0d8-f303-4b5a-8832-f0eb9f33ef85.png)

<br>
#The company derives more revenue from skincae products, and the higher the price of sincare products, the more revenue they generate.
#Now let's have a look at the sales by product type:
sales_data = data.groupby("Product type")["Number of products sold"].sum().reset_index()
pie_chart=px.pie(sales_data, values="Number of products sold", names="Product type", title="Sales by Product Type", hover_data=["Number of products sold"],hole=0.5,color_discrete_sequence=px.colors.qualitative.Pastel)
pie_chart.update_traces(textposition="inside", textinfo="percent+label")
pie_chart.show()
<br>
![image](https://user-images.githubusercontent.com/95108103/230792773-63e58f1d-a345-4bc3-9d11-536e3d1463d2.png)

<br>
#So 45% of business comes from skincare products, 29.5% from haircare and 25.5% from cosmetics
#Now let's have a look at the total revenue generated from shipping carriers
total_revenue=data.groupby("Shipping carriers")["Revenue generated"].sum().reset_index()
fig=go.Figure()
fig.add_trace(go.Bar(x=total_revenue["Shipping carriers"], y=total_revenue["Revenue generated"]))
fig.update_layout(title="Total Revenue by Shipping Carrier",
                 xaxis_title="Shipping Carrier",
                 yaxis_title="Revenue Generated")
fig.show()

<br>
![image](https://user-images.githubusercontent.com/95108103/230792821-d4e3d72d-2682-41c3-a21d-c903658db294.png)

<br>
#So the company is using 3 carriers for transportation, and carrier B helps the company in generating more revenue.
#Let's havea look at the Average lead time and Average Manufacturing Costs for all products of the company:
avg_lead_time=data.groupby("Product type")["Lead time"].mean().reset_index()
avg_manufacturing_costs=data.groupby("Product type")["Manufacturing costs"].mean().reset_index()
result=pd.merge(avg_lead_time, avg_manufacturing_costs, on="Product type")
result.rename(columns={"Lead time": "Average Lead Time", "Manufacturing costs": "Average Manufacturing Costs"},inplace=True)
print(result)

<br>
![image](https://user-images.githubusercontent.com/95108103/230792908-790af508-f1ff-440f-824e-b7eb54758021.png)

<br>
#There is a column in the data set as SKUs, which stands for Stock Keeping Units, these are special codes that help companies track
#of all different things they have for sale.  Imagine that you have a large toy store with lots of toys, each toy is different and has
#its name and price, but when you want to know how many you have left, you need a way to identify them. So you give each toy a unique
#code, like a secret number only the store knows.  This secret number is called SKU.  Let's analyze the revenue generated by each SKU:
revenue_chart=px.line(data, x="SKU",
                     y="Revenue generated",
                     title="Revenue Generated by SKU")
revenue_chart.show()

<br>
<br>

![image](https://user-images.githubusercontent.com/95108103/230793066-3d9df6dd-f55e-4750-a629-ec1eae6948cd.png)


#There is another column in the dataset as Stock levels, which refers to the number of products a store has in its inventory.
#Now let's have a look at the stock levels of each SKU
stock_chart=px.line(data, x="SKU", y="Stock levels", 
                   title="Stock Levels by SKU")
stock_chart.show()
<br>
<br>
![image](https://user-images.githubusercontent.com/95108103/230793148-6dca8c23-bb0d-4576-9b79-872fafe09005.png)


#Now let's have a look at the order quantity of eack SKU
order_quantity_chart=px.bar(data, x="SKU",
                           y="Order quantities",
                           title="Order Quantity by SKU")
order_quantity_chart.show()

<br>
![image](https://user-images.githubusercontent.com/95108103/230793218-75d87dea-0c54-4c28-8925-bfe408f56e60.png)

<br>
#Cost Analysis
#Now let's analyze the shipping cost of Carriers:
shipping_cost_chart=px.bar(data, x="Shipping carriers",
                          y="Shipping costs",
                          title="Shipping Costs by Carrier")
shipping_cost_chart.show()
<br>
![image](https://user-images.githubusercontent.com/95108103/230793253-de986b65-59bf-452d-82fd-b262ae085f57.png)

<br>
<br>
#In one of the above visualizations we discovered that Carrier B helps the company in more revenue.  It is also the most costly carrier among the 3.
#Let's have a look at the cost distribution by transportation mode:
transportation_chart=px.pie(data,
                           values="Costs",names="Transportation modes",
                           title="Cost Distribution by Transportation Mode",
                           hole=0.5,
                           color_discrete_sequence=px.colors.qualitative.Pastel)
transportation_chart.show()
<br>
<br>
![image](https://user-images.githubusercontent.com/95108103/230793347-96ceed82-c591-41e4-ad7b-9df3aa7459e3.png)

<br>
<br>
#So the company spends more on road and rail modes of transporttaion for the transportation of goods
<br>
<br>
#Analyzing Defect Rate
#The defect rate in the supply chain refers to the percentage of products that have something wrong or are found broken after shipping.
#Let's have a look at the average defect rate of all product types:
<br>
defect_rates_by_product=data.groupby("Product type")["Defect rates"].mean().reset_index()
fig=px.bar(defect_rates_by_product, x="Product type", y="Defect rates",
          title="Average Defect Rates by Product Type")
fig.show()

![image](https://user-images.githubusercontent.com/95108103/230793804-cdc826e3-3074-479e-b345-30f825bfe8df.png)


<br>
<br>
#So the defect rate of haircare products is higher.  Now let's have a look at the defect rates by mode of transportation

pivot_table=pd.pivot_table(data, values="Defect rates", index=["Transportation modes"], aggfunc="mean")
transportation_chart=px.pie(values=pivot_table["Defect rates"],
                           names=pivot_table.index,
                           title="Defect Rates by Transportation Mode",
                           hole=0.5,
                           color_discrete_sequence=px.colors.qualitative.Pastel)
transportation_chart.show()
<br>
![image](https://user-images.githubusercontent.com/95108103/230793539-a65b6c3d-58be-4bec-9210-126309b0ba01.png)

<br>
<br>
Road transportation results in a higher defect rate, and Air transportation has the lowest defect rate.
<br>
Supply Chain Analysis means analyzing various components of a Supply Chain to understand how to improve the effectiveness of the Supply Chain to create more value for customers.






