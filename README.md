# Carbon-Emission-Analysis

![image](https://github.com/user-attachments/assets/161dd6fb-0f74-4922-b742-0690dec1389c)

This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.

Carbon emissions play a crucial role in the environment, accounting for over 75% of global emissions and posing a significant environmental challenge. These emissions contribute to the accumulation of greenhouse gases in the atmosphere, leading to climate change, planetary warming, and involvement in various environmental disasters.

Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development.

## Data Source

Our dataset is compiled from publicly available data from nature.com and encompasses the product carbon footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO2 (carbon dioxide equivalent).

## Data Structure

The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.
![image](https://github.com/user-attachments/assets/35267fd4-4c2b-4c92-b53a-bd8bf0fa72e7)

## Data Exploring
### Table 'product_emissions'
```sql
SELECT * FROM product_emissions
LIMIT 5;
```

| id           | company_id | country_id | industry_group_id | year | product_name                                                    | weight_kg | carbon_footprint_pcf | upstream_percent_total_pcf | operations_percent_total_pcf | downstream_percent_total_pcf | 
| -----------: | ---------: | ---------: | ----------------: | ---: | --------------------------------------------------------------: | --------: | -------------------: | -------------------------: | ---------------------------: | ---------------------------: | 
| 10056-1-2014 | 82         | 28         | 2                 | 2014 | Frosted Flakes(R) Cereal                                        | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10056-1-2015 | 82         | 28         | 15                | 2015 | "Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)" | 0.7485    | 2                    | 57.50                      | 30.00                        | 12.50                        | 
| 10222-1-2013 | 83         | 28         | 8                 | 2013 | Office Chair                                                    | 20.68     | 73                   | 80.63                      | 17.36                        | 2.01                         | 
| 10261-1-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1488                 | 30.65                      | 5.51                         | 63.84                        | 
| 10261-2-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1818                 | 25.08                      | 4.51                         | 70.41                        | 

### Table 'industry_groups'
```sql
SELECT * FROM industry_groups
LIMIT 5;
```

| id | industry_group                                                         | 
| -: | ---------------------------------------------------------------------: | 
| 1  | "Consumer Durables, Household and Personal Products"                   | 
| 2  | "Food, Beverage & Tobacco"                                             | 
| 3  | "Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber" | 
| 4  | "Mining - Iron, Aluminum, Other Metals"                                | 
| 5  | "Pharmaceuticals, Biotechnology & Life Sciences"                       | 

### Table 'companies'
```sql
SELECT * FROM companies
LIMIT 5;
```

| id | company_name                  | 
| -: | ----------------------------: | 
| 1  | "Autodesk, Inc."              | 
| 2  | "Casio Computer Co., Ltd."    | 
| 3  | "Cisco Systems, Inc."         | 
| 4  | "CNX Coal Resources, LP"      | 
| 5  | "Coca-Cola Enterprises, Inc." | 

### Table 'countries'
```sql
SELECT * FROM countries
LIMIT 5;
```

| id | country_name | 
| -: | -----------: | 
| 1  | Australia    | 
| 2  | Belgium      | 
| 3  | Brazil       | 
| 4  | Canada       | 
| 5  | Chile        | 
### Find Duplicates
```sql
SELECT *, COUNT(*) AS count_duplicate
FROM product_emissions
GROUP BY
	id,
	company_id,
	country_id,
	industry_group_id,
	year,
	product_name,
	weight_kg,
	carbon_footprint_pcf,
	upstream_percent_total_pcf,
	operations_percent_total_pcf,
	downstream_percent_total_pcf
HAVING COUNT(*) > 1
LIMIT 10;
```

| id           | company_id | country_id | industry_group_id | year | product_name                                                    | weight_kg | carbon_footprint_pcf | upstream_percent_total_pcf                       | operations_percent_total_pcf                     | downstream_percent_total_pcf                     | count_duplicate | 
| -----------: | ---------: | ---------: | ----------------: | ---: | --------------------------------------------------------------: | --------: | -------------------: | -----------------------------------------------: | -----------------------------------------------: | -----------------------------------------------: | --------------: | 
| 10056-1-2014 | 82         | 28         | 2                 | 2014 | Frosted Flakes(R) Cereal                                        | 0.7485    | 2                    | 57.50                                            | 30.00                                            | 12.50                                            | 2               | 
| 10056-1-2015 | 82         | 28         | 15                | 2015 | "Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)" | 0.7485    | 2                    | 57.50                                            | 30.00                                            | 12.50                                            | 2               | 
| 10222-1-2013 | 83         | 28         | 8                 | 2013 | Office Chair                                                    | 20.68     | 73                   | 80.63                                            | 17.36                                            | 2.01                                             | 2               | 
| 10261-1-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1488                 | 30.65                                            | 5.51                                             | 63.84                                            | 2               | 
| 10261-2-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 1818                 | 25.08                                            | 4.51                                             | 70.41                                            | 2               | 
| 10261-3-2017 | 14         | 16         | 25                | 2017 | Multifunction Printers                                          | 110       | 2274                 | 20.05                                            | 3.61                                             | 76.34                                            | 2               | 
| 10324-1-2016 | 15         | 16         | 19                | 2016 | KURALON  fiber                                                  | 1500      | 10000                | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | 2               | 
| 10418-1-2013 | 84         | 9          | 19                | 2013 | Portland Cement                                                 | 1000      | 1102                 | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | 2               | 
| 10661-1-2014 | 85         | 28         | 11                | 2014 | 501® Original Jeans – Dark Stonewash                            | 0.997     | 16                   | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | 2               | 
| 10661-1-2015 | 85         | 28         | 6                 | 2015 | 501® Original Jeans – Dark Stonewash                            | 0.997     | 16                   | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | 2               | 

There are 171 duplicate rows, so they need to be removed before analysis 

## Analysis
### 1. Which products contribute the most to carbon emissions?
```sql
SELECT product_name, ROUND(AVG(carbon_footprint_pcf),2) AS carbon_emission
FROM product_emissions
GROUP BY product_name
ORDER BY ROUND(AVG(carbon_footprint_pcf),2) DESC
LIMIT 10;
```

Result: Top 10 products contribute the most to carbon emissions
| product_name                                                                                                                       | carbon_emission | 
| ---------------------------------------------------------------------------------------------------------------------------------: | --------------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | 3718044.00      | 
| Wind Turbine G132 5 Megawats                                                                                                       | 3276187.00      | 
| Wind Turbine G114 2 Megawats                                                                                                       | 1532608.00      | 
| Wind Turbine G90 2 Megawats                                                                                                        | 1251625.00      | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | 191687.00       | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | 167000.00       | 
| TCDE                                                                                                                               | 99075.00        | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | 91000.00        | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | 85000.00        | 
| Mercedes-Benz SL (SL 350)                                                                                                          | 72000.00        | 

### 2. What are the industry groups of these products?
```sql
SELECT 
	pr_em.product_name, 
	in_gr.industry_group,
	ROUND(AVG(pr_em.carbon_footprint_pcf),2) AS carbon_emission
FROM product_emissions pr_em
JOIN industry_groups in_gr ON pr_em.industry_group_id = in_gr.id
GROUP BY pr_em.product_name
ORDER BY ROUND(AVG(pr_em.carbon_footprint_pcf),2) DESC
LIMIT 10;
```

Result: The industry groups of these products
| product_name                                                                                                                       | industry_group                     | carbon_emission | 
| ---------------------------------------------------------------------------------------------------------------------------------: | ---------------------------------: | --------------: | 
| Wind Turbine G128 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3718044.00      | 
| Wind Turbine G132 5 Megawats                                                                                                       | Electrical Equipment and Machinery | 3276187.00      | 
| Wind Turbine G114 2 Megawats                                                                                                       | Electrical Equipment and Machinery | 1532608.00      | 
| Wind Turbine G90 2 Megawats                                                                                                        | Electrical Equipment and Machinery | 1251625.00      | 
| Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.                                                                 | Automobiles & Components           | 191687.00       | 
| Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall | Materials                          | 167000.00       | 
| TCDE                                                                                                                               | Materials                          | 99075.00        | 
| Mercedes-Benz GLE (GLE 500 4MATIC)                                                                                                 | Automobiles & Components           | 91000.00        | 
| Mercedes-Benz S-Class (S 500)                                                                                                      | Automobiles & Components           | 85000.00        | 
| Mercedes-Benz SL (SL 350)                                                                                                          | Automobiles & Components           | 72000.00        | 

### 3. What are the industries with the highest contribution to carbon emissions?
```sql
SELECT 
	in_gr.industry_group, 
	ROUND(SUM(pr_em.carbon_footprint_pcf),2) AS carbon_emissions
FROM product_emissions pr_em
JOIN industry_groups in_gr ON pr_em.industry_group_id = in_gr.id
GROUP BY in_gr.industry_group
ORDER BY ROUND(SUM(pr_em.carbon_footprint_pcf),2) DESC
LIMIT 10;
```

Result: 
| industry_group                                   | carbon_emissions | 
| -----------------------------------------------: | ---------------: | 
| Electrical Equipment and Machinery               | 9801558.00       | 
| Automobiles & Components                         | 2582264.00       | 
| Materials                                        | 577595.00        | 
| Technology Hardware & Equipment                  | 363776.00        | 
| Capital Goods                                    | 258712.00        | 
| "Food, Beverage & Tobacco"                       | 111131.00        | 
| "Pharmaceuticals, Biotechnology & Life Sciences" | 72486.00         | 
| Chemicals                                        | 62369.00         | 
| Software & Services                              | 46544.00         | 
| Media                                            | 23017.00         | 

### 4. What are the companies with the highest contribution to carbon emissions?
```sql
SELECT 
	c.company_name,
	ROUND(SUM(pr_em.carbon_footprint_pcf),2) AS carbon_emissions
FROM product_emissions pr_em
JOIN companies c ON pr_em.company_id = c.id
GROUP BY c.company_name
ORDER BY ROUND(SUM(pr_em.carbon_footprint_pcf),2) DESC
LIMIT 10;
```

Result: 
| company_name                            | carbon_emissions | 
| --------------------------------------: | ---------------: | 
| "Gamesa Corporación Tecnológica, S.A."  | 9778464.00       | 
| Daimler AG                              | 1594300.00       | 
| Volkswagen AG                           | 655960.00        | 
| "Mitsubishi Gas Chemical Company, Inc." | 212016.00        | 
| "Hino Motors, Ltd."                     | 191687.00        | 
| Arcelor Mittal                          | 167007.00        | 
| Weg S/A                                 | 160655.00        | 
| General Motors Company                  | 137007.00        | 
| "Lexmark International, Inc."           | 132012.00        | 
| "Daikin Industries, Ltd."               | 105600.00        | 

### 5. What are the countries with the highest contribution to carbon emissions?
```sql
SELECT 
	cou.country_name,
	ROUND(SUM(pr_em.carbon_footprint_pcf),2) AS carbon_emissions
FROM product_emissions pr_em
JOIN countries cou ON pr_em.company_id = cou.id
GROUP BY cou.country_name
ORDER BY ROUND(SUM(pr_em.carbon_footprint_pcf),2) DESC
LIMIT 10;
```

Result:
| country_name | carbon_emissions | 
| -----------: | ---------------: | 
| Germany      | 9778464.00       | 
| Lithuania    | 212016.00        | 
| Greece       | 191687.00        | 
| Japan        | 132012.00        | 
| Colombia     | 105600.00        | 
| South Africa | 35505.00         | 
| France       | 21364.00         | 
| Italy        | 20000.00         | 
| Ireland      | 11160.00         | 
| India        | 9328.00          | 

### 6. 
