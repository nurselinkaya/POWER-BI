In this project, I have reviewed different hotels' sales analytics from different locations in India.

### 1) Data cleaning and preparation:

First, I have checked the missing values and outliers. This dataset is clean. Therefore, there was no need for cleaning. However, for data preparation, I have created new columns such as from dim_date dataset, I created "Day" column, which shows, whether it is a weekday or weekend. In hotel business, weekend is considered as Friday and Saturday, and the rest of the week is accepted as weekdays. Moreover, the week number column was in the format of "W 19". I have created a new column "WN", which contains only the number of the week.

### 2) DAX measures:

To have meaningful numbers on the dashboard, I have created several new measures, which will be the key metrics of the analysis. Each measure and the DAX code can be found below:

#### ADR: Average Daily Rate
ADR = DIVIDE([Revenue], [Total Bookings],0)

#### ADR WoW Change: Average Daily Rate Week over Week Change
ADR WoW Change = 
Var selv =  
IF(HASONEFILTER(dim_date[WN]), SELECTEDVALUE(dim_date[WN]), MAX(dim_date[WN]))
var revcw = CALCULATE([ADR], dim_date[WN] = selv)
var revpw = CALCULATE([ADR], FILTER(ALL(dim_date),dim_date[WN] = selv -1))

return 

(DIVIDE(revcw, revpw,0)-1)*100

#### Average Rating
Average Rating = AVERAGE(fact_bookings[ratings_given])

#### Booking % by Platform
Booking % by Platform = DIVIDE([Total Bookings],
 CALCULATE([Total Bookings], 
 ALL(fact_bookings[booking_platform])
  ))*100

#### Booking & by Room Type
Booking % by room = DIVIDE([Total Bookings], CALCULATE([Total Bookings],ALL(dim_rooms[room_class])))*100

#### Cancellation %
Cancellation% = DIVIDE([Total Cancelled Bookings], [Total Bookings])*100

#### DBRN: Daily Booked Room Nights
DBRN = DIVIDE([Total Bookings], [No of Days])

#### DBRN WoW Change: Daily Booked Room Nights Week over Week Change
DBRN WoW change% = 
Var selv = IF(HASONEFILTER(dim_date[wn]),SELECTEDVALUE(dim_date[wn]),MAX(dim_date[wn]))
var revcw = CALCULATE([DBRN],dim_date[wn]= selv)
var revpw =  CALCULATE([DBRN],FILTER(ALL(dim_date),dim_date[wn]= selv-1))

return

DIVIDE(revcw,revpw,0)-1

#### No of Days 
No of Days = DATEDIFF(MIN(dim_date[date]), MAX(dim_date[date]), DAY)+1

#### No Show Rate %
No Show Rate % = DIVIDE([Total No Show Bookings], [Total Bookings])*100

#### Occupancy %
Occupancy % = DIVIDE(SUM(fact_aggregated_bookings[successful_bookings]), SUM(fact_aggregated_bookings[capacity]))*100

#### Occupancy WoW Change
Occupancy WoW Change% = 
Var selv = IF(HASONEFILTER(dim_date[WN]),SELECTEDVALUE(dim_date[WN]),MAX(dim_date[WN]))
var revcw = CALCULATE([Occupancy %],dim_date[WN]= selv)
var revpw =  CALCULATE([Occupancy %],FILTER(ALL(dim_date),dim_date[WN]= selv-1))

return

(DIVIDE(revcw,revpw,0)-1)*100

#### Realization %
Realization % = 100- ([Cancellation%] + [No Show Rate %])

#### Realization WoW Change
Realization WoW Change% = 
Var selv =  
IF(HASONEFILTER(dim_date[WN]), SELECTEDVALUE(dim_date[WN]), MAX(dim_date[WN]))
var revcw = CALCULATE([Realization %], dim_date[WN] = selv)
var revpw = CALCULATE([Realization %], FILTER(ALL(dim_date),dim_date[WN] = selv -1))

return 

(DIVIDE(revcw, revpw,0)-1)*100

#### RevPar: Revenue per Available Room
Rev Par = DIVIDE([Revenue], [Total Capacity],0)

#### Revenue
Revenue = SUM(fact_bookings[revenue_realized])

#### Revenue Change WoW %
Revenue Change WoW % = 
Var selv =  
IF(HASONEFILTER(dim_date[WN]), SELECTEDVALUE(dim_date[WN]), MAX(dim_date[WN]))
var revcw = CALCULATE([Revenue], dim_date[WN] = selv)
var revpw = CALCULATE([Revenue], FILTER(ALL(dim_date),dim_date[WN] = selv -1))

return 

(DIVIDE(revcw, revpw,0)-1)*100

#### RevPar WoW Change
RevPar WoW Change% = 
Var selv =  
IF(HASONEFILTER(dim_date[WN]), SELECTEDVALUE(dim_date[WN]), MAX(dim_date[WN]))
var revcw = CALCULATE([Rev Par], dim_date[WN] = selv)
var revpw = CALCULATE([Rev Par], FILTER(ALL(dim_date),dim_date[WN] = selv -1))

return 

(DIVIDE(revcw, revpw,0)-1)*100

#### Total Bookings
Total Bookings = COUNT(fact_bookings[booking_id])

#### Total Cancelled Bookings
Total Cancelled Bookings = CALCULATE(COUNT(fact_bookings[booking_id]), fact_bookings[booking_status]="Cancelled")

#### Total Capacity
Total Capacity = SUM(fact_aggregated_bookings[capacity])

#### Total Checked Out
Total Checked Out = CALCULATE([Total Bookings],fact_bookings[booking_status]="Checked Out")

#### Total No Show Bookings
Total No Show Bookings = CALCULATE([Total Bookings], fact_bookings[booking_status]="No Show")

#### Total Successful Bookings
Total Successful Bookings = SUM(fact_aggregated_bookings[successful_bookings])


### 3) Dashboard Creation:

- First, I have created a table which contains the important metrics and original columns from the dataset. The table was sorted out according to "property_id".
- Then, I created two filters: by City and Room Type.
- I created two slicers: One with months and one with the week numbers.
- I created a line chart with RevPar, Occupancy % and ADR based on the week numbers. 
- Then, I created a donut chart to show the revenue by category: Luxury and Business.
- I created a line and stacked chart with realization % and ADR according to different booking platforms.
- Next, I created a section with different cards including Revenue, RevPar, DBRN, Occupancy %, ADR and Realization %. Below each card, I have inserted the WoW changes of each in the form of table.









