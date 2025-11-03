# 3. Parameters and Data

The CPOST model features techno-economic details on power generation, transmission, and electricity storage. Parameters and data are calibrated to 2023 based on various sources, including publicly available data, statistical data, commercial databases, and expert consultation.

## 3.1. Generation

CPOST includes both renewable and non-renewable power generation technologies:
- Coal w/o CCS: ultra-supercritical units (USC), supercritical units (SC), and subcritical units (Sub-C);
- Coal w/ CCS: ultra-supercritical units with CCS, supercritical units with CCS;
- Gas w/o CCS: combined cycle gas turbine (CCGT) and open cycle gas turbine (OCGT);
- Gas w/ CCS: CCGT with CCS and OCGT with CCS;
- Biomass w/ CCS;
- Biomass w/o CCS;
- Solar: centralized/distributed photovoltaic (PV) power station and solar thermal power plant (concentrated solar power, CSP);
- Wind: onshore/offshore wind;
- Nuclear.
- Hydro.

China’s electricity demand has grown steadily, reaching 9,224 TWh in 2023—a 6.7% increase from the previous year ([CEC, 2023a](./references.md)). This growth is primarily driven by rising industrial consumption, which accounts for 85.3% of total demand. Meanwhile, the supply side is still dominated by thermal power (mainly coal-fired plants), although significant structural shifts highlight progress toward a cleaner power sector. By the end of 2023, China’s total installed power generation capacity reached 2,920 GW, a 13.9% increase from 2022. Non-fossil energy sources surpassed half the total capacity for the first time ({ref}`Fig. 3-1 <Fig. 3-1>`).

```{figure} _static/fig_3_1.png
---
name: Fig. 3-1
align: center
---
Fig. 3-1: Installed capacity mix by technology in 2023 (unit: GW).
```

Electricity consumption in China’s East, Central, West, and Northeast regions grew by 6.9%, 4.3%, 8.1%, and 5.1%, respectively, compared to the previous year. All provinces in China experienced positive growth in total electricity consumption, with six provinces—Hainan, Xizang, Inner Mongolia, Ningxia, Guangxi, and Qinghai—seeing growth rates exceeding 10% ([CEC, 2023a; CEC, 2024; CETP, 2024](./references.md)). These figures are used to calibrate the model’s start-year parameters.

Regarding cost trajectory, the default baseline scenario assumes a downward trend in the costs of power generation technologies ({ref}`Table 3-1 <Table 3-1>`). The model allows for adjustments to the cost of each specific generation technology in different scenario designs.

```{table} Table 3-1: Assumptions for the declining trend in future capacity costs (unit: US$/kW).
:name: Table 3-1
| Technology         | 2025  | 2030  | 2035  | 2040  | 2045  | 2050  | 2055  | 2060  |
| :----------------- | ----: | ----: | ----: | ----: | ----: | ----: | ----: | ----: |
| Coal w/o CCS       |   631 |   606 |   583 |   563 |   546 |   533 |   523 |   514 |
| Coal w/ CCS        |  1015 |   932 |   860 |   798 |   753 |   719 |   695 |   668 |
| Gas w/o CCS        |   325 |   315 |   306 |   298 |   291 |   286 |   282 |   278 |
| Gas w/ CCS         |   678 |   617 |   564 |   519 |   487 |   463 |   446 |   427 |
| Biomass w/o CCS    |  1290 |  1231 |  1191 |  1150 |  1113 |  1080 |  1053 |  1032 |
| Biomass w/ CCS     |  2321 |  2108 |  1936 |  1784 |  1668 |  1580 |  1515 |  1445 |
| Hydro              |  2168 |  2059 |  1966 |  1873 |  1873 |  1873 |  1873 |  1873 |
| Nuclear            |  2311 |  2242 |  2173 |  2103 |  2034 |  1965 |  1910 |  1865 |
| Solar: CSP         |   493 |   394 |   296 |   272 |   251 |   232 |   217 |   205 |
| Solar: PV          |   393 |   336 |   279 |   256 |   235 |   216 |   201 |   189 |
| Solar: thermal     |  2329 |  1491 |  1400 |  1309 |  1227 |  1154 |  1095 |  1048 |
| Wind: onshore      |   600 |   521 |   489 |   457 |   428 |   402 |   378 |   360 |
| Wind: offshore     |  1383 |  1047 |   808 |   778 |   750 |   726 |   706 |   690 |
| Storage: battery   |  1237 |  1090 |  1002 |   949 |   917 |   898 |   889 |   884 |
| Storage: pumped-hydro | 824 |   798 |   773 |   764 |   757 |   750 |   744 |   740 |
| UHV                |   347 |   329 |   325 |   315 |   306 |   299 |   296 |   295 |
```

## 3.2. Transmission 

The CPOST model divides the subnational power systems into seven grid regions: North, Northeast, East, Central, Northwest, South, and Southwest. The provincial coverage of the above grid boundaries is shown as follows (Hong Kong, Macau, and Taiwan are not included for now):


```{table} Table 3-2: Regional power grid division.
:name: Table 3-2

| Region    | Provinces |
| :-------- | :-------- |
| North     | Beijing; Tianjin; Hebei; Shanxi; Inner Mongolia; Shandong |
| Northeast | Liaoning; Jilin; Heilongjiang |
| East      | Shanghai; Jiangsu; Zhejiang; Anhui; Fujian |
| Central   | Jiangxi; Henan; Hubei; Hunan |
| Northwest | Shaanxi; Gansu; Qinghai; Ningxia; Xinjiang |
| South     | Guangdong; Guangxi; Hainan; Guizhou; Yunnan |
| Southwest | Chongqing; Sichuan; Xizang |
```

```{figure} _static/fig_3_2.png
---
name: Fig. 3-2
align: center
width: 70%
---
Fig. 3-2: Map of regional power grid division.
```

Inter-regional power transmission in China mainly relies on ultra-high voltage (UHV) lines. These UHV lines are modeled simplistically, where transmission capabilities between regions are uniformly expressed in transmission capacity (GW). By the end of 2023, China’s transformer and converter equipment capacity, rated at 220 kV and above, reached approximately 5.42 TW(TVA), a year-on-year increase of 5.7%. Specifically, the capacity of 1000 kV AC transformer equipment was around 213 GVA, while ±800 kV and above DC converter equipment had a capacity of approximately 336 GW ([Wang & Cui, 2024](./references.md)). To date, 39 UHV transmission projects have been completed nationwide (among them 18 inter-regional projects), enabling a cross-provincial transmission capacity exceeding 300 GW and a cumulative power transmission exceeding 3 PWh ([SASAC, 2024; SGCC, 2024](./references.md)). Based on these figures, the model was calibrated to align with the current infrastructure, incorporating the existing list of UHV transmission projects.

```{table} Table 3-3: Current status of inter-regional power transmission (source: compiled by the authors based on public information).
:name: Table 3-3

| Start     | End      | Investment (billion RMB) | Capacity (GW) |
| :-------- | :------- | ----------------------: | ------------: |
| North     | Central  |                     5.7 |             6 |
| North     | East     |                   102.9 |            61 |
| Northwest | East     |                    86.3 |            55 |
| South     | East     |                    45.3 |          13.6 |
| Northwest | Central  |                    90.7 |            40 |
| Southwest | East     |                   115.7 |            24 |
| Southwest | Central  |                    24.4 |             8 |
```

UHV transmission projects’ investment costs primarily include infrastructure investment, lines and towers, and in-station equipment. In the CPOST model, the cost of UHV transmission is simplified by calculating the total cost of the transmission project and dividing it by its capacity. Analysis of 18 existing inter-regional UHV transmission projects indicates that the unit capacity cost ranges from $137 to $556 per kW, typically showing a negative correlation with capacity. In addition, the construction cost of UHV lines is significantly affected by location and geographical environment. For instance, the construction cost in the southwest mountainous area can reach approximately 5 times that in the central and eastern plain areas ({ref}`Fig. 3-3 <Fig. 3-3>`). Future expenditure on transmission capacity expansion can be easily calculated by multiplying the capacity by the corresponding average unit capacity cost specific to the grid region involved.

```{figure} _static/fig_3_3.png
---
name: Fig. 3-3
align: center
---
Fig. 3-3: Cost distribution of UHV transmission projects (source: compiled by the authors based on public news).
```

CPOST can also be used to calculate and analyze scenarios for the future development of power transmission. The future power transmission is expected to continue expanding, as the geographic disparity between China’s clean energy resources and electricity demand necessitates initiatives like ‘West-to-East’ and ‘North-to-South’ power transmission projects to meet the growing demand in central and eastern regions. In CPOST, these changes are calculated with considerations of the announced planning by imposing growth constraints on new transmission capacity in the model. For example, in a baseline scenario ({ref}`Fig. 3-4 <Fig. 3-4>` and {ref}`Fig. 3-5 <Fig. 3-5>`), it is assumed that by the end of the 14th Five-Year Plan, inter-regional power transmission capacity will increase from 220 GW in 2019 to 360 GW by 2025. This increase includes 127 GW from Northwest China, 94 GW from Southwest China and Yunnan, 93 GW from North China, 15 GW from Northeast China, and 31 GW from other transmission lines. By 2035, total capacity is projected to reach 488 GW; by 2050, it is expected to expand to 631 GW ([GEIDCO, 2020](./references.md)). 

```{figure} _static/fig_3_4.png
---
name: Fig. 3-4
align: center
width: 70%
---
Fig. 3-4: Inter-regional power transmission in 2025.
```

```{figure} _static/fig_3_5.png
---
name: Fig. 3-5
align: center
---
Fig. 3-5: Inter-regional power transmission in 2035 and 2050.
```

## 3.3. Storage

The storage technologies modeled in CPOST include energy storage power stations and pumped-storage hydroelectricity (PSH). The installations are calibrated based on historical data. The Global Hydropower Tracker by Global Energy Monitor provides information on PSH station planning in provinces of China, encompassing announced, planned, and under-construction projects ([GEM, 2024](./references.md)). By 2060, the total installed capacity is projected to increase by 291.4 GW compared to 2030, reaching a cumulative capacity of 437.2 GW. The model is adjusted according to the specific circumstances of each province.

```{figure} _static/fig_3_6.png
---
name: Fig. 3-6
align: center
---
Fig. 3-6: Pumped storage capacity in provinces (2020).
```

Regarding costs, the model refers to the data from the China Electricity Council and calibrated with other sources ([Dianchacha, 2024; CEC, 2023b; CEC, 2023c](./references.md)). In 2020, the average capital cost of PSH stations (5 hours) was approximately $1422/kW. For chemical energy storage power stations (2 hours), the capital cost is around $950/kW. The possible declining ratios of these costs in the future are taken from NREL and other literature, and then the costs in 2020 are used as a benchmark to derive future costs ([Cole & Karmakar, 2023](./references.md)).
