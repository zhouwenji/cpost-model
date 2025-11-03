# 4. Demand Estimation

Electricity demand for each province is estimated exogenously to the model, mainly based on projections on driving factors such as GDP, population, industrial structure, etc. China’s total electricity consumption in 2023 reached 9.22 PWh, with a per capita consumption of 6.54 MWh, an increase of 422 kWh per person compared to the previous year ([CEC, 2023b](./references.md)). Future pathways are based on the experiences of representative countries, such as the United States, EU countries, and Japan. Analysis of historical data in these countries reveals that total electricity consumption stabilizes after reaching a certain level, with an annual growth rate below 5% ([EMBER, 2024b](./references.md)). Additionally, per capita electricity consumption tends to rise initially as GDP increases, but eventually peaks and declines.

```{figure} _static/fig_4_1.png
---
name: Fig. 4-1
align: center
width: 70%
---
Fig. 4-1: Electricity consumption and year-on-year growth rate.
```

```{figure} _static/fig_4_2.png
---
name: Fig. 4-2
align: center
width: 70%
---
Fig. 4-2: Electricity consumption and GDP per capita in representative countries.
```

Considering China still faces rapid electricity demand growth, we assume that future per capita electricity consumption in China will gradually increase from the current level to a saturation level of approximately 12 to 15 MWh. Under the high-demand scenario, the country’s total electricity consumption is expected to grow moderately during the 14th FYP (2021–2025), reaching 13.5 PWh by 2030, with an average annual growth rate of about 5.3%; during the 15th FYP (2026–2030), the growth rate is expected to slow to around 2.3% annually, with total electricity consumption reaching 15.1 PWh by 2035. Under the low-demand scenario, electricity consumption is expected to reach 11.9 TWh by 2030 and 13.1 TWh by 2035. These scenarios are exogenously input into the model to generate the total electricity generation required to meet the supply-demand balance.