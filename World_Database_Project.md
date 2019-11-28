# MYSQL PRACTICE

### 1) Tampilkan daftar 10 kota terpadat di Indonesia. Urutkan data dari kota dengan populasi terbanyak.
Kolom yang diwajibkan tampil adalah id kota, nama kota, kode negara, distrik/provinsi dan populasi. 

```
SELECT * FROM City
WHERE CountryCode = 'IDN'
limit 10;
```

```
+-----+----------------+-------------+------------------+------------+
| ID  | Name           | CountryCode | District         | Population |
+-----+----------------+-------------+------------------+------------+
| 939 | Jakarta        | IDN         | Jakarta Raya     |    9604900 |
| 940 | Surabaya       | IDN         | East Java        |    2663820 |
| 941 | Bandung        | IDN         | West Java        |    2429000 |
| 942 | Medan          | IDN         | Sumatera Utara   |    1843919 |
| 943 | Palembang      | IDN         | Sumatera Selatan |    1222764 |
| 944 | Tangerang      | IDN         | West Java        |    1198300 |
| 945 | Semarang       | IDN         | Central Java     |    1104405 |
| 946 | Ujung Pandang  | IDN         | Sulawesi Selatan |    1060257 |
| 947 | Malang         | IDN         | East Java        |     716862 |
| 948 | Bandar Lampung | IDN         | Lampung          |     680332 |
+-----+----------------+-------------+------------------+------------+
```

### 2) Tampilkan daftar 10 kota terpadat di dunia beserta asal negaranya. Urutkan data dari kota dengan populasi terbanyak. 
Kolom yang diwajibkan ada minimal adalah id kota, nama kota, distrik/provinsi, nama negara dan populasi. 

```
SELECT a.id, a.name as nama_kota, a.district,
b.name as negara, a.population
FROM city as a, country as b
WHERE a.countrycode = b.code
order by population desc
limit 10;
```

```
+------+------------------+------------------+--------------------+------------+
| id   | nama_kota        | district         | negara             | population |
+------+------------------+------------------+--------------------+------------+
| 1024 | Mumbai (Bombay)  | Maharashtra      | India              |   10500000 |
| 2331 | Seoul            | Seoul            | South Korea        |    9981619 |
|  206 | São Paulo        | São Paulo        | Brazil             |    9968485 |
| 1890 | Shanghai         | Shanghai         | China              |    9696300 |
|  939 | Jakarta          | Jakarta Raya     | Indonesia          |    9604900 |
| 2822 | Karachi          | Sindh            | Pakistan           |    9269265 |
| 3357 | Istanbul         | Istanbul         | Turkey             |    8787958 |
| 2515 | Ciudad de México | Distrito Federal | Mexico             |    8591309 |
| 3580 | Moscow           | Moscow (City)    | Russian Federation |    8389200 |
| 3793 | New York         | New York         | United States      |    8008278 |
+------+------------------+------------------+--------------------+------------+
```

### 3) Tampilkan daftar 10 negara yang tercatat merdeka paling awal. Daftar negara yang tidak diketahui tahun kemerdekaannya, tidak perlu diikutsertakan. 
Kolom yang diwajibkan ada minimal adalah kode negara, nama negara, benua, regional dan tahun merdeka (Independence Year).

```
SELECT code, name, continent, region, indepyear as tahun_merdeka
FROM country
WHERE indepyear != 0
ORDER BY indepyear asc
LIMIT 10;
```

```
+------+----------------+-----------+------------------+---------------+
| code | name           | continent | region           | tahun_merdeka |
+------+----------------+-----------+------------------+---------------+
| CHN  | China          | Asia      | Eastern Asia     |         -1523 |
| ETH  | Ethiopia       | Africa    | Eastern Africa   |         -1000 |
| JPN  | Japan          | Asia      | Eastern Asia     |          -660 |
| DNK  | Denmark        | Europe    | Nordic Countries |           800 |
| SWE  | Sweden         | Europe    | Nordic Countries |           836 |
| FRA  | France         | Europe    | Western Europe   |           843 |
| SMR  | San Marino     | Europe    | Southern Europe  |           885 |
| GBR  | United Kingdom | Europe    | British Islands  |          1066 |
| PRT  | Portugal       | Europe    | Southern Europe  |          1143 |
| AND  | Andorra        | Europe    | Southern Europe  |          1278 |
+------+----------------+-----------+------------------+---------------+
```

### 4) Tampilkan daftar benua yang memiliki lebih dari 10 negara di dalamnya. 
Kolom yang ditampilkan minimal: nama benua, jumlah negara di dalam benua, total populasi dan rata-rata angka harapan hidup (Life Expectancy) 
kemudian urutkan dari benua yang memiliki populasi terbanyak.

```
SELECT continent as Benua, count(name) as Jumlah_Negara,
sum(population) as Populasi, avg(lifeExpectancy) as Rata_AngkaHrpnHdp
FROM country
GROUP BY Benua
HAVING Jumlah_Negara > 10
ORDER BY Populasi desc;
```

```
+---------------+---------------+------------+-------------------+
| Benua         | Jumlah_Negara | Populasi   | Rata_AngkaHrpnHdp |
+---------------+---------------+------------+-------------------+
| Asia          |            51 | 3705025700 |          67.44118 |
| Africa        |            58 |  784475000 |          52.57193 |
| Europe        |            46 |  730074600 |          75.14773 |
| North America |            37 |  482993000 |          72.99189 |
| South America |            14 |  345780000 |          70.94615 |
| Oceania       |            28 |   30401150 |          69.71500 |
+---------------+---------------+------------+-------------------+
```

### 5) Tampilkan daftar negara-negara Asia yang memiliki angka harapan hidup lebih dari rata-rata angka harapan hidup negara-negara Eropa. 
Kolom yang diwajibkan ada yaitu nama negara, nama benua, angka harapan hidup dan Pendapatan Nasional Bruto/GNP (Gross National Product). Urutkan data dari negara Asia dengan angka harapan hidup tertinggi. 

```
SELECT name as Nama, continent as Benua,
lifeExpectancy as AngkaHarapanHidup, GNP
FROM country
WHERE continent = 'Asia' and
LifeExpectancy >
(select avg(lifeExpectancy) from country
where continent = 'Europe')
ORDER BY AngkaHarapanHidup desc;
```

```
+-----------+-------+-------------------+------------+
| Nama      | Benua | AngkaHarapanHidup | GNP        |
+-----------+-------+-------------------+------------+
| Macao     | Asia  |              81.6 |    5749.00 |
| Japan     | Asia  |              80.7 | 3787042.00 |
| Singapore | Asia  |              80.1 |   86503.00 |
| Hong Kong | Asia  |              79.5 |  166448.00 |
| Israel    | Asia  |              78.6 |   97477.00 |
| Jordan    | Asia  |              77.4 |    7526.00 |
| Cyprus    | Asia  |              76.7 |    9333.00 |
| Taiwan    | Asia  |              76.4 |  256254.00 |
| Kuwait    | Asia  |              76.1 |   27037.00 |
+-----------+-------+-------------------+------------+
```

### 6) Tampilkan daftar 10 negara yang bahasa resminya (official language) adalah bahasa Inggris, dan memiliki persentase pengguna bahasa Inggris tertinggi di dunia. 
Kolom yang diwajibkan ada yaitu kode negara, nama negara, bahasa, kolom isOfficial dan percentage. Urutkan dari persentase pengguna bahasa Inggris tertinggi. 

```
SELECT a.countrycode, b.name, a.language, a.isOfficial, a.percentage
FROM countrylanguage as a, country as b
WHERE a.countrycode = b.code and
language = 'English' and isOfficial = 'T'
ORDER BY percentage desc
LIMIT 10;
```

```
+-------------+----------------------+----------+------------+------------+
| countrycode | name                 | language | isOfficial | percentage |
+-------------+----------------------+----------+------------+------------+
| BMU         | Bermuda              | English  | T          |      100.0 |
| IRL         | Ireland              | English  | T          |       98.4 |
| GBR         | United Kingdom       | English  | T          |       97.3 |
| GIB         | Gibraltar            | English  | T          |       88.9 |
| NZL         | New Zealand          | English  | T          |       87.0 |
| USA         | United States        | English  | T          |       86.2 |
| VIR         | Virgin Islands, U.S. | English  | T          |       81.7 |
| AUS         | Australia            | English  | T          |       81.2 |
| CAN         | Canada               | English  | T          |       60.4 |
| BLZ         | Belize               | English  | T          |       50.8 |
+-------------+----------------------+----------+------------+------------+
```

### 7) Tampilkan daftar negara ASEAN beserta populasi negaranya, Pendapatan Nasional Bruto/GNP (Gross National Product), ibukota & populasi ibukota.
Kolom yang diwajibkan ada yaitu nama negara, populasi negara, pendapatan nasional 
bruto (GNP), nama ibukota dan populasi ibukota. Urutkan berdasarkan abjad nama negara. 

```
SELECT a.name as Negara_ASEAN, a.Population as Populasi_Negara,a.GNP,
b.name as Ibukota, b.population as Populasi_Ibukota
FROM country as a, city as b
WHERE b.countrycode = a.code and
a.region = 'southeast asia' and
b.name in('bandar seri begawan', 'Phnom Penh', 
'Dili', 'Jakarta', 'Vientiane', 'Kuala Lumpur',
'Rangoon (Yangon)', 'Manila', 'Singapore',
'Bangkok', 'Hanoi')
Order by a.name asc;
```

```
+--------------+-----------------+-----------+---------------------+------------------+
| Negara_ASEAN | Populasi_Negara | GNP       | Ibukota             | Populasi_Ibukota |
+--------------+-----------------+-----------+---------------------+------------------+
| Brunei       |          328000 |  11705.00 | Bandar Seri Begawan |            21484 |
| Cambodia     |        11168000 |   5121.00 | Phnom Penh          |           570155 |
| East Timor   |          885000 |      0.00 | Dili                |            47900 |
| Indonesia    |       212107000 |  84982.00 | Jakarta             |          9604900 |
| Laos         |         5433000 |   1292.00 | Vientiane           |           531800 |
| Malaysia     |        22244000 |  69213.00 | Kuala Lumpur        |          1297526 |
| Myanmar      |        45611000 | 180375.00 | Rangoon (Yangon)    |          3361700 |
| Philippines  |        75967000 |  65107.00 | Manila              |          1581082 |
| Singapore    |         3567000 |  86503.00 | Singapore           |          4017733 |
| Thailand     |        61399000 | 116416.00 | Bangkok             |          6320174 |
| Vietnam      |        79832000 |  21929.00 | Hanoi               |          1410000 |
+--------------+-----------------+-----------+---------------------+------------------+
```

### 8) Tampilkan daftar negara G20 beserta populasi negaranya, Pendapatan Nasional Bruto/GNP (Gross National Product), ibukota & populasi ibukota.
Kolom yang diwajibkan ada yaitu nama negara, populasi negara, pendapatan nasional bruto (GNP), nama ibukota dan populasi ibukota. Urutkan berdasarkan abjad nama negara.

```
SELECT a.name as Negara_G20, a.Population as Populasi_Negara, a.GNP,
b.name as Ibukota, b.population as Populasi_Ibukota
FROM country as a, city as b
WHERE b.countrycode = a.code and
b.name in('Buenos Aires','Canberra','BrasÃ­lia','Ottawa','Peking','Paris','Berlin', 'New Delhi',
'Jakarta','Tokyo','Ciudad de MÃ©xico','Moscow','Riyadh','Pretoria','Seoul','Ankara','London','Washington')
order by a.name asc;
```

```
+--------------------+-----------------+------------+--------------+------------------+
| Negara_G20         | Populasi_Negara | GNP        | Ibukota      | Populasi_Ibukota |
+--------------------+-----------------+------------+--------------+------------------+
| Argentina          |        37032000 |  340238.00 | Buenos Aires |          2982146 |
| Australia          |        18886000 |  351182.00 | Canberra     |           322723 |
| Canada             |        31147000 |  598862.00 | London       |           339917 |
| Canada             |        31147000 |  598862.00 | Ottawa       |           335277 |
| China              |      1277558000 |  982268.00 | Peking       |          7472000 |
| France             |        59225700 | 1424285.00 | Paris        |          2125246 |
| Germany            |        82164700 | 2133367.00 | Berlin       |          3386667 |
| India              |      1013662000 |  447114.00 | New Delhi    |           301297 |
| Indonesia          |       212107000 |   84982.00 | Jakarta      |          9604900 |
| Japan              |       126714000 | 3787042.00 | Tokyo        |          7980230 |
| Russian Federation |       146934000 |  276608.00 | Moscow       |          8389200 |
| Saudi Arabia       |        21607000 |  137635.00 | Riyadh       |          3324000 |
| South Africa       |        40377000 |  116729.00 | Pretoria     |           658630 |
| South Korea        |        46844000 |  320749.00 | Seoul        |          9981619 |
| Turkey             |        66591000 |  210721.00 | Ankara       |          3038159 |
| United Kingdom     |        59623400 | 1378330.00 | London       |          7285000 |
| United States      |       278357000 | 8510700.00 | Washington   |           572059 |
+--------------------+-----------------+------------+--------------+------------------+
```