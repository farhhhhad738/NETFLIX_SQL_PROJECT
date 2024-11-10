# NETFLIX_MOVIES  AND TV SHOW DATA ANALYSIS BY USING SQL POSTGRE(PGADMIN4)

 ![netflix logo](https://github.com/farhhhhad738/NETFLIX_SQL_PROJECT/blob/main/Netflix_Symbol_RGB.png)

# 
this project involves a comprehensive analysis ofnnetflix movies and tv show using sql postgre.The goal is to extract values from insight and answer different business question
based on the dataset. the following redme provide a detail acount of the project objective

## objective
analyze the distribution of content types( movie vs TV show)
identify the most common rating for tv show
list and analyze content based on release year,country and duration
explore and categories content based on specific criteria and keywords

# dataset
the source of dataset for this project is kaggle:
dataset link: https://www.kaggle.com/datasets/shivamb/netflix-shows?resource=download

## schema


## 15 business problem
''' sql
q1.count the no of movies vs tv shows
select count(*) as "total content of movies vs tv shows" from netflix
group by typess
'''

''' sql
-- q2. find the most common rating for movies and tv shows.
select typess,rating from netflix
;
select typess,rating from
(
select typess,rating,count(*),
RANK() over(partition by typess order by count(*)) as ranking from netflix
group by 1,2
 ) as t1
 where ranking=1;
'''

'''
-- q3. list all the movies released in specific year.alter
select * from netflix
where typess='Movie' and release_year=2020
'''

'''
-- q4. find the top country with most content on netflix.

select distinct unnest(string_to_array(country,','))as new_country ,count(show_id) as total_content
from netflix group by 1 order by 2 desc limit 5
''''

'''
-- Q5.identify the longest movie?
select * from  netflix where typess='Movie' and 
duration=(select max(duration) from netflix)

'''
-- q6.find content added in the last five year

select * from netflix where TO_DATE(date_added,'month,dd,yyyy')>= current_date- interval '5 years'

-- q7.find all the movies/tv shows by 'Rajiv chilaka'
select * from  netflix where director Ilike '%Rajiv chilaka%

-- q8. list all tv shows with more than 5 seasons
select duration,typess from netflix where typess='TV Show' and split_part(duration,' ',1):: numeric >5


-- q9. find the number of content in each genre
select unnest(string_to_array(listed_in,','))as genre,count(show_id) as total_content
from  netflix group by 1

-- q10. find each year and average content release by india on netflix.return top 5 year with highest
-- averaage content

select TO_DATE(date_added,'Month DD,YYYY') as daate from netflix

select extract(year from TO_DATE(date_added,'Month DD,YYYY'))as year,
count(*) as yearly_content,
round
(count(*)::numeric/(select count(*) from netflix where country = 'India')::numeric *100,2)
as avg_content_per_year
from netflix 
where country='India' 
group by 1

-- q11.list all the movie which are documentary
select * from netflix where  listed_in ilike '%documentaries%'


-- q12. find all content without a director.
select * from netflix where director is null


--q13.how many film were produced by using salman khan as actor in last 10 years
select * from netflix where castss ilike '%salman khan%' and release_year>extract(year from current_date)-10

-- q14. find the top 10 actors who have appeared in highest movie which are produced in india

select
-- show_id,
-- castss,
unnest(string_to_array(castss,',')) as actors,
count(*) as total_content from netflix where country ilike '%India%'
group by 1

