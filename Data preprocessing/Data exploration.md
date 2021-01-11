# Data exploration

De data die we ontvangen hebben van onze opdrachtgever was in de vorm van Excel. Voor ieder huis hebben we een apart excel bestand gekregen, met hierin verschillende features van de energieopslag. 

Na het ontvangen van de data konden we er niet veel mee, aangezien het via Excel is. De excel bestanden hebben verschillende sheets met allerlij componenten die gebruikt kunnnen worden. Om daadwerkelijk iets te kunnen doen met de data, is het van belang om de data omtezetten naar numpy. 

Dit heb ik als volgt gedaan.

Eerst ben ik begonnen met het importeren van de juiste imports
 ```python
 import pandas as pd
import numpy as np
import glob
import multiprocessing as mp
from tqdm import tqdm
```
Vervolgens heb ik gekozen waar de data moet worden opgeslagen
```python
load_path = '/home/18097367/notebooks/zero/Data:/Excel: 2020'
save_path = '/home/18097367/notebooks/zero/Data:/2020_Numpy/'

paths = glob.glob(load_path+"/*.xlsx")
```
De volgende functies heb ik hiervoor gebruikt
```python
def load_house(sheet, house):
    """Load the file and save the file."""
    dff = pd.read_excel(load_path + str(house) + '.xlsx', sheet_name=sheet, engine="openpyxl")
    np.save(save_path + sheet + '_' + str(house), dff.values)
    
def nParse3(n):
    """output a string that is 3 decimals long."""
    number = str(n)
    if len(number) == 1:
        number = "00" + number
    elif len(number) == 2:
        number = "0" + number
    elif len(number) == 3:
        number = number
    return str(number)

def get_sheet_names(i):
    """Get the sheet names from the excel file."""
    return pd.ExcelFile(load_path + str(i) + '.xlsx',engine="openpyxl").sheet_names
    
    for path in tqdm(paths[0:120]):
    house_number = path[-8:-5]
    df = pd.read_excel(path, engine="openpyxl", sheet_name=None)
    sheets_names = list(df.keys())
    
    for sheet in sheets_names:
        output_file_name = sheet + "_" + house_number
        np.save(save_path+output_file_name, df[sheet].to_numpy())
        
        print("House {} has {} sheets.".format(house_number, len(sheets_names)))
```
Dit is een functie die ik heb mogen gebruiken van een projectlid van mij. 
