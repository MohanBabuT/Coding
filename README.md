import pandas as pd
from pathlib import Path
import os

#Getting the file path and extension
CSV_Folder_Path=r"C:\Users\Input Folder"
Output_Folder_Path=r"C:\Users\\Output Folder"
File_Extension="*.csv"

#Combined path of files
Combined_Files_Path = sorted(Path(CSV_Folder_Path).glob(File_Extension))

#List to store the data temporarly
Combined_Data = []

#For each file extracting the file name excluding the extension
#and adding the file name as new column in the dataframe 
for Each_File in Combined_Files_Path:
    Reading_Curr_File = pd.read_csv(Each_File)
    FileName=Each_File.name
    Reading_Curr_File['Source Name'] = FileName.split(".")[0]
    Combined_Data.append(Reading_Curr_File)
    
Final_Combined_Data = pd.concat(Combined_Data)

#Applying few transformation like splitting columns, getting unique values
Final_Combined_Data['Year']=Final_Combined_Data['Date'].str.split("-").str[0]
Unique_Years_in_Final_Combined_Data=sorted(Final_Combined_Data['Year'].unique())

for Each_Year in Unique_Years_in_Final_Combined_Data:
    
    #Creating file name and folder in the output path
    Output_FileName=Output_Folder_Path+"/"+Each_Year+"/"+Each_Year+".xlsx"
    os.makedirs(os.path.join(Output_Folder_Path,Each_Year), exist_ok = True)
    
    #Exporting the filtered data to excel file in the output  path
    Temp_Store=Final_Combined_Data.loc[Final_Combined_Data['Year']==Each_Year]
    Temp_Store.to_excel(os.path.join(Output_Folder_Path,Output_FileName),sheet_name=Each_Year, index = False, header=True)

print("Task Completed Successfully!")
