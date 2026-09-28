# Airbnb Market Analysis

Eight dplyr analyses of Midwest Airbnb listings (Chicago, Columbus, Twin Cities) 
from `midwest_airbnb_extended.db`. Code: `airbnb_analysis.Rmd`. Knitted output: 
`airbnb_analysis.html`.

## Findings

1. Hosts with ten or more listings hold 37.1% of the listings in Columbus; in 
Chicago, this makes up 31.9% of their listings, and 17.2% in the Twin Cities. 
Columbus has the most professionally run market. 

2. The top 10% of listings earn 40.7% of the estimated revenue in Chicago, 
40.3% in the Twin Cities, and 36.1% in Columbus. A small percentage of listings 
take a large share of the money in all three cities. 

3. A typical entire home in the Twin Cities earns about $19,619 in estimated 
revenue per year; this is below Chicago at $27,057 and Columbus at $21,643. 
This is an important piece of information for those deciding to list their 
entire home. 

4. The revenue in the Twin Cities peaked in August 2025 at 2.37 times their 
February low; hosts there should expect strong summers and slower winters. 

5. There are 456 listings with no availability calendar at all; 267 of them are 
in Chicago, so any analysis of future bookings would leave those listings out. 
