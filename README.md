COVID-19 (*coronavirus* *disease* 2019) adalah penyakit yang disebabkan oleh virus dari golongan *Coronavirus*, yaitu SARS-CoV-2 yang juga sering disebut virus corona. Aksi pemerintah dalam menangani kasus ini salah satunya dengan mengumpulkan dan mengelola data covid-19 yang kemudian dipublikasikan dalam bentuk visualisasi data. Sehingga masyarakat umum dapat memahami informasi perkembangan covid-19 dari hari ke hari dengan mudah. Contohnya saja portal PIKOBAR yang dikelola oleh pemerintah Jawa Barat. 

Pada project ini akan dilakukan eksplorasi data covid-19 yang ada di provinsi Jawa Barat. Ada beberapa tahap yang akan dikerjakan, seperti dibawah ini. 

#### 1. Mengakses Dataset Menggunakan API 
```
import requests
resp_jabar = requests.get('https://data.covid19.go.id/public/api/prov_detail_JAWA_BARAT.json')
print(resp_jabar) #untuk mengetahui status get API
```
![image](https://user-images.githubusercontent.com/50388300/176937126-7c38aba0-98a2-4b10-9edf-7f56431bb695.png)
```
print(resp_jabar.headers)
```
![image](https://user-images.githubusercontent.com/50388300/176936924-f60b51b6-9e82-4d81-b994-7825bd4a1ad3.png)

#### 2. Mengekstrak dan Membersihkan Dataset
```
cov_jabar_raw = resp_jabar.json()
```
```
import pandas as pd
import numpy as np

cov_jabar = pd.DataFrame(cov_jabar_raw['list_perkembangan'])
print(cov_jabar.info())
print(cov_jabar.head())
```
![image](https://user-images.githubusercontent.com/50388300/176936757-a2d544d5-810d-4f9e-a233-dbdec23f28de.png) <br>
![image](https://user-images.githubusercontent.com/50388300/176936799-794d7b50-a339-4033-8d7c-c48e69b7fa2f.png)

```
cov_jabar_tidy = (cov_jabar.drop(columns=[item for item in cov_jabar.columns
                                          if item.startswith('AKUMULASI') 
                                          or item.startswith('DIRAWAT')])
                  .rename(columns=str.lower)
                  .rename(columns={'kasus':'kasus_baru'})
                 )
cov_jabar_tidy['tanggal']=pd.to_datetime(cov_jabar_tidy['tanggal']*1e6,unit='ns')     
print('Lima data teratas\n',cov_jabar_tidy.head())
````
![image](https://user-images.githubusercontent.com/50388300/176936617-5f576b52-dfc6-4598-a60f-831220e03d76.png)

#### 3. Membuat Visualisasi Data
```
import matplotlib.pyplot as plt
import matplotlib.dates as mdates

plt.clf()
fig, ax = plt.subplots(figsize=(10,5))
ax.bar(data=cov_jabar_tidy, x='tanggal', height='sembuh', color='olivedrab')
ax.set_title('Kasus Harian Sembuh dari COVID-19 di Jawa Barat',
             fontsize=22)
ax.set_xlabel('')
ax.set_ylabel('Jumlah kasus')
ax.text(1, -0.3, 'Sumber data: data.covid19.go.id', color='blue',
        ha='right', transform=ax.transAxes)
ax.set_xticklabels(ax.get_xticks(), rotation=45)

ax.xaxis.set_major_locator(mdates.MonthLocator())
ax.xaxis.set_major_formatter(mdates.DateFormatter('%b %Y'))

plt.grid(axis='y')
plt.tight_layout()
plt.show()
```
![image](https://user-images.githubusercontent.com/50388300/176937255-aebe5ed8-efae-4b16-9790-f2e7c3b3cec1.png)


<!-- Exploration and Analysis Data COVID-19 in West Java using Python
1. Understanding the dynamics of Covid-19 cases in West Java using available public data
2. Importing data real-time using API
3. Cleaning and doing some data transformation
4. Creating bar charts and line chats to visualize the data
    
<img src="https://github.com/virarkh/COVID-19/blob/master/IMG/Akumulasi_Aktif.png" width="600"> 
<img src="https://github.com/virarkh/COVID-19/blob/master/IMG/Dinamika_Kasus.png" width="600"> -->
