# product_emissions
#nối các bảng lại với nhau

```sql
WITH clean AS
(select 
 	p.id,
 	p.product_name,
 	p.year,
 	p.carbon_footprint_pcf,
 	p.upstream_percent_total_pcf,
 	p.operations_percent_total_pcf,
 	p.downstream_percent_total_pcf ,
 	i.industry_group,
 	c.company_name,
 	c1.country_name
from product_emissions p
left join industry_groups i on p.industry_group_id = i.id
left join companies c on p.company_id = c.id
left join countries c1 on p.country_id = c1.id
group by p.id);

SELECT * FROM clean LIMIT 10;


| id           | product_name                                                    | year | carbon_footprint_pcf | upstream_percent_total_pcf                       | operations_percent_total_pcf                     | downstream_percent_total_pcf                     | industry_group                                 | company_name           | country_name | 
| -----------: | --------------------------------------------------------------: | ---: | -------------------: | -----------------------------------------------: | -----------------------------------------------: | -----------------------------------------------: | ---------------------------------------------: | ---------------------: | -----------: | 
| 10056-1-2014 | Frosted Flakes(R) Cereal                                        | 2014 | 2                    | 57.50                                            | 30.00                                            | 12.50                                            | "Food, Beverage & Tobacco"                     | Kellogg Company        | USA          | 
| 10056-1-2015 | "Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)" | 2015 | 2                    | 57.50                                            | 30.00                                            | 12.50                                            | Food & Beverage Processing                     | Kellogg Company        | USA          | 
| 10222-1-2013 | Office Chair                                                    | 2013 | 73                   | 80.63                                            | 17.36                                            | 2.01                                             | Capital Goods                                  | KNOLL INC              | USA          | 
| 10261-1-2017 | Multifunction Printers                                          | 2017 | 1488                 | 30.65                                            | 5.51                                             | 63.84                                            | Technology Hardware & Equipment                | "Konica Minolta, Inc." | Japan        | 
| 10261-2-2017 | Multifunction Printers                                          | 2017 | 1818                 | 25.08                                            | 4.51                                             | 70.41                                            | Technology Hardware & Equipment                | "Konica Minolta, Inc." | Japan        | 
| 10261-3-2017 | Multifunction Printers                                          | 2017 | 2274                 | 20.05                                            | 3.61                                             | 76.34                                            | Technology Hardware & Equipment                | "Konica Minolta, Inc." | Japan        | 
| 10324-1-2016 | KURALON  fiber                                                  | 2016 | 10000                | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | Materials                                      | "Kuraray Co., Ltd."    | Japan        | 
| 10418-1-2013 | Portland Cement                                                 | 2013 | 1102                 | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | Materials                                      | Lafarge S.A.           | France       | 
| 10661-1-2014 | 501® Original Jeans – Dark Stonewash                            | 2014 | 16                   | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | Consumer Durables & Apparel                    | Levi Strauss & Co.     | USA          | 
| 10661-1-2015 | 501® Original Jeans – Dark Stonewash                            | 2015 | 16                   | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | N/a (product with insufficient stage-level data) | "Textiles, Apparel, Footwear and Luxury Goods" | Levi Strauss & Co.     | USA          | 

```sql
SELECT industry_group, product_name
FROM clean
```

| industry_group                                 | product_name                                                    | 
| ---------------------------------------------: | --------------------------------------------------------------: | 
| "Food, Beverage & Tobacco"                     | Frosted Flakes(R) Cereal                                        | 
| Food & Beverage Processing                     | "Frosted Flakes, 23 oz, produced in Lancaster, PA (one carton)" | 
| Capital Goods                                  | Office Chair                                                    | 
| Technology Hardware & Equipment                | Multifunction Printers                                          | 
| Technology Hardware & Equipment                | Multifunction Printers                                          | 
| Technology Hardware & Equipment                | Multifunction Printers                                          | 
| Materials                                      | KURALON  fiber                                                  | 
| Materials                                      | Portland Cement                                                 | 
| Consumer Durables & Apparel                    | 501® Original Jeans – Dark Stonewash                            | 
| "Textiles, Apparel, Footwear and Luxury Goods" | 501® Original Jeans – Dark Stonewash                            | 

```sql
select industry_group, carbon_footprint_pcf
from clean 
order by carbon_footprint_pcf DESC
LIMIT 5;

| industry_group                     | carbon_footprint_pcf | 
| ---------------------------------: | -------------------: | 
| Electrical Equipment and Machinery | 3718044              | 
| Electrical Equipment and Machinery | 3276187              | 
| Electrical Equipment and Machinery | 1532608              | 
| Electrical Equipment and Machinery | 1251625              | 
| Automobiles & Components           | 191687               |

