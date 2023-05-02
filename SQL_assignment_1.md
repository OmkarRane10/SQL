

Q1. Query all columns for all American cities in the CITY table with populations larger than 100000.
The CountryCode for America is USA.

	Ans - select * from CITY where COUNTRYCODE = 'USA' and POPULATION > 100000;


Q2. Query the NAME field for all American cities in the CITY table with populations larger than 120000.
The CountryCode for America is USA.

	Ans - select NAME from CITY where COUNTRYCODE = 'USA' and POPULATION > 120000;


Q3. Query all columns (attributes) for every row in the CITY table.

	Ans - select * from CITY;


Q4. Query all columns for a city in CITY with the ID 1661.

	Ans - select * from CITY where ID=1661;


Q5. Query all attributes of every Japanese city in the CITY table. The COUNTRYCODE for Japan is JPN.

	Ans - select * from CITY where COUNTRYCODE = 'JPN';


Q6. Query the names of all the Japanese cities in the CITY table. The COUNTRYCODE for Japan is
JPN.

	Ans - select DISTRICT from CITY where COUNTRYCODE = 'JPN';


Q7. Query a list of CITY and STATE from the STATION table.

	Ans - select City, State from Station;
	

Q8. Query a list of CITY names from STATION for cities that have an even ID number. Print the results in any order, but exclude duplicates from the answer.

	Ans - select Distinct City from Station where Id%2 = 0;


Q9. Find the difference between the total number of CITY entries in the table and the number of distinct CITY entries in the table.

	Ans - select (count(City) - count(distinct(City))) as difference from Station;


Q10. Query the two cities in STATION with the shortest and longest CITY names, as well as their respective lengths (i.e.: number of characters in the name).
If there is more than one smallest or largest city, choose the one that comes first when ordered alphabetically.

	Ans - select City, length(City) from Station order by length(name) asc limit 1;
	      select City, length(City) from Station order by length(name) desc limit 1;


Q11. Query the list of CITY names starting with vowels (i.e., a, e, i, o, or u) from STATION. Your result
cannot contain duplicates.

	Ans - select distinct City from Station where City like 'a%' OR City like 'e%' OR City like 'i%' OR City like 'o%' OR City like 'u%';


Q12. Query the list of CITY names ending with vowels (a, e, i, o, u) from STATION. Your result cannot contain duplicates.

	Ans - select distinct City from Station where City like '%a' OR City like '%e' OR City like '%i' OR City like '%o' OR City like '%u';


Q13. Query the list of CITY names from STATION that do not start with vowels. Your result cannot contain duplicates.

	Ans - select City from Station where City not like 'a%' OR City not like 'e%' OR City not like 'i%' OR City not like 'o%' OR City not like 'u%';


Q14. Query the list of CITY names from STATION that do not end with vowels. Your result cannot contain duplicates.

	Ans - select City from Station where City not like '%a' OR City not like '%e' OR City not like '%i' OR City not like '%o' OR City not like '%u';


Q15. Query the list of CITY names from STATION that either do not start with vowels or do not end with vowels. Your result cannot contain duplicates.

	Ans - SELECT DISTINCT CITY FROM STATION WHERE (CITY NOT IN (SELECT CITY FROM STATION WHERE CITY LIKE '%a' OR CITY LIKE '%e' OR CITY LIKE '%i' OR CITY LIKE '%o' OR CITY LIKE '%u')) OR (CITY NOT IN (SELECT CITY FROM STATION WHERE CITY LIKE 'A%' OR CITY LIKE 'E%' OR CITY LIKE 'I%' OR CITY LIKE 'O%' OR CITY LIKE 'U%'));


Q16. Query the list of CITY names from STATION that do not start with vowels and do not end with vowels. Your result cannot contain duplicates.

	Ans - SELECT DISTINCT CITY FROM STATION WHERE (CITY NOT IN (SELECT CITY FROM STATION WHERE CITY LIKE '%a' OR CITY LIKE '%e' OR CITY LIKE '%i' OR CITY LIKE '%o' OR CITY LIKE '%u')) AND (CITY NOT IN (SELECT CITY FROM STATION WHERE CITY LIKE 'A%' OR CITY LIKE 'E%' OR CITY LIKE 'I%' OR CITY LIKE 'O%' OR CITY LIKE 'U%'));


Q17. Write an SQL query that reports the products that were only sold in the first quarter of 2019. That is,between 2019-01-01 and 2019-03-31 inclusive.

	Ans - select s.product_id,p.product_name from Product p inner join Sales s on p.product_id = s.product_id where s.sale_date between 2019-01-01 and 2019-03-31 and s.sale_date NOT between 2019-04-01 and 2019-12-31;


Q18. Write an SQL query to find all the authors that viewed at least one of their own articles. Return the result table sorted by id in ascending order.

	Ans - select distinct viewer_id as id from view where author_id = viewer_id order by viewer_id;


Q19. Write an SQL query to find the percentage of immediate orders in the table, rounded to 2 decimal places.

	Ans - select ifnull(round((select count(*) from delivery where order_date = customer_pref_delivery_date)/ (count(delivery_id)) * 100, 2), 0) as immediate_percentage from delivery;


Q20. Write an SQL query to find the ctr of each Ad. Round ctr to two decimal points.
     Return the result table ordered by ctr in descending order and by ad_id in ascending order in case of a tie.

	Ans - select ad_id, ifnull(round(sum(action = 'Clicked')/sum(action != 'Ignored') *100 ,2) ,0) as ctr from Ads group by ad_id order by ad_id asc, ctr desc;


Q21. Write an SQL query to find the team size of each of the employees. Return result table in any order.

	Ans - select employee_id, count(*) over(partition by team_id) as team_size from Employee order by team_size desc;


Q22. Write an SQL query to find the type of weather in each country for November 2019.
The type of weather is:
● Cold if the average weather_state is less than or equal 15,
● Hot if the average weather_state is greater than or equal to 25, and
● Warm otherwise.
Return result table in any order

	Ans - select ct.country_name,
		case
		    when AVG(w.weather_state*1.0) <=15 then 'cold'
		    when AVG(w.weather_state*1.0) >=25 then 'Hot'
		    else 'warm'
		end as weather_type
	    from Countries ct inner join Weather w on ct.country_id = w.country_id 
	    where w.day between '2019-11-01' and '2019-11-30' GROUP BY ct.country_name;


Q23. Write an SQL query to find the average selling price for each product. average_price should be
rounded to 2 decimal places.
Return the result table in any order.

	Ans - with cte as (SELECT  p.product_id, p.price, o.units FROM Prices p inner JOIN UnitsSold o on o.product_id = p.product_id
	   	 where o.purchase_date BETWEEN p.start_date AND p.end_date)
		    select product_id, ROUND(sum(price * units)/sum(units) , 2) as average_price from cte GROUP BY product_id;


Q24. Write an SQL query to report the first login date for each player.
Return the result table in any order.

	Ans - select tmp.player_id,tmp.event_date as first_login from (select *,
		row_number() over(partition by player_id ) as row_num
	     from Activity) tmp where tmp.row_num = 1; 

Q25. Write an SQL query to report the device that is first logged in for each player. Return the result table in any order.

	Ans - select tmp.player_id, tmp.device_id from (select *, 
	    		row_number() over(partition by player_id) as row_num from Activity) as tmp where tmp.row_num = 1;


Q26. Write an SQL query to get the names of products that have at least 100 units ordered in February 2020 and their amount.

	Ans - select p.product_name, sum(o.unit) as unit_ordered from Products p inner join Orders o on p.product_id = o.product_id
	    where o.order_date BETWEEN '2020-02-01' and '2020-02-28' GROUP BY p.product_id having sum(unit) >= 100;


Q27. Write an SQL query to find the users who have valid emails.
A valid e-mail has a prefix name and a domain where:
● The prefix name is a string that may contain letters (upper or lower case), digits, underscore
'_', period '.', and/or dash '-'. The prefix name must start with a letter.
● The domain is '@leetcode.com'.

	Ans - select * from users where email REGEXP '^[a-zA-Z][a-zA-Z0-9\_\.\-]*@leetcode.com$';


Q28. Write an SQL query to report the customer_id and customer_name of customers who have spent at least $100 in each month of June and July 2020.

	Ans - select o.customer_id, c.name from Customers c,Product p, Orders o where c.customer_id = o.customer_id
		    and p.product_id = o.product_id
		    GROUP BY o.customer_id
		    having 
		(
		    sum(case when o.order_date like '2020-06%' then o.quantity*p.price else 0 end) >= 100
		    and
		    sum(case when o.order_date like '2020-07%' then o.quantity*p.price else 0 end) >= 100
		);


Q29. Write an SQL query to report the distinct titles of the kid-friendly movies streamed in June 2020. Return the result table in any order.

	Ans - select distinct(c.title) as title from Content c inner join TVProgram tvp on c.content_id = tvp.content_id
		    where c.Kids_content = 'Y'
		    and c.content_type = 'Movies'
		    and tvp.program_date BETWEEN '2020-06-01' and '2020-06-30' ;


Q30. Write an SQL query to find the npv of each query of the Queries table. Return the result table in any order.

	Ans - select q.id, q.year, COALESCE(n.npv, 0) from Queries q 
	    		LEFT join NPV n on q.id = n.id and q.year = n.year;


Q31. Write an SQL query to find the npv of each query of the Queries table. Return the result table in any order.

	Ans - select q.id, q.year, COALESCE(n.npv, 0) from Queries q 
	    		LEFT join NPV n on q.id = n.id and q.year = n.year;


Q32. Write an SQL query to show the unique ID of each user, If a user does not have a unique ID replace just show null. Return the result table in any order.

	Ans - select COALESCE(eu.unique_id, 'null'), e.name from employeeUNI eu 
			RIGHT join employees e on eu.id = e.id order by eu.unique_id;


Q33. Write an SQL query to report the distance travelled by each user. Return the result table ordered by travelled_distance in descending order,
if two or more users travelled the same distance, order them by their name in ascending order.

	Ans - select u.name, COALESCE(sum(r.distance), 0) as travelled_distance from users u
	    		left join rides r on r.user_id = u.id
	    		group by u.name order by travelled_distance desc, name asc;


Q34. Write an SQL query to get the names of products that have at least 100 units ordered in February 2020 and their amount.

	Ans - select a.product_name, sum(unit) as unit
			from Products a
			left join Orders b
			on a.product_id = b.product_id
			where b.order_date between '2020-02-01' and '2020-02-29'
			group by a.product_id
			having sum(unit) >= 100;


Q35. Write an SQL query to:
● Find the name of the user who has rated the greatest number of movies. In case of a tie,
return the lexicographically smaller user name.
● Find the movie name with the highest average rating in February 2020. In case of a tie, return
the lexicographically smaller movie name.

	Ans - select u_name as results from 
			(
			    select u.name as u_name, count(*) as counts from users u inner join movieRating m
			    on u.user_id = m.user_id group by m.user_id ORDER BY counts desc , u_name ASC limit 1
			) tmp
			UNION ALL
			select movie as results FROM
			(
			select m.title as movie, round(AVG(r.rating), 1) as avg_count from movies m inner join movieRating r 
			    on m.movie_id = r.movie_id where created_at between '2020-02-01' and '2020-02-28'
			    GROUP BY r.movie_id order by avg_count desc, movie asc limit 1
			) tmp2;


Q36. Write an SQL query to report the distance travelled by each user. Return the result table ordered by travelled_distance in descending order,
if two or more users travelled the same distance, order them by their name in ascending order.

	Ans - select u.name,COALESCE(sum(r.distance), 0) as distance_travelled from users u 
	    		left join rides r on r.user_id = u.id
		        group by u.name order by distance_travelled desc, name ASC;


Q37. Write an SQL query to show the unique ID of each user, If a user does not have a unique ID replace just show null.

	Ans - select COALESCE(eu.unique_id, null) as employee_id, e.name from Employees e
		     left join EmployeeUNI eu 
		     on eu.id = e.id ORDER BY employee_id;


Q38. Write an SQL query to find the id and the name of all students who are enrolled in departments that no longer exist.

	Ans - select s.id, s.name from 
		students s left join departments d 
		on s.department_id = d.id
	    	where d.id is null;


Q39. Write an SQL query to report the number of calls and the total call duration between each pair of distinct persons (person1, person2) where person1 < person2.

	Ans -	with tmptab as (
		(
		    select from_id as person_1, to_id as person_2, duration
		    from Calls
		)
		UNION ALL
		(
		    select to_id as person_1, from_id as person_2, duration
		    from Calls 
		))
		select person_1, person_2, count(*) as call_count,
		    sum(duration) from tmptab 
		    where person_1 < person_2
		    group by person_1,person_2;


Q40. Write an SQL query to find the average selling price for each product. average_price should be rounded to 2 decimal places.

	Ans - 	with tmptab as ( select p.product_id, p.price, u.units from prices p inner join unitsold u on p.product_id = u.product_id
			where u.purchase_date between p.start_date and p.end_date )
		    	select product_id, round(sum(price * units)/sum(units), 2) as avgprice from tmptab group by product_id;


Q41. Write an SQL query to report the number of cubic feet of volume the inventory occupies in each warehouse.

Ans -	select w.name as warehouse, sum(w.units * p.vol) as volume
	    from warehouse w inner JOIN
	    (select product_id, width*length*height as vol from products) p 
	    on w.product_id = p.product_id
	    group by warehouse; 


Q42. Write an SQL query to report the difference between the number of apples and oranges sold each day. Return the result table ordered by sale_date.

	Ans -	select a.sale_date, (a.sold_num - b.sold_num) as diff_apple_or
		    from sales a inner join sales b 
		    on a.sale_date = b.sale_date
		    where a.fruit = 'apples' and b.fruit='oranges';


Q43. Write an SQL query to report the fraction of players that logged in again on the day after the day they first logged in, rounded to 2 decimal places.

	Ans -	with cte as 
		(
		    select player_id, MIN(event_date) as start_date
		    from activity group by player_id )

		select round((count(DISTINCT c.player_id) / (select count(DISTINCT player_id) from activity)), 2) as fraction
		    from cte c inner join activity a 
		    on c.player_id = a.player_id 
		    and DATEDIFF(c.start_date, a.event_date)=-1;


Q44. Write an SQL query to report the managers with at least five direct reports.

	Ans -	select a.name from employee a inner join employee b 
		    on a.id = b.managerid group by a.name
		    HAVING count(distinct b.id) >= 5;


Q45. Write an SQL query to report the respective department name and number of students majoring in
each department for all departments in the Department table (even ones with no current students).
Return the result table ordered by student_number in descending order. In case of a tie, order them by
dept_name alphabetically.

	Ans -	select dept_name,COALESCE(count( DISTINCT student_id), 0) as student_count
		    from department d left join student s
		    on d.dept_id = s.dept_id
		    group by dept_name
		    order by student_count desc, dept_name;


Q46. Write an SQL query to report the customer ids from the Customer table that bought all the products in the Product table.

	Ans -	SELECT customer_id FROM customer
		GROUP BY customer_id
		HAVING COUNT( DISTINCT product_key) = (SELECT COUNT(*) FROM product);


Q47. Write an SQL query that reports the most experienced employees in each project. In case of a tie, report all employees with the maximum number of experience years.

	Ans - 	select project_id, employee_id from 
		(select p.project_id, e.employee_id,
		    DENSE_RANK() over (partition by p.project_id order by e.experience_years desc) as rnk
		from project p inner join employee e
		    on p.employee_id = e.employee_id) x where rnk = 1;


Q48. Write an SQL query that reports the books that have sold less than 10 copies in the last year,
excluding books that have been available for less than one month from today. Assume today is 2019-06-23.

	Ans - 	select books.book_id, name from books join orders
		    on books.book_id = orders.book_id
		    where available_from < '2019-05-23'
		    and dispatch_date between '2018-06-23' and '2019-06-23'
		    group by books.book_id
		    having sum(quantity) < 10;


Q49. Write a SQL query to find the highest grade with its corresponding course for each student. In case of a tie, you should find the course with the smallest course_id.
Return the result table ordered by student_id in ascending order.

	Ans -	select student_id, course_id, grade from 
		(
		select student_id, course_id, grade,
		    row_number() OVER(partition by student_id order by grade desc, course_id) as rnk
		    from enrollments
		) x where rnk = 1;


Q50. Write an SQL query to find the winner in each group. Return the result table in any order.

	Ans -	select group_id,player_id from 
		(select group_id,player_id,sum((
		    case when player_id = first_player then first_score
			 when player_id = second_player then second_score
			 end
		)) as totalScores
		from Players p,Matches m
		where p.player_id = m.first_player
		or p.player_id = m.second_player
		group by group_id,player_id
		order by group_id,totalScores desc,player_id) as temp
		group by group_id
		order by group_id,totalScores desc,player_id;










