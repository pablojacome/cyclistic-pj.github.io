# pablojacome.github.io

--PREPARE

-- 1) Change data type in order to standardize all tables for the UNION clause
ALTER TABLE Case_Study_1.dbo.trip_data_04_22
ALTER COLUMN start_station_id nvarchar(255)

ALTER TABLE Case_Study_1.dbo.trip_data_07_22
ALTER COLUMN start_station_id nvarchar(255)

ALTER TABLE Case_Study_1.dbo.trip_data_11_21
ALTER COLUMN start_station_id nvarchar(255)

--2)Create a temporary table which includes the union of datasets from august 2021 to july 2022

SELECT ride_id, rideable_type, started_at, ended_at, start_station_name, start_station_id, end_station_name, end_station_id,
start_lat, start_lng, end_lat, end_lng, member_casual
INTO #FinalTable
FROM
(
SELECT ride_id, rideable_type, started_at, ended_at, start_station_name, start_station_id, end_station_name, end_station_id,
start_lat, start_lng, end_lat, end_lng, member_casual
FROM Case_Study_1.dbo.trip_data_08_21

UNION
SELECT ride_id, rideable_type, started_at, ended_at, start_station_name, start_station_id, end_station_name, end_station_id,
start_lat, start_lng, end_lat, end_lng, member_casual
FROM Case_Study_1.dbo.trip_data_09_21

UNION
SELECT ride_id, rideable_type, started_at, ended_at, start_station_name, start_station_id, end_station_name, end_station_id,
start_lat, start_lng, end_lat, end_lng, member_casual
FROM Case_Study_1.dbo.trip_data_10_21

UNION
SELECT ride_id, rideable_type, started_at, ended_at, start_station_name, start_station_id, end_station_name, end_station_id,
start_lat, start_lng, end_lat, end_lng, member_casual
FROM Case_Study_1.dbo.trip_data_11_21

UNION
SELECT ride_id, rideable_type, started_at, ended_at, start_station_name, start_station_id, end_station_name, end_station_id,
start_lat, start_lng, end_lat, end_lng, member_casual
FROM Case_Study_1.dbo.trip_data_12_21

UNION
SELECT ride_id, rideable_type, started_at, ended_at, start_station_name, start_station_id, end_station_name, end_station_id,
start_lat, start_lng, end_lat, end_lng, member_casual
FROM Case_Study_1.dbo.trip_data_01_22

UNION
SELECT ride_id, rideable_type, started_at, ended_at, start_station_name, start_station_id, end_station_name, end_station_id,
start_lat, start_lng, end_lat, end_lng, member_casual
FROM Case_Study_1.dbo.trip_data_02_22

UNION
SELECT ride_id, rideable_type, started_at, ended_at, start_station_name, start_station_id, end_station_name, end_station_id,
start_lat, start_lng, end_lat, end_lng, member_casual
FROM Case_Study_1.dbo.trip_data_03_22

UNION
SELECT ride_id, rideable_type, started_at, ended_at, start_station_name, start_station_id, end_station_name, end_station_id,
start_lat, start_lng, end_lat, end_lng, member_casual
FROM Case_Study_1.dbo.trip_data_04_22

UNION
SELECT ride_id, rideable_type, started_at, ended_at, start_station_name, start_station_id, end_station_name, end_station_id,
start_lat, start_lng, end_lat, end_lng, member_casual
FROM Case_Study_1.dbo.trip_data_05_22

UNION
SELECT ride_id, rideable_type, started_at, ended_at, start_station_name, start_station_id, end_station_name, end_station_id,
start_lat, start_lng, end_lat, end_lng, member_casual
FROM Case_Study_1.dbo.trip_data_06_22

UNION
SELECT ride_id, rideable_type, started_at, ended_at, start_station_name, start_station_id, end_station_name, end_station_id,
start_lat, start_lng, end_lat, end_lng, member_casual
FROM Case_Study_1.dbo.trip_data_07_22
) a


-- PROCESS

--  3) Removing all null values. Create a new temporary table 
SELECT *
INTO #FinalTable2
FROM
(
SELECT DISTINCT *
FROM #FinalTable
WHERE 
ride_id IS NOT NULL AND 
rideable_type IS NOT NULL AND 
started_at IS NOT NULL AND 
ended_at IS NOT NULL AND
start_lat IS NOT NULL AND 
start_lng IS NOT NULL AND
start_station_name IS NOT NULL AND
end_station_name IS NOT NULL
) a

-- 4) Create a function that allows to calculate the distances in km

CREATE FUNCTION dbo.fncCalcula_Distancia_Coordenada (
    @Latitude1 FLOAT,
    @Longitude1 FLOAT,
    @Latitude2 FLOAT,
    @Longitude2 FLOAT
)
RETURNS FLOAT
AS
BEGIN
 
    DECLARE @PI FLOAT = PI()
 
    DECLARE @lat1Radianos FLOAT = @Latitude1 * @PI / 180
    DECLARE @lng1Radianos FLOAT = @Longitude1 * @PI / 180
    DECLARE @lat2Radianos FLOAT = @Latitude2 * @PI / 180
    DECLARE @lng2Radianos FLOAT = @Longitude2 * @PI / 180
 
    RETURN (ACOS(COS(@lat1Radianos) * COS(@lng1Radianos) * COS(@lat2Radianos) * COS(@lng2Radianos) + COS(@lat1Radianos) * SIN(@lng1Radianos) * COS(@lat2Radianos) * SIN(@lng2Radianos) + SIN(@lat1Radianos) * SIN(@lat2Radianos)) * 6371) * 1.15
 
END


-- 5) Create a column that calculates the ride length in minutes. Another column that determines the day of the week and a last one that 
--  calculates the distance in km

SELECT *
INTO #FinalTable3
FROM (
SELECT member_casual, rideable_type, TRIM( start_station_name ) AS station_name, ROUND(start_lat,1) AS latitude, ROUND(start_lng,1) AS longitude,
DATEDIFF(MINUTE,started_at,ended_at) AS ride_length_min, DATEPART(DW,started_at) AS day_of_week, MONTH(started_at) AS month_of_year,
ROUND(dbo.fncCalcula_Distancia_Coordenada(start_lat,start_lng,end_lat,end_lng),2) AS distance_km
FROM #FinalTable2
WHERE start_lat <> end_lat
)a

-- 6) Remove inconsistent data by assuming an avg speed of 10 km/h. Calculate teoric time with speed formula and give 15 min plus if 
 the user make some stop. Rest 2 min for the minimum time in case there are fastest users.
SELECT *
INTO #FinalTable4
FROM (
SELECT *,
CASE 
	WHEN ride_length_min > ((distance_km/0.17)+15) THEN 'INUSUAL'
	WHEN ride_length_min < ((distance_km/0.17)-2) THEN 'INUSUAL'
	WHEN ((distance_km/0.17)-2) <= 0 THEN 'INUSUAL'
	WHEN ride_length_min <= 0 THEN 'INUSUAL'
	WHEN distance_km = 0 THEN 'INUSUAL'
	ELSE 'NORMAL'
END AS inconsistent_data
FROM #FinalTable3
) a

--  7) Discard "Inusual" Behavior and create a new table
SELECT *
INTO #CompleteDataset
FROM(
SELECT member_casual, rideable_type, station_name, latitude, longitude, ride_length_min, day_of_week, month_of_year, distance_km
FROM #FinalTable4
WHERE inconsistent_data LIKE 'NORMAL'
) a

-- ANALYSIS 

-- 8) Determine total number of memberships
SELECT member_casual, 
COUNT(member_casual) AS total_membership_rides,
AVG(ride_length_min) AS avg_ride_length_min, 
MAX(ride_length_min) AS max_ride_length_min,
MIN(ride_length_min) AS min_ride_length_min,
AVG(distance_km) AS avg_distance_km,
MAX(distance_km) AS max_distance_km,
MIN(distance_km) AS min_distance_km
FROM #CompleteDataset
GROUP BY member_casual


-- 9) Determine behavior with month of year
SELECT member_casual, month_of_year,
COUNT(member_casual) AS total_membership_rides
FROM #CompleteDataset
GROUP BY member_casual, month_of_year
ORDER BY month_of_year

-- 10) Determine behavior with day of week
SELECT member_casual, day_of_week,
COUNT(member_casual) AS total_membership_rides
FROM #CompleteDataset
GROUP BY member_casual, day_of_week
ORDER BY day_of_week

-- 11) Determine behavior with location
SELECT TOP 5 member_casual,  station_name,
COUNT(member_casual) AS total_membership_rides
FROM #CompleteDataset
WHERE member_casual LIKE 'member'
GROUP BY member_casual, station_name
ORDER BY total_membership_rides DESC


-- 12)  5) Determine the whole picture of the data
SELECT member_casual, rideable_type, day_of_week, month_of_year, station_name, latitude, longitude, 
COUNT(member_casual) AS total_membership_rides,
AVG(ride_length_min) AS avg_ride_length_min, 
AVG(distance_km) AS avg_distance_km
FROM #CompleteDataset
GROUP BY rideable_type, member_casual, day_of_week, month_of_year, station_name, latitude, longitude

