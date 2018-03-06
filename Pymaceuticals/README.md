
# Pymaceuticals Data Analysis

#### In this study, 250 mice were treated through a variety of drug regimes over the course of 45 days. Their physiological responses were then monitored over the course of that time. The objective is to analyze the data to show how four treatments (Capomulin, Infubinol, Ketapril, and Placebo) compare.

## Observed Trends 
* Figures 1-4 all indicate that Capomulin is the superior treatment option compared with Infubinol, Ketapril, and placebo. Not only is Capomulin better, but the drugs Infubinol and Ketapril had outcomes comparable to placebo. 
* Fig. 1: Tumor Response to Treatment shows Capomulin as the most effective in tumor response, with tumor volume decreasing over the 45-day timeframe, whereas tumor volume increased for the other treatments. 
* Metastatic Spread During Treatment (Fig. 2) was also lowest for Capomulin.
* Similarly, Capomulin had the best survival rate (Fig. 3), whereas Infubinol, Ketapril, and Placebo all had much lower rates of survival. 
* Fig. 4: Tumor Change Over 45-Day Treatment shows that the Capomulin treatment resulted in about 20% reduction in tumor volume, whereas as the other treatments were not at all effective in tumor reduction, and had approximately 40-60% INCREASE in tumor volume. 









```python
# Import Dependencies
import random
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd 
```


```python
csv_path = "raw_data/clinicaltrial_data.csv"
df = pd.read_csv(csv_path)
df.head(10)
csv_path2 = "raw_data/mouse_drug_data.csv"
df2 = pd.read_csv(csv_path2)
df2.head(10)
df3 = pd.merge(df, df2, how="inner")
df3.head()
#df3["Timepoint"].value_counts()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Mouse ID</th>
      <th>Timepoint</th>
      <th>Tumor Volume (mm3)</th>
      <th>Metastatic Sites</th>
      <th>Drug</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>b128</td>
      <td>0</td>
      <td>45.000000</td>
      <td>0</td>
      <td>Capomulin</td>
    </tr>
    <tr>
      <th>1</th>
      <td>b128</td>
      <td>5</td>
      <td>45.651331</td>
      <td>0</td>
      <td>Capomulin</td>
    </tr>
    <tr>
      <th>2</th>
      <td>b128</td>
      <td>10</td>
      <td>43.270852</td>
      <td>0</td>
      <td>Capomulin</td>
    </tr>
    <tr>
      <th>3</th>
      <td>b128</td>
      <td>15</td>
      <td>43.784893</td>
      <td>0</td>
      <td>Capomulin</td>
    </tr>
    <tr>
      <th>4</th>
      <td>b128</td>
      <td>20</td>
      <td>42.731552</td>
      <td>0</td>
      <td>Capomulin</td>
    </tr>
  </tbody>
</table>
</div>




```python
tx_grp = df3.groupby("Drug")
tx_grp_df = pd.DataFrame(tx_grp.count())
tx_grp_df = tx_grp_df.reset_index()
tx_grp_df
tx = tx_grp_df.iloc[[0,2,3,5],0]
tx = pd.DataFrame(tx)
tx
#df3
# remove rows with extra tx
df4 = pd.merge(df3, tx, how="inner")
df4["Drug"].value_counts()
#df4
#Capomulin, Infubinol, Ketapril, and Placebo
tx1 = df4.loc[df4["Drug"]=="Capomulin"]
tx1
tx2 = df4.loc[df4["Drug"]=="Infubinol"]
tx2
tx3 = df4.loc[df4["Drug"]=="Ketapril"]
tx3
tx4 = df4.loc[df4["Drug"]=="Placebo"]
#tx4
```


```python
# Creating a scatter plot that shows how the tumor volume changes over time for each treatment. 

tx1_plt1 = tx1.groupby("Timepoint")["Tumor Volume (mm3)"].mean()
tx1_plt1

tx2_plt1 = tx2.groupby("Timepoint")["Tumor Volume (mm3)"].mean()
tx2_plt1

tx3_plt1 = tx3.groupby("Timepoint")["Tumor Volume (mm3)"].mean()
tx3_plt1

tx4_plt1 = tx4.groupby("Timepoint")["Tumor Volume (mm3)"].mean()
tx4_plt1

tx1_sem1 = tx1.groupby("Timepoint")["Tumor Volume (mm3)"].sem()
tx1_sem1

tx2_sem1 = tx2.groupby("Timepoint")["Tumor Volume (mm3)"].sem()
tx2_sem1

tx3_sem1 = tx3.groupby("Timepoint")["Tumor Volume (mm3)"].sem()
tx3_sem1

tx4_sem1 = tx4.groupby("Timepoint")["Tumor Volume (mm3)"].sem()
tx4_sem1

x_axis = [0, 5, 10, 15, 20, 25, 30, 35, 40, 45]
fig, ax = plt.subplots(figsize=(7,7))

ax.errorbar(x_axis, tx1_plt1, yerr=tx1_sem1, fmt=":^", capsize=4, capthick=1, color="purple", label="Capomuline") 
ax.errorbar(x_axis, tx2_plt1, yerr=tx2_sem1, fmt=":x", capsize=4, capthick=1, color="blue", label="Infubinol")
ax.errorbar(x_axis, tx3_plt1, yerr=tx3_sem1, fmt=":o", capsize=4, capthick=1, color="red", label="Ketapril")
ax.errorbar(x_axis, tx4_plt1, yerr=tx4_sem1, fmt=":D", capsize=4, capthick=1, color="green", label="Placebo")
ax.grid()
ax.set_xlim(0,45,5)
ax.set_ylim(35, 75, 5)
ax.set_xlabel("Time (Days)",fontsize=14)
ax.set_ylabel("Tumor VOlume (mm3)",fontsize=14)
plt.title("Fig. 1: Tumor Response to Treatment",fontsize=18)
plt.legend(loc="best", fontsize="large", fancybox=True)
plt.savefig("1_Change_in_Tumor_Size.png")
plt.show()
```


![png](output_4_0.png)



```python
# Creating a scatter plot that shows how the number of metastatic (cancer spreading) sites changes over time for each treatment.

tx1_plt2 = tx1.groupby("Timepoint")["Metastatic Sites"].mean()
tx1_plt2

tx2_plt2 = tx2.groupby("Timepoint")["Metastatic Sites"].mean()
tx2_plt2

tx3_plt2 = tx3.groupby("Timepoint")["Metastatic Sites"].mean()
tx3_plt2

tx4_plt2 = tx4.groupby("Timepoint")["Metastatic Sites"].mean()
tx4_plt2

tx1_sem2 = tx1.groupby("Timepoint")["Metastatic Sites"].sem()
tx1_sem2

tx2_sem2 = tx2.groupby("Timepoint")["Metastatic Sites"].sem()
tx2_sem2

tx3_sem2 = tx3.groupby("Timepoint")["Metastatic Sites"].sem()
tx3_sem2

tx4_sem2 = tx4.groupby("Timepoint")["Metastatic Sites"].sem()
tx4_sem2

x_axis = [0, 5, 10, 15, 20, 25, 30, 35, 40, 45]
y_axis = np.arange(0, 4.1, .25)
fig, ax = plt.subplots(figsize=(7,7))

ax.errorbar(x_axis, tx1_plt2, yerr=tx1_sem2, fmt="-.^", capsize=4, capthick=1, color="m", label="Capomuline") 
ax.errorbar(x_axis, tx2_plt2, yerr=tx2_sem2, fmt="-.x", capsize=4, capthick=1, color="b", label="Infubinol")
ax.errorbar(x_axis, tx3_plt2, yerr=tx3_sem2, fmt="-.o", capsize=4, capthick=1, color="r", label="Ketapril")
ax.errorbar(x_axis, tx4_plt2, yerr=tx4_sem2, fmt="-.D", capsize=4, capthick=1, color="g", label="Placebo")
ax.grid()
ax.set_xlim(0,46,5)
ax.set_ylim(0, 4, .5)
ax.set_xlabel("Treatment Duration (Days)",fontsize=14)
ax.set_ylabel("Metastatic Sites",fontsize=14)

plt.title("Fig. 2: Metastatic Spread During Treatment",fontsize=17)
plt.xticks(x_axis)
plt.yticks(y_axis)
plt.legend(loc="upper left", fontsize="large",fancybox=True)
plt.savefig("2_Metastatic_Spread.png")
plt.show()
```


![png](output_5_0.png)



```python
# Creating a scatter plot that shows the number of mice still alive through the course of treatment (Survival Rate)

tx1_plt3 = tx1.groupby("Timepoint")["Mouse ID"].count()
tx1_plt3

tx2_plt3 = tx2.groupby("Timepoint")["Mouse ID"].count()
tx2_plt3

tx3_plt3 = tx3.groupby("Timepoint")["Mouse ID"].count()
tx3_plt3

tx4_plt3 = tx4.groupby("Timepoint")["Mouse ID"].count()
tx4_plt3

tx1_0 = tx1.groupby("Timepoint")["Mouse ID"].count()[0]
tx1_0

tx2_0 = tx2.groupby("Timepoint")["Mouse ID"].count()[0]
tx2_0

tx3_0 = tx3.groupby("Timepoint")["Mouse ID"].count()[0]
tx3_0

tx4_0 = tx4.groupby("Timepoint")["Mouse ID"].count()[0]
tx4_0

sr1 = (tx1_plt3/tx1_0)*100
sr1

sr2 = (tx2_plt3/tx1_0)*100
sr2


sr3 = (tx3_plt3/tx3_0)*100
sr3


sr4 = (tx4_plt3/tx4_0)*100
sr4

x_axis = [0, 5, 10, 15, 20, 25, 30, 35, 40, 45]
y_axis = np.arange(30, 100.9, 5)
fig, ax = plt.subplots(figsize=(7,7))

ax.errorbar(x_axis, sr1, yerr=tx1_sem2, fmt="-.^", capsize=4, capthick=1, color="m", label="Capomuline") 
ax.errorbar(x_axis, sr2, yerr=tx2_sem2, fmt="-.x", capsize=4, capthick=1, color="b", label="Infubinol")
ax.errorbar(x_axis, sr3, yerr=tx3_sem2, fmt="-.o", capsize=4, capthick=1, color="r", label="Ketapril")
ax.errorbar(x_axis, sr4, yerr=tx4_sem2, fmt="-.D", capsize=4, capthick=1, color="g", label="Placebo")
ax.grid()
ax.set_xlim(0,46,5)
ax.set_ylim(30, 100.9, 10)
ax.set_xlabel("Time (Days)",fontsize=14)
ax.set_ylabel("Survival Rate (%)",fontsize=14)

plt.title("Fig. 3: Survival During Treatment",fontsize=18)
plt.xticks(x_axis)
plt.yticks(y_axis)
plt.legend(loc="best", fontsize="large",fancybox=True)
plt.savefig("3_SR.png")
plt.show()
```


![png](output_6_0.png)



```python
# Creating a bar graph that compares the total % tumor volume change for each drug across the full 45 days.

tx1_plt1 = tx1.groupby("Timepoint")["Tumor Volume (mm3)"].mean()
tx1_plt1

tx2_plt1 = tx2.groupby("Timepoint")["Tumor Volume (mm3)"].mean()
tx2_plt1

tx3_plt1 = tx3.groupby("Timepoint")["Tumor Volume (mm3)"].mean()
tx3_plt1

tx4_plt1 = tx4.groupby("Timepoint")["Tumor Volume (mm3)"].mean()
tx4_plt1

df_bar = pd.concat([tx1_plt1,tx2_plt1,tx3_plt1,tx4_plt1],axis=1)
df_bar.columns = ['Capomuline', 'Infubinol','Ketapril','Placebo']
df_bar = df_bar.reset_index()
df_bar
df_bar2=round(((df_bar.iloc[9,:]-df_bar.iloc[0,:])*100/df_bar.iloc[0,:]),1).to_frame()

df_bar2 = df_bar2.drop(["Timepoint"])
df_bar2 = df_bar2.reset_index()
df_bar2.columns=["Drug","% Tumor Change"]

df_bar2

tick_locations = [0.4,1.4,2.4,3.4]
fig, ax = plt.subplots(figsize=(7,7))

ax.bar(0,df_bar2["% Tumor Change"][0], color="g", align="edge", ec="black", width=1, label="Capomuline") 
ax.bar(1,df_bar2["% Tumor Change"][1], color="r", align="edge", ec="black", width=1, label="Infubinol")
ax.bar(2,df_bar2["% Tumor Change"][2], color="r", align="edge", ec="black", width=1, label="Ketapril")
ax.bar(3,df_bar2["% Tumor Change"][3], color="r", align="edge", ec="black", width=1, label="Placebo")


for i, j in enumerate(df_bar2["% Tumor Change"]):
    ax.annotate(((str(np.round(j, decimals=2))+"%")), xy=(i+0.5,1), ha="center", color="k", fontsize=21)

plt.xticks(tick_locations,df_bar2["Drug"])
plt.xlim(0, 4)
plt.ylim(-22, 60)

# set title and axis labels
plt.title("Fig. 4: Tumor Change Over 45-Day Treatment",fontsize=16)
plt.xlabel("Drug")
plt.ylabel("% Change in Tumor Volume (mm3)")
plt.xlim(0, len(df_bar2))
plt.grid()

plt.savefig("4_Overall_Tumor_Change_Bar.png")
plt.show()
```


![png](output_7_0.png)

