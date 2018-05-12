# Heros_of_Pymoli
Homework 4

Heroes Of Pymoli Data Analysis
Of the 573 active players, the vast majority are male (81%). There also exists, a smaller, but notable proportion of female players (17%).

Our peak age demographic falls between 20-24 (41%) with secondary groups falling between 15-19 (24%) and 10-14 (10%).

Our best selling item was the Final Critic which resulted in a net profit of $38.60

import pandas as pd
import numpy as np

data = "purchase_data.json"
tdata = pd.read_json(data)
tdata.head()

playerc = tdata[["SN"]]
playerrename = playerc.rename(columns={'SN':'Player'})
playertotal = playerrename["Player"].nunique()
playerdata = [{'Total Players': playertotal}]
playerdf = pd.DataFrame(playerdata)
playerdf

items = tdata[["Item Name", "Price"]]
#items = items["Price"].map("${:,.2f}".format)
itemnum = items["Item Name"].value_counts()
itemunique = tdata["Item Name"].nunique()
priceavg = items["Price"].mean()
totalrev = items["Price"].sum()
itemtotal = tdata["Item Name"].count()-1
itemdata = [{'Number of Unique Items': itemunique, 'Average Price': priceavg, 'Number of Purchases': itemtotal, 'Total Revenue': totalrev}]
itemdf = pd.DataFrame(itemdata)
itemdf = itemdf[['Number of Unique Items', 'Average Price', 'Number of Purchases', 'Total Revenue']]
itemdf

dropgen = tdata.drop_duplicates(['SN'])
gendata = dropgen[['Gender','SN']]
gendtotal = gendata["Gender"].count()-1
gendercount = gendata["Gender"].value_counts()
genperc = pd.DataFrame((gendercount/gendtotal)*100)
genperc = genperc.rename(columns={'Gender': 'Percentage of Players'})
genperc = genperc["Percentage of Players"].map("{:,.2f}".format)
#gento = [{'Percentage of Players': (male/gendtotal)*100, 'Total Count': gendercount},{'Percentage of Players': (female/gendtotal)*100, 'Total Count': gendercount}]
#gendf = pd.DataFrame(gento)
totas = pd.DataFrame(gendercount)
totas = totas.rename(columns={'Gender': 'Total Count'})
totalgen = pd.concat([genperc, totas], axis=1)
totalgen

agender = tdata[["Gender", "Price"]]
sumgender = agender.groupby(["Gender"])
purchcnt = sumgender.count()
purchcnt = purchcnt.rename(columns={'Price': 'Purchase Count'})
tgenpurch = sumgender.sum()
tgenpurch = tgenpurch.rename(columns={'Price': 'Total Purchase Value'})
tgenpurch = tgenpurch["Total Purchase Value"].map("${:,.2f}".format)
avgpurch = sumgender.mean()
avgpurch = avgpurch.rename(columns={'Price': 'Average Purchase Price'})
avgpurch = avgpurch["Average Purchase Price"].map("${:,.2f}".format)
totalpg = pd.concat([tgenpurch, avgpurch, purchcnt], axis=1)
totalpg

bindata = dropgen["Age"]
bins = [0,9,15,20,25,30,35,40,100]
demoname = ['<10', '10-14', '15-19', '20-24', '25-29', '30-34', '35-39', '40+']
agetab = pd.cut(dropgen["Age"], bins, labels=demoname)
dropgen["Age Summary"] = agetab
age_sum = dropgen.groupby("Age Summary")
age_count = age_sum['SN'].count()
age_perc = pd.DataFrame((age_count/gendtotal)*100)
age_perc = age_perc.rename(columns={'SN': 'Percentage of Players'})
age_perc = age_perc["Percentage of Players"].map("{:,.2f}".format)
age_df = pd.DataFrame(age_count)
age_df = age_df.rename(columns={'SN': 'Total Count'})
totalage = pd.concat([age_perc, age_df], axis=1)
totalage

tage = pd.cut(tdata["Age"], bins, labels=demoname)
tdata ["Age Sum Total"] = tage
agesu = tdata.groupby("Age Sum Total")
purage = agesu['Price'].count()
purage_df= pd.DataFrame(purage)
purage_df = purage_df.rename(columns={'Price': 'Purchase Count'})
avgprage = agesu['Price'].mean()
avgprage_df = pd.DataFrame(avgprage)
avgprage_df = avgprage_df.rename(columns={'Price': 'Average Purchase Price'})
avgprage_df = avgprage_df["Average Purchase Price"].map("${:,.2f}".format)
totprage = agesu['Price'].sum()
totprage_df = pd.DataFrame(totprage)
totprage_df = totprage_df.rename(columns={'Price': 'Total Purchase Value'})
totprage_df = totprage_df["Total Purchase Value"].map("${:,.2f}".format)
totagep = pd.concat([purage_df, avgprage_df, totprage_df], axis=1)
totagep

spender = tdata[["SN", "Price"]]
grptot = spender.groupby("SN")
grpcnt = grptot.count()
grpsum = grptot.sum()
grpavg = grptot.mean()
grpsumdf = grpsum.nlargest(5,'Price')
grpsumdf

popitem = tdata[["Item Name", "Price"]]
itemp = popitem.groupby("Item Name")
itemc = itemp.count()
item5 = itemc.nlargest(5,'Price')
item5

profit = tdata[["Price", "Item Name"]]
prfts = profit.groupby("Item Name")
prftcnt = prfts.count()
prftsum = prfts.sum()
sprft = prftsum.nlargest(5,"Price")
#cntlrg = prftcnt.nlargest(5,"Price")
sprft
