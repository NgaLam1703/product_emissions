# Carbon Emission Analysis

This report aims to analyze carbon emissions to examine the carbon footprint across various industries. We aim to identify sectors with the highest levels of emissions by analyzing them across countries and years, as well as to uncover trends.
Carbon emissions play a crucial role in the environment, accounting for over 75% of global emissions and posing a significant environmental challenge. These emissions contribute to the accumulation of greenhouse gases in the atmosphere, leading to climate change, planetary warming, and involvement in various environmental disasters.
Through this analysis, we hope to gain an understanding of the environmental impact of different industries and contribute to making informed decisions in sustainable development.

![Alt text](https://github.com/NgaLam1703/product_emissions/blob/main/chris-leboutillier-TUJud0AWAPI-unsplash.jpg?raw=true)
Photo by Chris LeBoutillier (unsplash.com)

## Data Source: Where Our Data Comes From
Our dataset is compiled from publicly available data from nature.com and encompasses the product carbon footprints (PCF) for various companies. PCFs represent the greenhouse gas emissions associated with specific products, quantified in CO2 (carbon dioxide equivalent).

## Data Structure
The dataset consists of 4 tables containing information regarding carbon emissions generated during the production of goods.

![image](https://github.com/user-attachments/assets/70f8195a-3aa7-4ce3-a125-8a2c193d30e9)

## Data and Tables
Before diving into the project we will take a look at the 4 tables and their data.

### Table product_emissions

```
SELECT *
FROM product_emissions pe limit 5;
```
|id|company_id|country_id|industry_group_id|year|product_name|weight_kg|carbon_footprint_pcf|upstream_percent_total_pcf|operations_percent_total_pcf|downstream_percent_total_pcf|
|--|----------|----------|-----------------|----|------------|---------|--------------------|--------------------------|----------------------------|----------------------------|
|10056-1-2014|82|28|2|2014|Frosted Flakes(R) Cereal|0.7485|2|57.50|30.00|12.50|
|10056-1-2015|82|28|15|2015|"Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)"|0.7485|2|57.50|30.00|12.50|
|10222-1-2013|83|28|8|2013|Office Chair|20.68|73|80.63|17.36|2.01|
|10261-1-2017|14|16|25|2017|Multifunction Printers|110.0|1488|30.65|5.51|63.84|
|10261-2-2017|14|16|25|2017|Multifunction Printers|110.0|1818|25.08|4.51|70.41|

### Table industry_groups

```
SELECT *
FROM industry_groups ig LIMIT 5;
```
|id|industry_group|
|--|--------------|
|1|"Consumer Durables, Household and Personal Products"|
|2|"Food, Beverage & Tobacco"|
|3|"Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber"|
|4|"Mining - Iron, Aluminum, Other Metals"|
|5|"Pharmaceuticals, Biotechnology & Life Sciences"|

### Table companies

```
SELECT *
FROM companies c LIMIT 5;
```
|id|company_name|
|--|------------|
|1|"Autodesk, Inc."|
|2|"Casio Computer Co., Ltd."|
|3|"Cisco Systems, Inc."|
|4|"CNX Coal Resources, LP"|
|5|"Coca-Cola Enterprises, Inc."|

### Table countries

```
SELECT * FROM countries c LIMIT 5;
```
|id|country_name|
|--|------------|
|1|Australia|
|2|Belgium|
|3|Brazil|
|4|Canada|
|5|Chile|

### Check duplicates
Before analyzing the project, we will search for duplicate information and group by that duplicate data.

```
SELECT COUNT(product_name) AS 'Total number of products',
	COUNT(DISTINCT(product_name)) AS 'Number of unique products'
FROM product_emissions pe ;
```
|Total number of products|Number of unique products|
|------------------------|-------------------------|
|1037|661|
The result shows that we have duplicate data so we will solve them by grouping by them.

## Data is clean, let's go into the project.
Now we will find out which factors will create the most carbon emissions.

### Products contribute the most to carbon emissions

```
SELECT 
	product_name,
	SUM(carbon_footprint_pcf) AS total_carbon_pcf
FROM product_emissions pe
GROUP BY product_name
ORDER BY SUM(carbon_footprint_pcf) DESC
LIMIT 1;
```
|product_name|total_carbon_pcf|
|------------|----------------|
|Wind Turbine G128 5 Megawats|3718044|

Wind Turbine G128 5 Megawatts is the product that generates the highest amount of carbon, Turbine also brings higher power production efficiency, but cheaper development and operating costs. It is becoming a trend in power supply in many countries.

### Top 10 Industries groups of these products with the Highest Contribution to Carbon Emissions
```
SELECT 
	PE.product_name,
	IG.industry_group AS 'Industry group',
	ROUND(AVG(PE.carbon_footprint_pcf), 2) AS 'Average carbon PCF'
FROM product_emissions PE
LEFT JOIN 
	industry_groups IG 
ON PE.industry_group_id = IG.id
GROUP BY PE.product_name
ORDER BY ROUND(AVG(PE.carbon_footprint_pcf), 2) DESC LIMIT 10;
```
|product_name|Industry group|Average carbon PCF|
|------------|--------------|------------------|
|Wind Turbine G128 5 Megawats|Electrical Equipment and Machinery|3718044.00|
|Wind Turbine G132 5 Megawats|Electrical Equipment and Machinery|3276187.00|
|Wind Turbine G114 2 Megawats|Electrical Equipment and Machinery|1532608.00|
|Wind Turbine G90 2 Megawats|Electrical Equipment and Machinery|1251625.00|
|Land Cruiser Prado. FJ Cruiser. Dyna trucks. Toyoace.IMV def unit.|Automobiles & Components|191687.00|
|Retaining wall structure with a main wall (sheet pile): 136 tonnes of steel sheet piles and 4 tonnes of tierods per 100 meter wall|Materials|167000.00|
|TCDE|Materials|99075.00|
|Mercedes-Benz GLE (GLE 500 4MATIC)|Automobiles & Components|91000.00|
|Mercedes-Benz S-Class (S 500)|Automobiles & Components|85000.00|
|Mercedes-Benz SL (SL 350)|Automobiles & Components|72000.00|

Not only the electrical equipment and machinery industry emits high carbon emissions, the Automotive & Components and Materials industries also contribute to the greenhouse effect. I am very concerned about the impact of Automotive & Components because it is closely related to our daily lives, Mercedes-Benz is popular.

### Top 10 Industries with the highest contribution to carbon emissions

```
SELECT
	IG.industry_group AS 'Industry group',
	ROUND(SUM(PE.carbon_footprint_pcf), 2) AS 'Total carbon PCF'
FROM product_emissions PE
LEFT JOIN industry_groups IG
ON PE.industry_group_id = IG.id
GROUP BY IG.industry_group
ORDER BY ROUND(SUM(PE.carbon_footprint_pcf), 2) DESC
LIMIT 10;
```
|Industry group|Total carbon PCF|
|--------------|----------------|
|Electrical Equipment and Machinery|9801558.00|
|Automobiles & Components|2582264.00|
|Materials|577595.00|
|Technology Hardware & Equipment|363776.00|
|Capital Goods|258712.00|
|"Food, Beverage & Tobacco"|111131.00|
|"Pharmaceuticals, Biotechnology & Life Sciences"|72486.00|
|Chemicals|62369.00|
|Software & Services|46544.00|
|Media|23017.00|

When the average is changed to total, the Top sector retains its leading position. What is a bit surprising is that "Food, Beverage & Tobacco" and "Pharmaceuticals, Biotechnology & Life Sciences" are essential but high carbon-emitting sectors.
Pharmaceuticals, Biotechnology & Life Sciences industry is one of the leaders in carbon emissions - Now we know what our health comes at.

### Top 8 Companies with the highest contribution to carbon emissions

```SELECT 
	cp.company_name,
	ROUND(SUM(pe.carbon_footprint_pcf), 2) AS 'Total carbon PCF'
FROM product_emissions pe
LEFT JOIN companies cp
	ON pe.company_id = cp.id
GROUP BY cp.company_name
ORDER BY ROUND(SUM(pe.carbon_footprint_pcf), 2) DESC
LIMIT 8;
```
|company_name|Total carbon PCF|
|------------|----------------|
|"Gamesa Corporación Tecnológica, S.A."|9778464.00|
|Daimler AG|1594300.00|
|Volkswagen AG|655960.00|
|"Mitsubishi Gas Chemical Company, Inc."|212016.00|
|"Hino Motors, Ltd."|191687.00|
|Arcelor Mittal|167007.00|
|Weg S/A|160655.00|
|General Motors Company|137007.00|

### Top 10 Countries with the highest contribution to carbon emissions

```
SELECT 
	c.country_name,
	ROUND(SUM(pe.carbon_footprint_pcf), 2) AS 'Total carbon PCF'
FROM product_emissions pe
LEFT JOIN countries c
	ON pe.country_id = c.id 
GROUP BY c.country_name
ORDER BY ROUND(SUM(pe.carbon_footprint_pcf), 2) DESC 
LIMIT 10;
```
|country_name|Total carbon PCF|
|------------|----------------|
|Spain|9786130.00|
|Germany|2251225.00|
|Japan|653237.00|
|USA|518381.00|
|South Korea|186965.00|
|Brazil|169337.00|
|Luxembourg|167007.00|
|Netherlands|70417.00|
|Taiwan|62875.00|
|India|24574.00|

The locations of the above countries are all in Europe. According to VietNam+, CO2 emissions from energy use are the main cause of global warming and account for about 75% of total greenhouse gas emissions caused by humans in the EU. (Minh Hang, https://www.vietnamplus.vn/luong-khi-thai-co2-do-su-dung-nang-luong-tai-chau-au-giam-post867468.vnp)
### the trend of carbon footprints (PCFs) over the years

```
SELECT 
	year,
	ROUND(SUM(carbon_footprint_pcf), 2) AS 'Total carbon PCF',
	ROUND(AVG(carbon_footprint_pcf), 2) AS 'Average carbon PCF'
FROM product_emissions 
GROUP BY year 
ORDER BY year;
```
|year|Total carbon PCF|Average carbon PCF|
|----|----------------|------------------|
|2013|503857.00|2399.32|
|2014|624226.00|2457.58|
|2015|10840415.00|43188.90|
|2016|1640182.00|6891.52|
|2017|340271.00|4050.85|

From the above results, I see that carbon concentration tends to increase steadily over the years from 2013 to 2015. 
In 2015, carbon emissions were at their highest level and then tended to decrease gradually.
###  Industry groups has demonstrated the most notable decrease in carbon footprints (PCFs) over time

```
SELECT 
	pe.year,
	ig.industry_group,
	ROUND(SUM(pe.carbon_footprint_pcf), 2) AS 'Total carbon PCF'
FROM product_emissions pe 
LEFT JOIN industry_groups ig 
	ON pe.industry_group_id = ig.id 
GROUP BY pe.year, ig.industry_group
ORDER BY 
	industry_group,
	year,
	ROUND(SUM(pe.carbon_footprint_pcf), 2);
```
|year|industry_group|Total carbon PCF|
|----|--------------|----------------|
|2015|"Consumer Durables, Household and Personal Products"|931.00|
|2013|"Food, Beverage & Tobacco"|4995.00|
|2014|"Food, Beverage & Tobacco"|2685.00|
|2015|"Food, Beverage & Tobacco"|0.00|
|2016|"Food, Beverage & Tobacco"|100289.00|
|2017|"Food, Beverage & Tobacco"|3162.00|
|2015|"Forest and Paper Products - Forestry, Timber, Pulp and Paper, Rubber"|8909.00|
|2015|"Mining - Iron, Aluminum, Other Metals"|8181.00|
|2013|"Pharmaceuticals, Biotechnology & Life Sciences"|32271.00|
|2014|"Pharmaceuticals, Biotechnology & Life Sciences"|40215.00|
|2015|"Textiles, Apparel, Footwear and Luxury Goods"|387.00|
|2013|Automobiles & Components|130189.00|
|2014|Automobiles & Components|230015.00|
|2015|Automobiles & Components|817227.00|
|2016|Automobiles & Components|1404833.00|
|2013|Capital Goods|60190.00|
|2014|Capital Goods|93699.00|
|2015|Capital Goods|3505.00|
|2016|Capital Goods|6369.00|
|2017|Capital Goods|94949.00|
|2015|Chemicals|62369.00|
|2013|Commercial & Professional Services|1157.00|
|2014|Commercial & Professional Services|477.00|
|2016|Commercial & Professional Services|2890.00|
|2017|Commercial & Professional Services|741.00|
|2013|Consumer Durables & Apparel|2867.00|
|2014|Consumer Durables & Apparel|3280.00|
|2016|Consumer Durables & Apparel|1162.00|
|2015|Containers & Packaging|2988.00|
|2015|Electrical Equipment and Machinery|9801558.00|
|2013|Energy|750.00|
|2016|Energy|10024.00|
|2015|Food & Beverage Processing|141.00|
|2014|Food & Staples Retailing|773.00|
|2015|Food & Staples Retailing|706.00|
|2016|Food & Staples Retailing|2.00|
|2015|Gas Utilities|122.00|
|2013|Household & Personal Products|0.00|
|2013|Materials|200513.00|
|2014|Materials|75678.00|
|2016|Materials|88267.00|
|2017|Materials|213137.00|
|2013|Media|9645.00|
|2014|Media|9645.00|
|2015|Media|1919.00|
|2016|Media|1808.00|
|2014|Retailing|19.00|
|2015|Retailing|11.00|
|2014|Semiconductors & Semiconductor Equipment|50.00|
|2016|Semiconductors & Semiconductor Equipment|4.00|
|2015|Semiconductors & Semiconductors Equipment|3.00|
|2013|Software & Services|6.00|
|2014|Software & Services|146.00|
|2015|Software & Services|22856.00|
|2016|Software & Services|22846.00|
|2017|Software & Services|690.00|
|2013|Technology Hardware & Equipment|61100.00|
|2014|Technology Hardware & Equipment|167361.00|
|2015|Technology Hardware & Equipment|106157.00|
|2016|Technology Hardware & Equipment|1566.00|
|2017|Technology Hardware & Equipment|27592.00|
|2013|Telecommunication Services|52.00|
|2014|Telecommunication Services|183.00|
|2015|Telecommunication Services|183.00|
|2015|Tires|2022.00|
|2015|Tobacco|1.00|
|2015|Trading Companies & Distributors and Commercial Services & Supplies|239.00|
|2013|Utilities|122.00|
|2016|Utilities|122.00|

Based on the results obtained, we can see that the Top industry groups still have a steady increase in carbon emissions over the years such as:
- Automobiles & Components
- Pharmaceuticals, Biotechnology & Life Sciences
- Food, Beverage & Tobacco,...

Some industry groups have a decrease in emissions such as: Food & Staples Retailing and Retailing

# Insights
After analyzing and exploring the data, it shows that:
- The products that generate the highest amount of carbon emissions are heavy industries such as:
  	-Electrical Equipment and Machinery
	-Automobiles & Components
	-Materials
	-Technology Hardware & Equipment
- Popular luxury car models such as: Mercedes-Benz GLE, Mercedes-Benz S-Class, Mercedes-Benz SL contribute to creating very high greenhouse gas emissions.
- In particular, the Pharmaceuticals, Biotechnology & Life Sciences industry is a leading industry group that is important to humans but it contributes to creating high greenhouse gas emissions.
- Most of the countries with the highest carbon emissions are located in continental Europe.
The amount of carbon emissions peaked in 2015 and has been decreasing.
- The Food & Staples Retailing and Retailing industry group has the most prominent reduction in carbon emissions
