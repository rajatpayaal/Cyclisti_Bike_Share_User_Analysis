# Cyclistic Bike Share EDA
## G.Schlitt
#### 2023-07-21

### Define the issue:


Cyclistic's future depends on maximizing the number of annual memberships.\
How do Casual Riders and Annual Members use Cyclistic bikes differently?

#### Hypothesis:

*Members are local residents who use the system for regular transportation for work and errands.\
Casual Users are tourists who use the service to get from hotels to entertainment locations around the city.*

#### Questions to validate hypothesis:

1.  What is the most common day of the week for each user type?
2.  What is the most common trip start time for each user type?
3.  How long is the average trip for each user type?
4.  Where in the city do most trips start for each user type?
5.  What is the average distance for each trip for each user type?
6.  Does time of the year play a factor in usage for each user type?
7.  What type of bike does each user type rent?\
     

### Deliverables:

#### 1. Statement of Business Task.

-   Analyze rider data to identify usage differences between casual riders and annual members.

#### 2. Description of data sources.

-   CSV files for each month provided by Motivate International Inc. via Amazon AWS Server\
    *<https://divvy-tripdata.s3.amazonaws.com/index.html>*
-   **Trip data from 2022-07 to 2023-06**
    -   202306-divvy-tripdata.zip
    -   202305-divvy-tripdata.zip
    -   202304-divvy-tripdata.zip
    -   202303-divvy-tripdata.zip
    -   202302-divvy-tripdata.zip
    -   202301-divvy-tripdata.zip
    -   202212-divvy-tripdata.zip
    -   202211-divvy-tripdata.zip
    -   202210-divvy-tripdata.zip
    -   202209-divvy-tripdata.zip
    -   202208-divvy-tripdata.zip
    -   202207-divvy-tripdata.zip

#### 3. Documentation of cleaning and manipulation of data.

##### a. Imported to BigQuery and created a single table for all 12 months

```         
        CREATE TABLE `cyclistic-bike-share-v001.bikeshare_trips.12mo_tripdata`
        AS
        SELECT *
        FROM `cyclistic-bike-share-v001.bikeshare_trips.202207`
        UNION ALL
          SELECT *
          FROM `cyclistic-bike-share-v001.bikeshare_trips.202208`
        UNION ALL
          SELECT *
          FROM `cyclistic-bike-share-v001.bikeshare_trips.202209`
        UNION ALL
          SELECT *
          FROM `cyclistic-bike-share-v001.bikeshare_trips.202210`
        UNION ALL
          SELECT *
          FROM `cyclistic-bike-share-v001.bikeshare_trips.202211`
        UNION ALL
          SELECT *
          FROM `cyclistic-bike-share-v001.bikeshare_trips.202212`
        UNION ALL
          SELECT *
          FROM `cyclistic-bike-share-v001.bikeshare_trips.202301`
        UNION ALL
          SELECT *
          FROM `cyclistic-bike-share-v001.bikeshare_trips.202302`
        UNION ALL
          SELECT *
          FROM `cyclistic-bike-share-v001.bikeshare_trips.202303`
        UNION ALL
          SELECT *
          FROM `cyclistic-bike-share-v001.bikeshare_trips.202304`
        UNION ALL
          SELECT *
          FROM `cyclistic-bike-share-v001.bikeshare_trips.202305`
        UNION ALL
          SELECT *
          FROM `cyclistic-bike-share-v001.bikeshare_trips.202306`
```

**Result** = 5,779,444 records


##### b. Clean dataset by:

-   Removing all records with NULL values (1,370,355 records removed).
-   Create 'trip_time' column.
-   Create 'trip_duration' column to account for trips that bridge two days.
-   Create 'start_day' column.
-   Create 'start_time' column.
-   Create 'start_date' column.
-   Create 'month' column.
-   Create 'trip_distance' column.
-   Create 'start_zone' column by breaking up area by lat/lon coordinates into 9 zones.
-   Create 'end_zone' column in the same manner.
-   Excluded all trips under 1 minute in length (91,761 records removed).

```         
CREATE TABLE `cyclistic-bike-share-v001.bikeshare_trips.12mo_tripdata-clean`
AS
SELECT
  member_casual
  , rideable_type
  , start_station_name
  , end_station_name
  , started_at - ended_at AS trip_time
  , TIME_ADD(TIME '0:0:0',INTERVAL TIMESTAMP_DIFF(ended_at, started_at, SECOND) SECOND) AS trip_duration
  , FORMAT_DATE('%A', started_at) AS start_day
  , TIME(started_at) AS start_time
  , DATE(started_at) AS start_date
  , FORMAT_DATE('%B', started_at) AS month
  , ST_DISTANCE(ST_GEOGPOINT(start_lng, start_lat), ST_GEOGPOINT(end_lng, end_lat)) * 0.000621 as trip_distance
  ,

  CASE
    WHEN start_lat > 41.954035 THEN 'far_north'
    WHEN start_lat BETWEEN 41.910916 AND 41.954035 AND start_lng < -87.688464 THEN 'northwest'
    WHEN start_lat BETWEEN 41.910916 AND 41.954035 AND start_lng > -87.688464 THEN 'north'
    WHEN start_lat BETWEEN 41.848365 AND 41.910916 AND start_lng < -87.648274 THEN 'west'
    WHEN start_lat BETWEEN 41.848365 AND 41.910916 AND start_lng > -87.648274 THEN 'central'
    WHEN start_lat BETWEEN 41.765875 AND 41.848365 AND start_lng < -87.648274 THEN 'southwest'
    WHEN start_lat BETWEEN 41.765875 AND 41.848365 AND start_lng > -87.648274 THEN 'south'
    WHEN start_lat < 41.765875 AND start_lng < -87.648274 THEN 'far_southwest'
    WHEN start_lat < 41.765875 AND start_lng > -87.648274 THEN 'southeast'
    ELSE 'ERROR'

  END AS start_zone,
  
  CASE
        WHEN end_lat > 41.954035 THEN 'far_north'
    WHEN end_lat BETWEEN 41.910916 AND 41.954035 AND end_lng < -87.688464 THEN 'northwest'
    WHEN end_lat BETWEEN 41.910916 AND 41.954035 AND end_lng > -87.688464 THEN 'north'
    WHEN end_lat BETWEEN 41.848365 AND 41.910916 AND end_lng < -87.648274 THEN 'west'
    WHEN end_lat BETWEEN 41.848365 AND 41.910916 AND end_lng > -87.648274 THEN 'central'
    WHEN end_lat BETWEEN 41.765875 AND 41.848365 AND end_lng < -87.648274 THEN 'southwest'
    WHEN end_lat BETWEEN 41.765875 AND 41.848365 AND end_lng > -87.648274 THEN 'south'
    WHEN end_lat < 41.765875 AND end_lng < -87.648274 THEN 'far_southwest'
    WHEN end_lat < 41.765875 AND end_lng > -87.648274 THEN 'southeast'
    ELSE 'ERROR'

  END AS end_zone

FROM
  `cyclistic-bike-share-v001.bikeshare_trips.12mo_tripdata`

WHERE
  end_station_id IS NOT NULL 
  AND end_station_name IS NOT NULL 
  AND start_station_id IS NOT NULL 
  AND start_station_name IS NOT NULL 
  AND member_casual IS NOT NULL 
  AND rideable_type IS NOT NULL 
  AND start_lat IS NOT NULL 
  AND start_lng IS NOT NULL 
  AND end_lat IS NOT NULL 
  AND end_lng IS NOT NULL 
  AND started_at IS NOT NULL 
  AND ended_at IS NOT NULL 
  AND ride_id IS NOT NULL
  AND TIME_ADD(TIME '0:0:0',INTERVAL TIMESTAMP_DIFF(ended_at, started_at, SECOND) SECOND) > "00:01:00"
```

**Result** = 4,317,328 records


#### 4. Summary of analysis.

##### Question 1: What is the most common day of the week for each user type?

```         
SELECT
  start_day,
  , COUNTIF(member_casual='member') AS member
  , COUNTIF(member_casual='casual') AS casual
  , COUNT(member_casual) AS total

FROM `cyclistic-bike-share-v001.bikeshare_trips.12mo_tripdata-clean`

GROUP BY
  start_day
```

**Results:** More Members ride on Wednesday while Saturday is the most frequent for Casual Riders.


##### Question 2: What is the most common trip start time for each user type?

```         
SELECT
  TIME_TRUNC(start_time, HOUR) time_slot
  , COUNTIF(member_casual='member') AS member
  , COUNTIF(member_casual='casual') AS casual
  , COUNT(member_casual) AS total

FROM `cyclistic-bike-share-v001.bikeshare_trips.12mo_tripdata-clean`

GROUP BY
  time_slot
ORDER BY
  time_slot
```

**Results:** Both Members and Casual Riders start most of their trips at 5pm.


##### Question 3: How long is the average trip for each user type?

```         
SELECT
  AVG(case when member_casual = 'member' then trip_time else NULL end) as AVG_member_trip_duration
  , AVG(case when member_casual = 'casual' then trip_time else NULL end) as AVG_casual_trip_duration
  , AVG(case when member_casual = 'member' then trip_distance else NULL end) as AVG_member_trip_distance
  , AVG(case when member_casual = 'casual' then trip_distance else NULL end) as AVG_casual_trip_distance

FROM `cyclistic-bike-share-v001.bikeshare_trips.12mo_tripdata-clean`

GROUP BY member_casual
```

**Results:** Members ride an average of 12:22, Casual Riders ride an average of 22:53.  
*However, when accounting for round-trips (trips where the start and end are at the same station):*

```         
SELECT
  AVG(trip_time) AS time_roundtrip
 , MAX(trip_duration) AS max_roundtrip
 , MIN(trip_duration) AS min_roundtrip
 , COUNT(trip_duration) AS number_roundtrips
 , member_casual
 
FROM `cyclistic-bike-share-v001.bikeshare_trips.12mo_tripdata-clean`

WHERE
  trip_distance=0
GROUP BY member_casual
```

**Results:** On round-trip rides, Members and Casual Riders average doubles to 24:36 and 48:13 respectively.   
*Casual Riders are also nearly 2x more likely to return their bikes at the original rental location.*


##### Question 4: Where in the city do most trips start for each user type? 

```         
SELECT
  member_casual
  , start_day
  , COUNTIF(start_zone = 'far_north') far_north
  , COUNTIF(start_zone = 'northwest') northwest
  , COUNTIF(start_zone = 'north') north
  , COUNTIF(start_zone = 'west') west
  , COUNTIF(start_zone = 'central') central
  , COUNTIF(start_zone = 'southwest') southwest
  , COUNTIF(start_zone = 'south') south
  , COUNTIF(start_zone = 'far_southwest') far_southwest
  , COUNTIF(start_zone = 'southeast') southeast


FROM `cyclistic-bike-share-v001.bikeshare_trips.12mo_tripdata-clean`


GROUP BY member_casual, start_day
ORDER BY start_day
```

**Results:** Both rider types are heavily concentrated in the Central zone of the city.


##### Question 5: What is the average distance for each trip for each user type? 

```         
SELECT
  AVG(case when member_casual = 'member' then trip_time else NULL end) as AVG_member_trip_duration
  , AVG(case when member_casual = 'casual' then trip_time else NULL end) as AVG_casual_trip_duration
  , AVG(case when member_casual = 'member' then trip_distance else NULL end) as AVG_member_trip_distance
  , AVG(case when member_casual = 'casual' then trip_distance else NULL end) as AVG_casual_trip_distance

FROM `cyclistic-bike-share-v001.bikeshare_trips.12mo_tripdata-clean`

GROUP BY member_casual
```

**Results:** Members ride an average of 1.30 miles, Casual Riders ride an average of 1.35 miles.


##### Question 6: Does time of the year play a factor in useage for each user type? 

```         
SELECT
  month
  , COUNTIF(member_casual='member') AS member
  , COUNTIF(member_casual='casual') AS casual
  , COUNT(member_casual) AS total

FROM `cyclistic-bike-share-v001.bikeshare_trips.12mo_tripdata-clean`

GROUP BY
  month
```

**Results:** Members peak usage is in August with the low in December (69% drop),  
Casual Riders peak usage comes in July with their low in January (91% drop).


##### Question 7: What type of bike does each user type rent? 

```         
SELECT  
  COUNTIF(rideable_type='electric_bike'AND member_casual='member') AS member_electric
  , COUNTIF(rideable_type='classic_bike'AND member_casual='member') AS member_classic
  , COUNTIF(rideable_type='docked_bike'AND member_casual='member') AS member_docked
  , COUNTIF(rideable_type='electric_bike'AND member_casual='casual') AS casual_electric
  , COUNTIF(rideable_type='classic_bike'AND member_casual='casual') AS casual_classic
  , COUNTIF(rideable_type='docked_bike'AND member_casual='casual') AS casual_docked

FROM `cyclistic-bike-share-v001.bikeshare_trips.12mo_tripdata-clean`
```

**Results:** 62% of Members prefer Classic bikes with the remaining 38% renting Electric.  
48% or Casual Riders rent Classic bikes, 44% rent Electric, and the remaining 8% rent Docked bikes.


#### 5. Conclusion:

-   Data Limitations:  
-   No customer identifiers available - limits knowledge of recurring trips, frequency of usage, variety of trip origins, etc.  
-   No financial data available - unable to analyze revenue per ride, revenue by user type, revenue by location, etc.  

##### Hypothesis Conclusion:

-   Data suggests Members use service as weekly transportation to/from work and for recreational on weekends.  
-   Casual Riders are more likely to rent in high tourist areas, use for longer periods of time, and return to the original station.  

#### 6. Recommendations based on analysis:

-   To increase Annual Membership, Cyclistic should focus on the following:  
    -   **Geographically:** Areas south of 26th Street are severely underused. Campaigns targeting these areas should increase overall ridership significantly.
    -   **Conversion:** Target casual riders with Weekend Membership option.
    -   **Tourists:** Offer a Weekly Pass option for tourists.
