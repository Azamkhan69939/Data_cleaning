# Data_cleaning
cleaned data using pandas, using mean imputation, random imputation and used values of the column to predict the data in the missing columns
import pandas as pd
import numpy as np
df=pd.read_csv("dirty_cafe_sales.csv")
df
new_df=df.drop_duplicates(subset=None,keep="first")
df.loc[df["Price Per Unit"]=="3.0","Item"].unique()
df.Item.unique()
df.loc[df["Item"].isin(["Coffee","Cake","Cookie",'Juice','Sandwich','Smoothie','Salad']),["Price Per Unit","Item"]]
price_mapping={'2.0':"Coffee"
               , '1.0':"Cookie"
               , '5.0':"Salad"
               , '4.0':"Smoothie"
               , '1.5':"Tea"
               }
df.loc[df["Item"].isin(["UNKNOWN","ERROR",np.nan]),"Item"]=df["Price Per Unit"].map(price_mapping)
df.loc[df["Price Per Unit"].isin(["UNKNOWN","ERROR",np.nan]),"Price Per Unit"]=df["Item"].map(price_mapping)
df["Item"]=df["Item"].apply(lambda x: np.random.choice(['Juice','Cake']) if x in [np.nan,"UNKNOWN","ERROR"] else x)
df["Price Per Unit"]=df["Price Per Unit"].apply(lambda x: "3.0" if x in [np.nan,"UNKNOWN","ERROR"] else x)
df.head(10)
df["Item"]=df["Item"].astype(str)
df["Price Per Unit"].unique()
df["Quantity"]=df["Quantity"].replace(["UNKNOWN","ERROR"],"0")
df["Quantity"]=df["Quantity"].astype(float)
df["Quantity"]=df["Quantity"].replace(np.nan,round(df["Quantity"].mean()))
df["Quantity"]=df["Quantity"].replace(0,round(df["Quantity"].mean()))
df["Quantity"]=df["Quantity"].astype(int)
df["Price Per Unit"]=df["Price Per Unit"].astype(float)
df.drop(columns="Total Spent")
df["Total Spent"]=df["Price Per Unit"]*df["Quantity"]
df["Transaction Date"].ffill(inplace=True)
date=df["Transaction Date"].unique()
date=list(date)
len(date)
date.remove("ERROR")
date.remove("UNKNOWN")
df["Transaction Date"].replace(["UNKNOWN","ERROR"],np.random.choice(date),inplace=True)
df["Transaction Date"]=df["Transaction Date"].astype("date32[pyarrow]")
df["Payment Method"]=df["Payment Method"].fillna(df["Payment Method"].mode()[0])
df["Payment Method"]=df["Payment Method"].replace(["UNKNOWN","ERROR"],df["Payment Method"].mode()[0])
df=df.drop(columns="Location")
