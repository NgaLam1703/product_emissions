
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

