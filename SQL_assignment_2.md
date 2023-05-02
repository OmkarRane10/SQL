

Q51. Write an SQL query to report the name, population, and area of the big countries. Return the result table in any order.

	Ans -	select name, population, area from world
		    where area >= 3000000 or population >= 25000000;
		    
	    
Q52. Write an SQL query to report the names of the customer that are not referred by the customer with id = 2.

	Ans -	select name from customer
		    where referee_id !=2 or referee_id is NULL;
		    

Q53. Write an SQL query to report all customers who never order anything. Return the result table in any order.

	Ans -	select c.name as customers from customers c left join orders o 
		    on c.id = o.customerId where o.customerId is null;


Q54. Write an SQL query to find the team size of each of the employees.

	Ans - select employee_id, count(team_id) over (partition by team_id) as teamSize from employee order by employee_id;


Q55. Write an SQL query to find the countries where this company can invest.

	Ans - 	SELECT co.name AS country FROM Person p
		 JOIN Country co
		     ON SUBSTRING(phone_number,1,3) = country_code
		 JOIN Calls c
		     ON p.id IN (c.caller_id, c.callee_id)
		GROUP BY country
		HAVING AVG(duration) > (SELECT AVG(duration) FROM Calls);


Q56. Write an SQL query to report the device that is first logged in for each player.

	Ans -	SELECT player_id, device_id from
		(select player_id, device_id, event_date,
		    RANK() OVER(partition by player_id ORDER BY event_date) as rownum
		from Activity order by player_id) tmp 
		where rownum = 1;


Q57. Write an SQL query to find the customer_number for the customer who has placed the largest number of orders.

	Ans -	SELECT customer_number, count(order_number)
		FROM Orders
		GROUP BY customer_number
		ORDER BY COUNT(*) DESC limit 1;


Q58. Write an SQL query to report all the consecutive available seats in the cinema.

	Ans - 	SELECT DISTINCT t1.seat_id
		    FROM Cinema t1 JOIN Cinema t2
		    ON abs(t1.seat_id - t2.seat_id) = 1 
		    AND t1.free = 1 AND t2.free = 1 ORDER BY 1; 


Q59. Write an SQL query to report the names of all the salespersons who did not have any orders related to the company with the name "RED".

	Ans -	SELECT name FROM SalesPerson WHERE sales_id
		NOT IN (
		    SELECT s.sales_id FROM Orders o
		    INNER JOIN SalesPerson s ON o.sales_id = s.sales_id
		    INNER JOIN Company c ON o.com_id = c.com_id
		    WHERE c.name = 'RED');


Q60. Write an SQL query to report for every three line segments whether they can form a triangle.

	Ans -	select x, y, z,
		    CASE WHEN x + y > z and x + z > y and y + z > x THEN 'YES'
			ELSE 'NO' END as triangle
		    from Triangle;


Q61. Write an SQL query to report the shortest distance between any two points from the Point table.

	Ans -	select min(abs(p2.x-p1.x)) as shortest from point p1, point p2
			where p1.x != p2.x;


Q62. Write a SQL query for a report that provides the pairs (actor_id, director_id) where the actor has cooperated with the director at least three times.

	Ans -	select actor_id, director_id from ActorDirector
		    where actor_id = director_id 
		    group by actor_id 
		    having count(actor_id = director_id) >= 3;


Q63. Write an SQL query that reports the product_name, year, and price for each sale_id in the Sales table.

	Ans -	select p.product_name,s.year, s.price 
		    from Product p right join Sales s 
		    on p.product_id = s.product_id;


Q64. Write an SQL query that reports the average experience years of all the employees for each project, rounded to 2 digits.

	Ans -	select project_id, ROUND(avg(experience_years), 2) as avg_years
		from 
		(select p.project_id, p.employee_id, e.experience_years 
		    from Project p left join Employee e
		    on p.employee_id = e.employee_id order by project_id) tmp
		    GROUP BY project_id;


Q65. Write an SQL query that reports the best seller by total sales price, If there is a tie, report them all.

	Ans -	select a.seller_id from
		(select seller_id, sum(price) as total_sales
		    from Sales GROUP BY seller_id) a
		    where a.total_sales = 
		    (select max(b.total_sales) from 
		    (select seller_id, sum(price) as total_sales
		    from Sales GROUP BY seller_id) b);


Q66. Write an SQL query that reports the buyers who have bought S8 but not iPhone. Note that S8 and iPhone are products present in the Product table.

	Ans -	select distinct buyer_id from Sales s 
		    inner join Product p 
		    on s.product_id = p.product_id
		    where p.product_name = 'S8'
		    and 
		    buyer_id not in 
		    (
			select distinct buyer_id from Sales s 
			inner join Product p 
			on s.product_id = p.product_id
			where p.product_name = 'iPhone'
		    );


Q67. You are the restaurant owner and you want to analyse a possible expansion (there will be at least one customer every day).
Write an SQL query to compute the moving average of how much the customer paid in a seven days
window (i.e., current day + 6 days before). average_amount should be rounded to two decimal places.
Return result table ordered by visited_on in ascending order.

	Ans -	with cte as(
		select visited_on,
		    sum(total_amount) over( rows between 6 preceding and current row)as total_sales,
		    round(avg(total_amount) over(rows between 6 preceding and current row), 2)as avg_sales
		    from (
		    select visited_on, sum(amount) as total_amount
		    from Customer group by visited_on)tmp )
		    select * from cte order by visited_on;


Q68. Write an SQL query to find the total score for each gender on each day.
Return the result table ordered by gender and day in ascending order.

	Ans -	select s1.gender, s1.day, sum(s2.score_points) as total from Scores s1 inner join Scores s2
		where s1.gender = s2.gender and s1.day >= s2.day
		group by s1.gender, s1.day
		order by s1.gender, s1.day;


Q69. Write an SQL query to find the start and end number of continuous ranges in the table Logs. Return the result table ordered by start_id.

	Ans -	select min(log_id) as start_id, max(log_id) as end_id from (select l.log_id, (l.log_id - l.row_num) as diff
		      from (select log_id, row_number() over() as row_num from logs) l) l2 group by diff;


Q70. Write an SQL query to find the number of times each student attended each exam. Return the result table ordered by student_id and subject_name.

	Ans -	select st.student_id, st.student_name, s.subject_name, count(e.subject_name) as attended_exams
		    from Students st 
		    inner join Subjects s
		    left join Examinations e
		    on st.student_id = e.student_id and s.subject_name = e.subject_name
		    group by st.student_id, s.subject_name
		    order by st.student_id, s.subject_name;


Q71. Write an SQL query to find employee_id of all employees that directly or indirectly report their work to the head of the company.

	Ans -	select e3.employee_id from Employees e1, Employees e2, Employees e3
		where e1.manager_id = 1 
		    and e2.manager_id = e1.employee_id 
		    and e3.manager_id = e2.employee_id 
		    and e3.employee_id != 1;


Q72. Write an SQL query to find for each month and country, the number of transactions and their total amount, the number of approved transactions and their total amount.

	Ans -	select DATE_FORMAT(trans_date, '%Y-%m') AS month,
		    country,
		    count(state) as number_of_trans,
		    count(if(state='approved',1,null)) as approved_count,
		    sum(amount) as total_amount,
		    sum(if(state='approved',amount,0)) as approved_amount
		from Transactions
		GROUP BY month, country;


Q73. Write an SQL query to find the average daily percentage of posts that got removed after being reported as spam, rounded to 2 decimal places.

	Ans -	SELECT ROUND((sum(percentage)/count(percentage)),2) as Average
		FROM
		(select a.action_date,
		count(DISTINCT b.post_id)/COUNT(DISTINCT a.post_id)*100 as percentage
		from Actions a left join Removals b 
		on a.post_id = b.post_id
		where a.extra = "spam"
		GROUP BY a.action_date ) tmp;


Q74. Write an SQL query to report the fraction of players that logged in again on the day after the day they first logged in, rounded to 2 decimal places.

	Ans -	with cte as 
		(select player_id, MIN(event_date) as min_event_date
		from Activity GROUP BY player_id
		)
		select 
		round(COUNT(DISTINCT c.player_id)/ (select COUNT(DISTINCT player_id) from Activity),2) as fraction
		from cte c inner join Activity a
		on c.player_id = a.player_id
		and DATEDIFF(c.min_event_date, a.event_date)= -1;


Q75. Write an SQL query to report the fraction of players that logged in again on the day after the day they first logged in, rounded to 2 decimal places.

	Ans -	with cte as 
		(select player_id, MIN(event_date) as min_event_date
		from Activity GROUP BY player_id
		)
		select 
		round(COUNT(DISTINCT c.player_id)/ (select COUNT(DISTINCT player_id) from Activity),2) as fraction
		from cte c inner join Activity a
		on c.player_id = a.player_id
		and DATEDIFF(c.min_event_date, a.event_date)= -1;


Q76. Write an SQL query to find the salaries of the employees after applying taxes. Round the salary to the nearest integer.

	Ans -	SELECT 
		    t1.company_id,
		    t1.employee_id,
		    t1.employee_name,
		    ROUND(CASE WHEN t2.max_sal >= 1000 AND t2.max_sal <= 10000 then salary * 0.76
			WHEN t2.max_sal > 10000 THEN salary * 0.51 
			Else salary end, 0) as salary
		FROM Salaries as t1 JOIN (SELECT company_id, MAX(salary) as max_sal FROM Salaries GROUP BY 1) as t2
		ON t1.company_id = t2.company_id;


Q77. Write an SQL query to evaluate the boolean expressions in Expressions table. Return the result table in any order.

	Ans -	select 
		    e.left_operand,
		    e.operator,
		    e.right_operand,
		    CASE
		    when e.operator = '<' then if(l.value < r.value,'true','false')
		    when e.operator = '>' then if(l.value > r.value,'true','false')
		    else if(l.value = r.value,'true','false')
		    end as value
		    from Expressions e 
		    left join Variables l on e.left_operand = l.name
		    left join Variables r on e.right_operand = r.name;


Q78. Write an SQL query to find the countries where this company can invest.

	Ans - 	SELECT co.name AS country FROM Person p
		 JOIN Country co
		     ON SUBSTRING(phone_number,1,3) = country_code
		 JOIN Calls c
		     ON p.id IN (c.caller_id, c.callee_id)
		GROUP BY country
		HAVING AVG(duration) > (SELECT AVG(duration) FROM Calls);
		
		
Q79. Write a query that prints a list of employee names (i.e.: the name attribute) from the Employee table in alphabetical order.

	Ans - select DISTINCT name from Employee order by name ;


Q80. Assume you are given the table below containing information on user transactions for particular products.
Write a query to obtain the year-on-year growth rate for the total spend of each product for each year.
Output the year (in ascending order) partitioned by product id, current year's spend, previous year's
spend and year-on-year growth rate (percentage rounded to 2 decimal places).

Ans -	select 
	    EXTRACT(year from transaction_date) as year_id,
	    product_id,
	    spend as current_year_spend,
	    lag(spend) over() as prev_year_spend,
	    round(((spend/lag(spend) over() * 100) - 100), 2)as yoy
	from transactions;


Q81. Write a SQL query to find the number of prime and non-prime items that can be stored in the 500,000 square feet warehouse.
Output the item type and number of items to be stocked.

	Ans -   select item_type,
	    	count(item_type) as item_count,
	   	 sum(square_footage) as total_space
		from inventory GROUP BY item_type;


Q82. Assume you have the table below containing information on Facebook user actions. Write a query to obtain the active user retention in July 2022.
Output the month (in numerical format 1, 2, 3) and the number of monthly active users (MAUs).

















		    
