# SQL report showing movie trends

![](IMDB Image.jpg)

## Objective
 - Find top-rated genres over time.
 - Compare critics’ vs. audience scores.
 - Identify highest-grossing movies by decade.

## SQL Queries

## Find top-rated genres over time.
```SQL
select Top 10 Series_Title, genre, released_year, Round(IMDB_Rating, 2) as IMDB_ratings
from Movie_Dataset
order by IMDB_Rating Desc, Released_Year
```

## Compare critics’ vs. audience scores for each decade
```SQL
with CTE as (
select *,
case
when Released_Year between 1920 and 1929 then 'Decade_1920'
when Released_Year between 1930 and 1939 then 'Decade_1930'
when Released_Year between 1940 and 1949 then 'Decade_1940'
when Released_Year between 1950 and 1959 then 'Decade_1950'
when Released_Year between 1960 and 1969 then 'Decade_1960'
when Released_Year between 1970 and 1979 then 'Decade_1970'
when Released_Year between 1980 and 1989 then 'Decade_1980'
when Released_Year between 1990 and 1999 then 'Decade_1990'
when Released_Year between 2000 and 2009 then 'Decade_2000'
when Released_Year between 2010 and 2019 then 'Decade_2010'
when Released_Year = 2020 then 'Decade_2020'
end as Decades
from Movie_Dataset
)

select decades, Round(AVG(imdb_rating),2) * 10 as Audience_score, AVG(meta_score) as Critic_score
from CTE
group by Decades
order by Decades
```

##Identify highest-grossing movies by decade.
```SQL
with CTE as (
select *,
case
when Released_Year between 1920 and 1929 then 'Decade_1920'
when Released_Year between 1930 and 1939 then 'Decade_1930'
when Released_Year between 1940 and 1949 then 'Decade_1940'
when Released_Year between 1950 and 1959 then 'Decade_1950'
when Released_Year between 1960 and 1969 then 'Decade_1960'
when Released_Year between 1970 and 1979 then 'Decade_1970'
when Released_Year between 1980 and 1989 then 'Decade_1980'
when Released_Year between 1990 and 1999 then 'Decade_1990'
when Released_Year between 2000 and 2009 then 'Decade_2000'
when Released_Year between 2010 and 2019 then 'Decade_2010'
when Released_Year = 2020 then 'Decade_2020'
end as Decades
from Movie_Dataset
)
, high_earners as (
select *, 
Dense_rank() over(partition by decades order by gross desc, series_title) as High_gross
from CTE
)
select Decades, Released_Year, series_title, gross
from High_earners
where High_gross = 1
order by decades
```
