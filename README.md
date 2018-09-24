
## Weather Observations
* Temperature increases in cities closer to the Equator (latitude 0).

* There doesn't seem to be a strong correlation between latitude with the other variables (cloudiness, humidity, and wind speed).

* One might conclude that wind speed increases the further a city is from the Equator. 


```python
# Dependencies and Setup
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
import requests
import time

# Import API key
import api_keys

# Incorporated citipy to determine city based on latitude and longitude
from citipy import citipy

# # Output File (CSV)
# output_data_file = "output_data/cities.csv"

# Range of latitudes and longitudes
lat_range = (-90, 90)
lng_range = (-180, 180)
```

## Generate Cities List


```python
# List for holding lat_lngs and cities
lat_lngs = []
cities = []
countries = []

# Create a set of random lat and lng combinations
lats = np.random.uniform(low=-90.000, high=90.000, size=1500)
lngs = np.random.uniform(low=-180.000, high=180.000, size=1500)
lat_lngs = zip(lats, lngs)

# Identify nearest city for each lat, lng combination
for lat_lng in lat_lngs:
    city = citipy.nearest_city(lat_lng[0], lat_lng[1]).city_name
    country = citipy.nearest_city(lat_lng[0], lat_lng[1]).country_code

    # If the city is unique, then add it to a our cities list
    if city not in cities:
        cities.append(city)
        countries.append(country)

# Print the city count to confirm sufficient count
print(len(cities))
print(len(countries))
```

    619
    619



```python
# create dataframe
city_df = pd.DataFrame(
    {"City": cities,
     "Country": countries
    })

city_df.head()

# create sample size of 500
city500 = city_df.sample(500)

# reset index
city500 = city500.reset_index(drop=True)
city500.head(10)
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
      <th>City</th>
      <th>Country</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>lisala</td>
      <td>cd</td>
    </tr>
    <tr>
      <th>1</th>
      <td>tiksi</td>
      <td>ru</td>
    </tr>
    <tr>
      <th>2</th>
      <td>iquique</td>
      <td>cl</td>
    </tr>
    <tr>
      <th>3</th>
      <td>quaregnon</td>
      <td>be</td>
    </tr>
    <tr>
      <th>4</th>
      <td>malino</td>
      <td>ru</td>
    </tr>
    <tr>
      <th>5</th>
      <td>flinders</td>
      <td>au</td>
    </tr>
    <tr>
      <th>6</th>
      <td>catuday</td>
      <td>ph</td>
    </tr>
    <tr>
      <th>7</th>
      <td>acari</td>
      <td>pe</td>
    </tr>
    <tr>
      <th>8</th>
      <td>khuzhir</td>
      <td>ru</td>
    </tr>
    <tr>
      <th>9</th>
      <td>kracheh</td>
      <td>kh</td>
    </tr>
  </tbody>
</table>
</div>




```python
# OpenWeatherMap API Key
api_key = api_keys.api_key

# Starting URL for Weather Map API Call
# url = "http://api.openweathermap.org/data/2.5/weather?units=Imperial&APPID=" + api_key 
```

## Perform API Calls


```python

city = []
country = []
lat = []
lon = []
temp = []
humid = []
cloud = []
wind = []
count = 0

for index,row in city500.iterrows():
    query_url = url + "&q=" + row["City"]
    count +=1
    print(f"Processing record {count} for the city: {row['City']}")
    response = requests.get(query_url).json()
    if response["cod"] == "404":
        print("Data not found... skip.")
        
    else:
        print(f"Record found for the city: {row['City']}")
        city.append(response["name"])
        country.append(response["sys"]["country"])
        lat.append(response["coord"]["lat"])
        lon.append(response["coord"]["lon"])
        temp.append(response["main"]["temp"])
        humid.append(response["main"]["humidity"])
        cloud.append(response["clouds"]["all"])
        wind.append(response["wind"]["speed"])
        
```

    Processing record 1 for the city: lisala
    Record found for the city: lisala
    Processing record 2 for the city: tiksi
    Record found for the city: tiksi
    Processing record 3 for the city: iquique
    Record found for the city: iquique
    Processing record 4 for the city: quaregnon
    Record found for the city: quaregnon
    Processing record 5 for the city: malino
    Record found for the city: malino
    Processing record 6 for the city: flinders
    Record found for the city: flinders
    Processing record 7 for the city: catuday
    Record found for the city: catuday
    Processing record 8 for the city: acari
    Record found for the city: acari
    Processing record 9 for the city: khuzhir
    Record found for the city: khuzhir
    Processing record 10 for the city: kracheh
    Data not found... skip.
    Processing record 11 for the city: maniitsoq
    Record found for the city: maniitsoq
    Processing record 12 for the city: salantai
    Record found for the city: salantai
    Processing record 13 for the city: kupang
    Record found for the city: kupang
    Processing record 14 for the city: oranjemund
    Record found for the city: oranjemund
    Processing record 15 for the city: tual
    Record found for the city: tual
    Processing record 16 for the city: palasa
    Data not found... skip.
    Processing record 17 for the city: vagur
    Record found for the city: vagur
    Processing record 18 for the city: longyearbyen
    Record found for the city: longyearbyen
    Processing record 19 for the city: pevek
    Record found for the city: pevek
    Processing record 20 for the city: roald
    Record found for the city: roald
    Processing record 21 for the city: borovoy
    Record found for the city: borovoy
    Processing record 22 for the city: san miguelito
    Record found for the city: san miguelito
    Processing record 23 for the city: provideniya
    Record found for the city: provideniya
    Processing record 24 for the city: vrangel
    Record found for the city: vrangel
    Processing record 25 for the city: cherskiy
    Record found for the city: cherskiy
    Processing record 26 for the city: buta
    Record found for the city: buta
    Processing record 27 for the city: makubetsu
    Record found for the city: makubetsu
    Processing record 28 for the city: khani
    Record found for the city: khani
    Processing record 29 for the city: cacoal
    Record found for the city: cacoal
    Processing record 30 for the city: thompson
    Record found for the city: thompson
    Processing record 31 for the city: coahuayana
    Record found for the city: coahuayana
    Processing record 32 for the city: nouadhibou
    Record found for the city: nouadhibou
    Processing record 33 for the city: keuruu
    Record found for the city: keuruu
    Processing record 34 for the city: gorno-chuyskiy
    Data not found... skip.
    Processing record 35 for the city: aloleng
    Record found for the city: aloleng
    Processing record 36 for the city: barcelona
    Record found for the city: barcelona
    Processing record 37 for the city: oksfjord
    Record found for the city: oksfjord
    Processing record 38 for the city: okandja
    Data not found... skip.
    Processing record 39 for the city: porto novo
    Record found for the city: porto novo
    Processing record 40 for the city: tasiilaq
    Record found for the city: tasiilaq
    Processing record 41 for the city: victoria
    Record found for the city: victoria
    Processing record 42 for the city: sawakin
    Record found for the city: sawakin
    Processing record 43 for the city: yulara
    Record found for the city: yulara
    Processing record 44 for the city: charagua
    Record found for the city: charagua
    Processing record 45 for the city: marystown
    Record found for the city: marystown
    Processing record 46 for the city: deputatskiy
    Record found for the city: deputatskiy
    Processing record 47 for the city: sorong
    Record found for the city: sorong
    Processing record 48 for the city: korla
    Data not found... skip.
    Processing record 49 for the city: fortuna
    Record found for the city: fortuna
    Processing record 50 for the city: cidreira
    Record found for the city: cidreira
    Processing record 51 for the city: utete
    Record found for the city: utete
    Processing record 52 for the city: thunder bay
    Record found for the city: thunder bay
    Processing record 53 for the city: teya
    Record found for the city: teya
    Processing record 54 for the city: byron bay
    Record found for the city: byron bay
    Processing record 55 for the city: jiaozuo
    Record found for the city: jiaozuo
    Processing record 56 for the city: kiunga
    Record found for the city: kiunga
    Processing record 57 for the city: ojinaga
    Record found for the city: ojinaga
    Processing record 58 for the city: port keats
    Record found for the city: port keats
    Processing record 59 for the city: abu samrah
    Record found for the city: abu samrah
    Processing record 60 for the city: tambacounda
    Record found for the city: tambacounda
    Processing record 61 for the city: kavaratti
    Record found for the city: kavaratti
    Processing record 62 for the city: mar del plata
    Record found for the city: mar del plata
    Processing record 63 for the city: ust-kamchatsk
    Data not found... skip.
    Processing record 64 for the city: faya
    Record found for the city: faya
    Processing record 65 for the city: ostrovskoye
    Record found for the city: ostrovskoye
    Processing record 66 for the city: clyde river
    Record found for the city: clyde river
    Processing record 67 for the city: lima
    Record found for the city: lima
    Processing record 68 for the city: santa marinella
    Record found for the city: santa marinella
    Processing record 69 for the city: siguatepeque
    Record found for the city: siguatepeque
    Processing record 70 for the city: abalak
    Record found for the city: abalak
    Processing record 71 for the city: plettenberg bay
    Record found for the city: plettenberg bay
    Processing record 72 for the city: urucara
    Record found for the city: urucara
    Processing record 73 for the city: vao
    Record found for the city: vao
    Processing record 74 for the city: ayan
    Record found for the city: ayan
    Processing record 75 for the city: antalaha
    Record found for the city: antalaha
    Processing record 76 for the city: killybegs
    Record found for the city: killybegs
    Processing record 77 for the city: taolanaro
    Data not found... skip.
    Processing record 78 for the city: namatanai
    Record found for the city: namatanai
    Processing record 79 for the city: novoorsk
    Record found for the city: novoorsk
    Processing record 80 for the city: broken hill
    Record found for the city: broken hill
    Processing record 81 for the city: yellowknife
    Record found for the city: yellowknife
    Processing record 82 for the city: ancud
    Record found for the city: ancud
    Processing record 83 for the city: puerto ayora
    Record found for the city: puerto ayora
    Processing record 84 for the city: seoul
    Record found for the city: seoul
    Processing record 85 for the city: locri
    Record found for the city: locri
    Processing record 86 for the city: daru
    Record found for the city: daru
    Processing record 87 for the city: kailua
    Record found for the city: kailua
    Processing record 88 for the city: viedma
    Record found for the city: viedma
    Processing record 89 for the city: saint-philippe
    Record found for the city: saint-philippe
    Processing record 90 for the city: bluff
    Record found for the city: bluff
    Processing record 91 for the city: belaya gora
    Record found for the city: belaya gora
    Processing record 92 for the city: barrow
    Record found for the city: barrow
    Processing record 93 for the city: riverton
    Record found for the city: riverton
    Processing record 94 for the city: atuona
    Record found for the city: atuona
    Processing record 95 for the city: nuevo progreso
    Record found for the city: nuevo progreso
    Processing record 96 for the city: marcona
    Data not found... skip.
    Processing record 97 for the city: come
    Record found for the city: come
    Processing record 98 for the city: kulhudhuffushi
    Record found for the city: kulhudhuffushi
    Processing record 99 for the city: skibbereen
    Record found for the city: skibbereen
    Processing record 100 for the city: misratah
    Record found for the city: misratah
    Processing record 101 for the city: tarabuco
    Record found for the city: tarabuco
    Processing record 102 for the city: dwarka
    Record found for the city: dwarka
    Processing record 103 for the city: mjolby
    Record found for the city: mjolby
    Processing record 104 for the city: talnakh
    Record found for the city: talnakh
    Processing record 105 for the city: haibowan
    Data not found... skip.
    Processing record 106 for the city: spirit river
    Record found for the city: spirit river
    Processing record 107 for the city: gonbad-e qabus
    Record found for the city: gonbad-e qabus
    Processing record 108 for the city: pochutla
    Record found for the city: pochutla
    Processing record 109 for the city: mount isa
    Record found for the city: mount isa
    Processing record 110 for the city: leningradskiy
    Record found for the city: leningradskiy
    Processing record 111 for the city: erzin
    Record found for the city: erzin
    Processing record 112 for the city: constitucion
    Record found for the city: constitucion
    Processing record 113 for the city: alice springs
    Record found for the city: alice springs
    Processing record 114 for the city: rostock
    Record found for the city: rostock
    Processing record 115 for the city: ribeira grande
    Record found for the city: ribeira grande
    Processing record 116 for the city: ilo
    Record found for the city: ilo
    Processing record 117 for the city: vaitupu
    Data not found... skip.
    Processing record 118 for the city: rawannawi
    Data not found... skip.
    Processing record 119 for the city: amga
    Record found for the city: amga
    Processing record 120 for the city: poum
    Record found for the city: poum
    Processing record 121 for the city: high level
    Record found for the city: high level
    Processing record 122 for the city: cervo
    Record found for the city: cervo
    Processing record 123 for the city: kenora
    Record found for the city: kenora
    Processing record 124 for the city: airai
    Record found for the city: airai
    Processing record 125 for the city: luancheng
    Record found for the city: luancheng
    Processing record 126 for the city: belyy yar
    Record found for the city: belyy yar
    Processing record 127 for the city: kharp
    Record found for the city: kharp
    Processing record 128 for the city: songea
    Record found for the city: songea
    Processing record 129 for the city: pacific grove
    Record found for the city: pacific grove
    Processing record 130 for the city: tigil
    Record found for the city: tigil
    Processing record 131 for the city: kangaatsiaq
    Record found for the city: kangaatsiaq
    Processing record 132 for the city: campbell river
    Record found for the city: campbell river
    Processing record 133 for the city: marrakesh
    Record found for the city: marrakesh
    Processing record 134 for the city: mizpe ramon
    Data not found... skip.
    Processing record 135 for the city: merauke
    Record found for the city: merauke
    Processing record 136 for the city: abha
    Record found for the city: abha
    Processing record 137 for the city: half moon bay
    Record found for the city: half moon bay
    Processing record 138 for the city: tougan
    Record found for the city: tougan
    Processing record 139 for the city: ariquemes
    Record found for the city: ariquemes
    Processing record 140 for the city: arrifes
    Record found for the city: arrifes
    Processing record 141 for the city: vanavara
    Record found for the city: vanavara
    Processing record 142 for the city: meulaboh
    Record found for the city: meulaboh
    Processing record 143 for the city: adelaide
    Record found for the city: adelaide
    Processing record 144 for the city: sheopur
    Record found for the city: sheopur
    Processing record 145 for the city: edgewood
    Record found for the city: edgewood
    Processing record 146 for the city: te anau
    Record found for the city: te anau
    Processing record 147 for the city: svolvaer
    Record found for the city: svolvaer
    Processing record 148 for the city: hecun
    Record found for the city: hecun
    Processing record 149 for the city: port macquarie
    Record found for the city: port macquarie
    Processing record 150 for the city: osoyoos
    Record found for the city: osoyoos
    Processing record 151 for the city: attawapiskat
    Data not found... skip.
    Processing record 152 for the city: aklavik
    Record found for the city: aklavik
    Processing record 153 for the city: qurayyat
    Data not found... skip.
    Processing record 154 for the city: yerbogachen
    Record found for the city: yerbogachen
    Processing record 155 for the city: maun
    Record found for the city: maun
    Processing record 156 for the city: fairbanks
    Record found for the city: fairbanks
    Processing record 157 for the city: mendi
    Record found for the city: mendi
    Processing record 158 for the city: albany
    Record found for the city: albany
    Processing record 159 for the city: iranshahr
    Record found for the city: iranshahr
    Processing record 160 for the city: henties bay
    Record found for the city: henties bay
    Processing record 161 for the city: shebunino
    Record found for the city: shebunino
    Processing record 162 for the city: sohag
    Record found for the city: sohag
    Processing record 163 for the city: almaznyy
    Record found for the city: almaznyy
    Processing record 164 for the city: kolomna
    Record found for the city: kolomna
    Processing record 165 for the city: louisbourg
    Data not found... skip.
    Processing record 166 for the city: santiago
    Record found for the city: santiago
    Processing record 167 for the city: mezen
    Record found for the city: mezen
    Processing record 168 for the city: suhbaatar
    Record found for the city: suhbaatar
    Processing record 169 for the city: narsaq
    Record found for the city: narsaq
    Processing record 170 for the city: portoferraio
    Record found for the city: portoferraio
    Processing record 171 for the city: gold coast
    Record found for the city: gold coast
    Processing record 172 for the city: sterling
    Record found for the city: sterling
    Processing record 173 for the city: atar
    Record found for the city: atar
    Processing record 174 for the city: sungaipenuh
    Record found for the city: sungaipenuh
    Processing record 175 for the city: yuncheng
    Record found for the city: yuncheng
    Processing record 176 for the city: salalah
    Record found for the city: salalah
    Processing record 177 for the city: yar-sale
    Record found for the city: yar-sale
    Processing record 178 for the city: disna
    Data not found... skip.
    Processing record 179 for the city: aykhal
    Record found for the city: aykhal
    Processing record 180 for the city: east london
    Record found for the city: east london
    Processing record 181 for the city: bengkulu
    Data not found... skip.
    Processing record 182 for the city: saleaula
    Data not found... skip.
    Processing record 183 for the city: latacunga
    Record found for the city: latacunga
    Processing record 184 for the city: rantepao
    Record found for the city: rantepao
    Processing record 185 for the city: tessaoua
    Record found for the city: tessaoua
    Processing record 186 for the city: saskylakh
    Record found for the city: saskylakh
    Processing record 187 for the city: vila franca do campo
    Record found for the city: vila franca do campo
    Processing record 188 for the city: kemijarvi
    Data not found... skip.
    Processing record 189 for the city: bambanglipuro
    Record found for the city: bambanglipuro
    Processing record 190 for the city: callaguip
    Record found for the city: callaguip
    Processing record 191 for the city: umm durman
    Data not found... skip.
    Processing record 192 for the city: ust-omchug
    Record found for the city: ust-omchug
    Processing record 193 for the city: pangnirtung
    Record found for the city: pangnirtung
    Processing record 194 for the city: soderhamn
    Record found for the city: soderhamn
    Processing record 195 for the city: chuy
    Record found for the city: chuy
    Processing record 196 for the city: rolla
    Record found for the city: rolla
    Processing record 197 for the city: oistins
    Record found for the city: oistins
    Processing record 198 for the city: nantucket
    Record found for the city: nantucket
    Processing record 199 for the city: mandalgovi
    Record found for the city: mandalgovi
    Processing record 200 for the city: boa vista
    Record found for the city: boa vista
    Processing record 201 for the city: maningrida
    Record found for the city: maningrida
    Processing record 202 for the city: havre-saint-pierre
    Record found for the city: havre-saint-pierre
    Processing record 203 for the city: labytnangi
    Record found for the city: labytnangi
    Processing record 204 for the city: chokurdakh
    Record found for the city: chokurdakh
    Processing record 205 for the city: helena
    Record found for the city: helena
    Processing record 206 for the city: celestun
    Record found for the city: celestun
    Processing record 207 for the city: goderich
    Record found for the city: goderich
    Processing record 208 for the city: hohoe
    Record found for the city: hohoe
    Processing record 209 for the city: khatanga
    Record found for the city: khatanga
    Processing record 210 for the city: iqaluit
    Record found for the city: iqaluit
    Processing record 211 for the city: bandarbeyla
    Record found for the city: bandarbeyla
    Processing record 212 for the city: ostrovnoy
    Record found for the city: ostrovnoy
    Processing record 213 for the city: ampanihy
    Record found for the city: ampanihy
    Processing record 214 for the city: bathsheba
    Record found for the city: bathsheba
    Processing record 215 for the city: tommot
    Record found for the city: tommot
    Processing record 216 for the city: kruisfontein
    Record found for the city: kruisfontein
    Processing record 217 for the city: lebu
    Record found for the city: lebu
    Processing record 218 for the city: chicama
    Record found for the city: chicama
    Processing record 219 for the city: forecariah
    Record found for the city: forecariah
    Processing record 220 for the city: blyth
    Record found for the city: blyth
    Processing record 221 for the city: norman wells
    Record found for the city: norman wells
    Processing record 222 for the city: miri
    Record found for the city: miri
    Processing record 223 for the city: ahipara
    Record found for the city: ahipara
    Processing record 224 for the city: dingle
    Record found for the city: dingle
    Processing record 225 for the city: saint george
    Record found for the city: saint george
    Processing record 226 for the city: georgetown
    Record found for the city: georgetown
    Processing record 227 for the city: lulea
    Record found for the city: lulea
    Processing record 228 for the city: northam
    Record found for the city: northam
    Processing record 229 for the city: lompoc
    Record found for the city: lompoc
    Processing record 230 for the city: castro
    Record found for the city: castro
    Processing record 231 for the city: ajaccio
    Record found for the city: ajaccio
    Processing record 232 for the city: tsihombe
    Data not found... skip.
    Processing record 233 for the city: hailar
    Record found for the city: hailar
    Processing record 234 for the city: illoqqortoormiut
    Data not found... skip.
    Processing record 235 for the city: codrington
    Record found for the city: codrington
    Processing record 236 for the city: shihezi
    Record found for the city: shihezi
    Processing record 237 for the city: lumberton
    Record found for the city: lumberton
    Processing record 238 for the city: shieli
    Record found for the city: shieli
    Processing record 239 for the city: kargil
    Record found for the city: kargil
    Processing record 240 for the city: zhanatas
    Data not found... skip.
    Processing record 241 for the city: sukhoverkovo
    Data not found... skip.
    Processing record 242 for the city: xining
    Record found for the city: xining
    Processing record 243 for the city: dattapur
    Record found for the city: dattapur
    Processing record 244 for the city: simi
    Record found for the city: simi
    Processing record 245 for the city: cabo san lucas
    Record found for the city: cabo san lucas
    Processing record 246 for the city: shirokiy
    Record found for the city: shirokiy
    Processing record 247 for the city: qaqortoq
    Record found for the city: qaqortoq
    Processing record 248 for the city: honiara
    Record found for the city: honiara
    Processing record 249 for the city: westport
    Record found for the city: westport
    Processing record 250 for the city: watertown
    Record found for the city: watertown
    Processing record 251 for the city: manta
    Record found for the city: manta
    Processing record 252 for the city: amderma
    Data not found... skip.
    Processing record 253 for the city: ouesso
    Record found for the city: ouesso
    Processing record 254 for the city: sinegorye
    Record found for the city: sinegorye
    Processing record 255 for the city: sinkat
    Data not found... skip.
    Processing record 256 for the city: seymchan
    Record found for the city: seymchan
    Processing record 257 for the city: darhan
    Record found for the city: darhan
    Processing record 258 for the city: turukhansk
    Record found for the city: turukhansk
    Processing record 259 for the city: mayor pablo lagerenza
    Record found for the city: mayor pablo lagerenza
    Processing record 260 for the city: novyy urengoy
    Record found for the city: novyy urengoy
    Processing record 261 for the city: kenai
    Record found for the city: kenai
    Processing record 262 for the city: sur
    Record found for the city: sur
    Processing record 263 for the city: matamoros
    Record found for the city: matamoros
    Processing record 264 for the city: cassilandia
    Record found for the city: cassilandia
    Processing record 265 for the city: kavieng
    Record found for the city: kavieng
    Processing record 266 for the city: matay
    Record found for the city: matay
    Processing record 267 for the city: samusu
    Data not found... skip.
    Processing record 268 for the city: kununurra
    Record found for the city: kununurra
    Processing record 269 for the city: latung
    Record found for the city: latung
    Processing record 270 for the city: anadyr
    Record found for the city: anadyr
    Processing record 271 for the city: portland
    Record found for the city: portland
    Processing record 272 for the city: tabiauea
    Data not found... skip.
    Processing record 273 for the city: bredasdorp
    Record found for the city: bredasdorp
    Processing record 274 for the city: oroville
    Record found for the city: oroville
    Processing record 275 for the city: astoria
    Record found for the city: astoria
    Processing record 276 for the city: mecca
    Record found for the city: mecca
    Processing record 277 for the city: altea
    Record found for the city: altea
    Processing record 278 for the city: grand river south east
    Data not found... skip.
    Processing record 279 for the city: haines junction
    Record found for the city: haines junction
    Processing record 280 for the city: ilulissat
    Record found for the city: ilulissat
    Processing record 281 for the city: ferzikovo
    Record found for the city: ferzikovo
    Processing record 282 for the city: kodiak
    Record found for the city: kodiak
    Processing record 283 for the city: mahadday weyne
    Data not found... skip.
    Processing record 284 for the city: mantenopolis
    Record found for the city: mantenopolis
    Processing record 285 for the city: qasigiannguit
    Record found for the city: qasigiannguit
    Processing record 286 for the city: naze
    Record found for the city: naze
    Processing record 287 for the city: bur gabo
    Data not found... skip.
    Processing record 288 for the city: black river
    Record found for the city: black river
    Processing record 289 for the city: padang
    Record found for the city: padang
    Processing record 290 for the city: ust-bolsheretsk
    Data not found... skip.
    Processing record 291 for the city: vanimo
    Record found for the city: vanimo
    Processing record 292 for the city: riyadh
    Record found for the city: riyadh
    Processing record 293 for the city: tomaszow lubelski
    Record found for the city: tomaszow lubelski
    Processing record 294 for the city: esil
    Record found for the city: esil
    Processing record 295 for the city: timmins
    Record found for the city: timmins
    Processing record 296 for the city: mrirt
    Data not found... skip.
    Processing record 297 for the city: ous
    Record found for the city: ous
    Processing record 298 for the city: araria
    Record found for the city: araria
    Processing record 299 for the city: kalengwa
    Record found for the city: kalengwa
    Processing record 300 for the city: pinotepa nacional
    Data not found... skip.
    Processing record 301 for the city: yelets
    Record found for the city: yelets
    Processing record 302 for the city: garowe
    Record found for the city: garowe
    Processing record 303 for the city: hutchinson
    Record found for the city: hutchinson
    Processing record 304 for the city: obo
    Record found for the city: obo
    Processing record 305 for the city: pathein
    Record found for the city: pathein
    Processing record 306 for the city: itarema
    Record found for the city: itarema
    Processing record 307 for the city: surt
    Record found for the city: surt
    Processing record 308 for the city: lavrentiya
    Record found for the city: lavrentiya
    Processing record 309 for the city: kamenka
    Record found for the city: kamenka
    Processing record 310 for the city: kochki
    Record found for the city: kochki
    Processing record 311 for the city: aflu
    Data not found... skip.
    Processing record 312 for the city: severobaykalsk
    Record found for the city: severobaykalsk
    Processing record 313 for the city: tabialan
    Data not found... skip.
    Processing record 314 for the city: keti bandar
    Record found for the city: keti bandar
    Processing record 315 for the city: avarua
    Record found for the city: avarua
    Processing record 316 for the city: smithers
    Record found for the city: smithers
    Processing record 317 for the city: lasa
    Record found for the city: lasa
    Processing record 318 for the city: saint-joseph
    Record found for the city: saint-joseph
    Processing record 319 for the city: sakakah
    Data not found... skip.
    Processing record 320 for the city: ulladulla
    Record found for the city: ulladulla
    Processing record 321 for the city: takapau
    Record found for the city: takapau
    Processing record 322 for the city: shelburne
    Record found for the city: shelburne
    Processing record 323 for the city: sarkand
    Record found for the city: sarkand
    Processing record 324 for the city: arica
    Record found for the city: arica
    Processing record 325 for the city: zhezkazgan
    Record found for the city: zhezkazgan
    Processing record 326 for the city: arraial do cabo
    Record found for the city: arraial do cabo
    Processing record 327 for the city: kuytun
    Record found for the city: kuytun
    Processing record 328 for the city: mana
    Record found for the city: mana
    Processing record 329 for the city: guerrero negro
    Record found for the city: guerrero negro
    Processing record 330 for the city: ishinomaki
    Record found for the city: ishinomaki
    Processing record 331 for the city: puerto madero
    Record found for the city: puerto madero
    Processing record 332 for the city: monmouth
    Record found for the city: monmouth
    Processing record 333 for the city: gweta
    Record found for the city: gweta
    Processing record 334 for the city: ocland
    Record found for the city: ocland
    Processing record 335 for the city: sembakung
    Record found for the city: sembakung
    Processing record 336 for the city: namibe
    Record found for the city: namibe
    Processing record 337 for the city: lithgow
    Record found for the city: lithgow
    Processing record 338 for the city: upata
    Record found for the city: upata
    Processing record 339 for the city: terme
    Record found for the city: terme
    Processing record 340 for the city: comodoro rivadavia
    Record found for the city: comodoro rivadavia
    Processing record 341 for the city: imeni poliny osipenko
    Record found for the city: imeni poliny osipenko
    Processing record 342 for the city: douglas
    Record found for the city: douglas
    Processing record 343 for the city: agropoli
    Record found for the city: agropoli
    Processing record 344 for the city: fuerte olimpo
    Record found for the city: fuerte olimpo
    Processing record 345 for the city: altoona
    Record found for the city: altoona
    Processing record 346 for the city: puyang
    Record found for the city: puyang
    Processing record 347 for the city: powell river
    Record found for the city: powell river
    Processing record 348 for the city: hofn
    Record found for the city: hofn
    Processing record 349 for the city: morondava
    Record found for the city: morondava
    Processing record 350 for the city: calama
    Record found for the city: calama
    Processing record 351 for the city: breyten
    Record found for the city: breyten
    Processing record 352 for the city: new norfolk
    Record found for the city: new norfolk
    Processing record 353 for the city: cayenne
    Record found for the city: cayenne
    Processing record 354 for the city: bethel
    Record found for the city: bethel
    Processing record 355 for the city: grindavik
    Record found for the city: grindavik
    Processing record 356 for the city: busselton
    Record found for the city: busselton
    Processing record 357 for the city: dunedin
    Record found for the city: dunedin
    Processing record 358 for the city: los llanos de aridane
    Record found for the city: los llanos de aridane
    Processing record 359 for the city: tonj
    Data not found... skip.
    Processing record 360 for the city: kapaa
    Record found for the city: kapaa
    Processing record 361 for the city: kikwit
    Record found for the city: kikwit
    Processing record 362 for the city: vaini
    Record found for the city: vaini
    Processing record 363 for the city: lodja
    Record found for the city: lodja
    Processing record 364 for the city: aripuana
    Record found for the city: aripuana
    Processing record 365 for the city: ust-nera
    Record found for the city: ust-nera
    Processing record 366 for the city: beringovskiy
    Record found for the city: beringovskiy
    Processing record 367 for the city: priekule
    Record found for the city: priekule
    Processing record 368 for the city: port elizabeth
    Record found for the city: port elizabeth
    Processing record 369 for the city: sao joao da barra
    Record found for the city: sao joao da barra
    Processing record 370 for the city: sitka
    Record found for the city: sitka
    Processing record 371 for the city: nome
    Record found for the city: nome
    Processing record 372 for the city: kahului
    Record found for the city: kahului
    Processing record 373 for the city: sobolevo
    Record found for the city: sobolevo
    Processing record 374 for the city: champerico
    Record found for the city: champerico
    Processing record 375 for the city: saint-francois
    Record found for the city: saint-francois
    Processing record 376 for the city: lagoa
    Record found for the city: lagoa
    Processing record 377 for the city: eyl
    Record found for the city: eyl
    Processing record 378 for the city: bobo dioulasso
    Data not found... skip.
    Processing record 379 for the city: waitati
    Record found for the city: waitati
    Processing record 380 for the city: port hardy
    Record found for the city: port hardy
    Processing record 381 for the city: carnarvon
    Record found for the city: carnarvon
    Processing record 382 for the city: palenque
    Record found for the city: palenque
    Processing record 383 for the city: purwodadi
    Record found for the city: purwodadi
    Processing record 384 for the city: kastamonu
    Record found for the city: kastamonu
    Processing record 385 for the city: coihaique
    Record found for the city: coihaique
    Processing record 386 for the city: ponta do sol
    Record found for the city: ponta do sol
    Processing record 387 for the city: kroya
    Record found for the city: kroya
    Processing record 388 for the city: hay river
    Record found for the city: hay river
    Processing record 389 for the city: ixtapa
    Record found for the city: ixtapa
    Processing record 390 for the city: brae
    Record found for the city: brae
    Processing record 391 for the city: bargal
    Data not found... skip.
    Processing record 392 for the city: port alfred
    Record found for the city: port alfred
    Processing record 393 for the city: hamilton
    Record found for the city: hamilton
    Processing record 394 for the city: potenza
    Record found for the city: potenza
    Processing record 395 for the city: mbaiki
    Record found for the city: mbaiki
    Processing record 396 for the city: muros
    Record found for the city: muros
    Processing record 397 for the city: alofi
    Record found for the city: alofi
    Processing record 398 for the city: komsomolskiy
    Record found for the city: komsomolskiy
    Processing record 399 for the city: lata
    Record found for the city: lata
    Processing record 400 for the city: santa rosalia
    Record found for the city: santa rosalia
    Processing record 401 for the city: brokopondo
    Record found for the city: brokopondo
    Processing record 402 for the city: vredendal
    Record found for the city: vredendal
    Processing record 403 for the city: pala
    Record found for the city: pala
    Processing record 404 for the city: dondo
    Record found for the city: dondo
    Processing record 405 for the city: faanui
    Record found for the city: faanui
    Processing record 406 for the city: kimbe
    Record found for the city: kimbe
    Processing record 407 for the city: edea
    Record found for the city: edea
    Processing record 408 for the city: barra do corda
    Record found for the city: barra do corda
    Processing record 409 for the city: valparaiso
    Record found for the city: valparaiso
    Processing record 410 for the city: angangxi
    Data not found... skip.
    Processing record 411 for the city: hanna
    Record found for the city: hanna
    Processing record 412 for the city: coria
    Record found for the city: coria
    Processing record 413 for the city: turayf
    Record found for the city: turayf
    Processing record 414 for the city: college
    Record found for the city: college
    Processing record 415 for the city: moyale
    Record found for the city: moyale
    Processing record 416 for the city: ushuaia
    Record found for the city: ushuaia
    Processing record 417 for the city: whitehorse
    Record found for the city: whitehorse
    Processing record 418 for the city: grand centre
    Data not found... skip.
    Processing record 419 for the city: bambous virieux
    Record found for the city: bambous virieux
    Processing record 420 for the city: tombouctou
    Record found for the city: tombouctou
    Processing record 421 for the city: luwuk
    Record found for the city: luwuk
    Processing record 422 for the city: hilo
    Record found for the city: hilo
    Processing record 423 for the city: touros
    Record found for the city: touros
    Processing record 424 for the city: san miguel
    Record found for the city: san miguel
    Processing record 425 for the city: safwah
    Data not found... skip.
    Processing record 426 for the city: qaanaaq
    Record found for the city: qaanaaq
    Processing record 427 for the city: pitimbu
    Record found for the city: pitimbu
    Processing record 428 for the city: saint anthony
    Record found for the city: saint anthony
    Processing record 429 for the city: ozgon
    Data not found... skip.
    Processing record 430 for the city: robertsport
    Record found for the city: robertsport
    Processing record 431 for the city: chapais
    Record found for the city: chapais
    Processing record 432 for the city: ust-tsilma
    Record found for the city: ust-tsilma
    Processing record 433 for the city: maceio
    Record found for the city: maceio
    Processing record 434 for the city: lomovka
    Record found for the city: lomovka
    Processing record 435 for the city: cockburn harbour
    Data not found... skip.
    Processing record 436 for the city: mount gambier
    Record found for the city: mount gambier
    Processing record 437 for the city: klaksvik
    Record found for the city: klaksvik
    Processing record 438 for the city: pskov
    Record found for the city: pskov
    Processing record 439 for the city: meyungs
    Data not found... skip.
    Processing record 440 for the city: mahebourg
    Record found for the city: mahebourg
    Processing record 441 for the city: pisco
    Record found for the city: pisco
    Processing record 442 for the city: laguna
    Record found for the city: laguna
    Processing record 443 for the city: viligili
    Data not found... skip.
    Processing record 444 for the city: ngukurr
    Data not found... skip.
    Processing record 445 for the city: halmstad
    Record found for the city: halmstad
    Processing record 446 for the city: tuyen quang
    Record found for the city: tuyen quang
    Processing record 447 for the city: ginir
    Record found for the city: ginir
    Processing record 448 for the city: xuddur
    Record found for the city: xuddur
    Processing record 449 for the city: buala
    Record found for the city: buala
    Processing record 450 for the city: sola
    Record found for the city: sola
    Processing record 451 for the city: kijang
    Record found for the city: kijang
    Processing record 452 for the city: finspang
    Record found for the city: finspang
    Processing record 453 for the city: sorland
    Record found for the city: sorland
    Processing record 454 for the city: miasskoye
    Record found for the city: miasskoye
    Processing record 455 for the city: solnechnyy
    Record found for the city: solnechnyy
    Processing record 456 for the city: sept-iles
    Record found for the city: sept-iles
    Processing record 457 for the city: nokaneng
    Record found for the city: nokaneng
    Processing record 458 for the city: bure
    Record found for the city: bure
    Processing record 459 for the city: souillac
    Record found for the city: souillac
    Processing record 460 for the city: dudinka
    Record found for the city: dudinka
    Processing record 461 for the city: kidal
    Record found for the city: kidal
    Processing record 462 for the city: luderitz
    Record found for the city: luderitz
    Processing record 463 for the city: drumheller
    Record found for the city: drumheller
    Processing record 464 for the city: kamiiso
    Record found for the city: kamiiso
    Processing record 465 for the city: fulitun
    Data not found... skip.
    Processing record 466 for the city: kalmunai
    Record found for the city: kalmunai
    Processing record 467 for the city: dikson
    Record found for the city: dikson
    Processing record 468 for the city: alta floresta
    Record found for the city: alta floresta
    Processing record 469 for the city: vardo
    Record found for the city: vardo
    Processing record 470 for the city: marienburg
    Record found for the city: marienburg
    Processing record 471 for the city: galle
    Record found for the city: galle
    Processing record 472 for the city: severo-kurilsk
    Record found for the city: severo-kurilsk
    Processing record 473 for the city: mataura
    Record found for the city: mataura
    Processing record 474 for the city: kindu
    Record found for the city: kindu
    Processing record 475 for the city: nikolskoye
    Record found for the city: nikolskoye
    Processing record 476 for the city: lahat
    Record found for the city: lahat
    Processing record 477 for the city: wanning
    Record found for the city: wanning
    Processing record 478 for the city: shimoda
    Record found for the city: shimoda
    Processing record 479 for the city: rio gallegos
    Record found for the city: rio gallegos
    Processing record 480 for the city: belushya guba
    Data not found... skip.
    Processing record 481 for the city: maragogi
    Record found for the city: maragogi
    Processing record 482 for the city: ballina
    Record found for the city: ballina
    Processing record 483 for the city: havoysund
    Record found for the city: havoysund
    Processing record 484 for the city: baiyin
    Record found for the city: baiyin
    Processing record 485 for the city: beyneu
    Record found for the city: beyneu
    Processing record 486 for the city: port-gentil
    Record found for the city: port-gentil
    Processing record 487 for the city: saldanha
    Record found for the city: saldanha
    Processing record 488 for the city: durban
    Record found for the city: durban
    Processing record 489 for the city: dubai
    Record found for the city: dubai
    Processing record 490 for the city: cairns
    Record found for the city: cairns
    Processing record 491 for the city: luwingu
    Record found for the city: luwingu
    Processing record 492 for the city: wukari
    Record found for the city: wukari
    Processing record 493 for the city: nanortalik
    Record found for the city: nanortalik
    Processing record 494 for the city: nemuro
    Record found for the city: nemuro
    Processing record 495 for the city: kutum
    Record found for the city: kutum
    Processing record 496 for the city: isangel
    Record found for the city: isangel
    Processing record 497 for the city: muhos
    Record found for the city: muhos
    Processing record 498 for the city: biak
    Record found for the city: biak
    Processing record 499 for the city: requena
    Record found for the city: requena
    Processing record 500 for the city: cordoba
    Record found for the city: cordoba



```python
# Run count to see how many records were found
len(city)
```




    450




```python
# Create dataframe
weather_dict = {
    "City": city,
    "Country": country,
    "Latitude": lat,
    "Longitude": lon,
    "Temperature (F)": temp,
    "Humidity (%)": humid,
    "Cloudiness (%)": cloud,
    "Wind Speed (mph)": wind
}

weather_data = pd.DataFrame(weather_dict)
weather_data.head(10)
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
      <th>City</th>
      <th>Country</th>
      <th>Latitude</th>
      <th>Longitude</th>
      <th>Temperature (F)</th>
      <th>Humidity (%)</th>
      <th>Cloudiness (%)</th>
      <th>Wind Speed (mph)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Lisala</td>
      <td>CD</td>
      <td>2.15</td>
      <td>21.52</td>
      <td>73.82</td>
      <td>94</td>
      <td>80</td>
      <td>2.73</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Tiksi</td>
      <td>RU</td>
      <td>71.64</td>
      <td>128.87</td>
      <td>35.39</td>
      <td>98</td>
      <td>92</td>
      <td>5.75</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Iquique</td>
      <td>CL</td>
      <td>-20.22</td>
      <td>-70.14</td>
      <td>62.60</td>
      <td>77</td>
      <td>75</td>
      <td>9.17</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Quaregnon</td>
      <td>BE</td>
      <td>50.44</td>
      <td>3.86</td>
      <td>45.91</td>
      <td>100</td>
      <td>20</td>
      <td>5.82</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Malino</td>
      <td>RU</td>
      <td>55.11</td>
      <td>38.18</td>
      <td>41.00</td>
      <td>93</td>
      <td>0</td>
      <td>8.95</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Flinders</td>
      <td>AU</td>
      <td>-34.58</td>
      <td>150.85</td>
      <td>57.20</td>
      <td>62</td>
      <td>75</td>
      <td>16.11</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Catuday</td>
      <td>PH</td>
      <td>16.29</td>
      <td>119.81</td>
      <td>84.17</td>
      <td>100</td>
      <td>8</td>
      <td>3.18</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Acari</td>
      <td>BR</td>
      <td>-6.44</td>
      <td>-36.64</td>
      <td>70.58</td>
      <td>85</td>
      <td>12</td>
      <td>9.33</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Khuzhir</td>
      <td>RU</td>
      <td>53.19</td>
      <td>107.35</td>
      <td>53.66</td>
      <td>77</td>
      <td>0</td>
      <td>5.53</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Maniitsoq</td>
      <td>GL</td>
      <td>65.42</td>
      <td>-52.90</td>
      <td>40.43</td>
      <td>78</td>
      <td>20</td>
      <td>21.18</td>
    </tr>
  </tbody>
</table>
</div>




```python
weather_data.to_csv("citiesweather.csv", index=False, header=True)
```


```python
plt.scatter(weather_data["Latitude"],weather_data["Temperature (F)"], marker = "o")

plt.title("City Latitude vs. Max Temperature (F)")
plt.xlabel("Latitude")
plt.ylabel("Temperature (F)")
plt.grid(True)

plt.savefig("LatitudevsTemperature.png")
plt.show
```




    <function matplotlib.pyplot.show(*args, **kw)>




![png](LatitudevsTemperature.png)



```python
plt.scatter(weather_data["Latitude"],weather_data["Humidity (%)"], marker = "o")

plt.title("City Latitude vs. Humidity (%)")
plt.xlabel("Latitude")
plt.ylabel("Humidity (%)")
plt.grid(True)

plt.savefig("LatitudevsHumidity.png")
plt.show
```




    <function matplotlib.pyplot.show(*args, **kw)>




![png](LatitudevsHumidity.png)



```python
plt.scatter(weather_data["Latitude"],weather_data["Cloudiness (%)"], marker = "o")

plt.title("City Latitude vs. Cloudiness (%)")
plt.xlabel("Latitude")
plt.ylabel("Cloudiness (%)")
plt.grid(True)

plt.savefig("LatitudevsCloudiness.png")
plt.show
```




    <function matplotlib.pyplot.show(*args, **kw)>




![png](LatitudevsCloudiness.png)



```python
plt.scatter(weather_data["Latitude"],weather_data["Wind Speed (mph)"], marker = "o")

plt.title("City Latitude vs. Wind Speed (mph)")
plt.xlabel("Latitude")
plt.ylabel("Cloudiness (%)")
plt.grid(True)

plt.savefig("LatitudevsWindspeed.png")
plt.show
```




    <function matplotlib.pyplot.show(*args, **kw)>




![png](LatitudevsWindspeed.png)

