# Data exploration

De data die we ontvangen hebben van onze opdrachtgever was in de vorm van Excel. Voor ieder huis hebben we een apart excel bestand gekregen, met hierin verschillende features van de energieopslag. 

Na het ontvangen van de data konden we er niet veel mee, aangezien het via Excel is. De excel bestanden hebben verschillende sheets met allerlij componenten die gebruikt kunnnen worden. Om daadwerkelijk iets te kunnen doen met de data, is het van belang om de data omtezetten naar numpy. 

Dit heb ik gedaan met de volgende code:
 ```python
 import pandas as pd
import numpy as np
import glob
import multiprocessing as mp
from tqdm import tqdm
```
