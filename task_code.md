1.	 -- Month

2.	-- Product Name

3.	-- Variants

4.	-- Sold Quantity

5.	-- Gross Price per Item

6.	-- Gross Price Total
7.	
8.	
9.	select 

10.		s.date, s.product_code, p.product, p.variant, s.sold_quantity, g.      gross_price,

11.		g.gross_price*s.sold_quantity as gross_price_total

12.	from fact_sales_monthly s

13.	Join dim_product p 

14.		on s.product_code = p.product_code

15.	Join fact_gross_price g 

16.		on p.product_code = g.product_code and g.fiscal_year = get_fiscal_year(s.date)

17.	where 	customer_code = 90002002 and

18.			get_fiscal_year(date) = 2021

19.	order by date

20.	limit 10000;




2.  SELECT 

	s.date,

    ROUND(sum(g.gross_price*s.sold_quantity),2) as gross_price_total

FROM fact_sales_monthly s

Join fact_gross_price g

	on s.product_code = s.product_code and 

    g.fiscal_year = get_fiscal_year(s.date)

WHERE customer_code = 90002002

group by s.date

order by s.date asc;



3.  CREATE DEFINER=`root`@`localhost` PROCEDURE `get_market_badge`(

IN in_market varchar(50),

IN in_fiscal_year year,

OUT out_badge varchar(50)
)
BEGIN
	declare qty int default 0;
    
	Select sum(s.sold_quantity) into qty  

	from fact_sales_monthly s

	join dim_customer c on s.customer_code = c.customer_code

	where get_fiscal_year(s.date) = in_fiscal_year and c.market = in_market
	group by c.market;
    
    if qty > 5000000 then set out_badge = 'Gold';
    else set out_badge = 'Silver';


    end if;
END



4. 1(market) CREATE DEFINER=`root`@`localhost` PROCEDURE `get_top_n_markets_by_net_sales`(

	in_fiscal_year INT,

    in_top_n INT
)
BEGIN

SELECT

market,

round(sum(net_sales)/1000000,2) as net_sales_mln

FROM gdb0041.net_sales

where fiscal_year = in_fiscal_year

group by market

order by net_sales_mln desc

limit in_top_n;

END


2CREATE DEFINER=`root`@`localhost` PROCEDURE `get_top_n_products_by_net_sales`(

	in_fiscal_year int,

    in_top_n int
)
BEGIN
    `SELECT

            product,

            round(sum(net_sales)/1000000,2) as net_sales_mln

            FROM gdb0041.net_sales


            where fiscal_year = in_fiscal_year

            group by product

            order by net_sales_mln desc

            limit in_top_n;`
END



3CREATE DEFINER=`root`@`localhost` PROCEDURE `get_top_n_customers_by_net_sales`(

	in_market varchar(50),

    in_fiscal_year int,

    in_top_n int
)
BEGIN

SELECT
        c.customer,

        round(sum(net_sales)/1000000,2) as net_sales_mln

        FROM gdb0041.net_sales n

        join dim_customer c

        on c.customer_code = n.customer_code

        where fiscal_year = in_fiscal_year and n.market = in_market

        group by c.customer

        order by net_sales_mln desc

        limit in_top_n;
END



5. with CTE1 as (

	SELECT

	c.customer,

	round(sum(net_sales)/1000000,2) as net_sales_mln

	FROM gdb0041.net_sales n

	join dim_customer c

	on c.customer_code = n.customer_code

	where fiscal_year = 2021

	group by c.customer)

select *,

net_sales_mln*100/sum(net_sales_mln) over() as pct

from CTE1

order by net_sales_mln desc;




6. with CTE1 as (
	SELECT

	c.customer,

    c.region,

	round(sum(net_sales)/1000000,2) as net_sales_mln

	FROM gdb0041.net_sales n

	join dim_customer c

	on c.customer_code = n.customer_code

	where fiscal_year = 2021

	group by c.customer, c.region)


select *,

net_sales_mln*100/sum(net_sales_mln) over(partition by region) as pct

from CTE1

order by region, net_sales_mln desc;




7. CREATE DEFINER=`root`@`localhost` PROCEDURE `get_top_n_products_per_division_by_qty_sold`(

	in_fiscal_year int,

    in_top_n int
)
BEGIN
			with cte1 as (

					select

						p.division as Division,

						p.product as Products,

						sum(s.sold_quantity) as Total_qty

					from fact_sales_monthly s

					join dim_product p on s.product_code = p.product_code

					where fiscal_year = in_fiscal_year

					group by p.product),

				cte2 as (select *,

						dense_rank() over(partition by Division order by Total_qty desc) as dnk

						from cte1)

			select * from cte2

			where dnk <= in_top_n;
            



8. with cte1 as (

		select

			c.market,

			c.region,

			round(sum(gross_price_total)/1000000,2) as gross_sales_mln

			from gross_sales s

			join dim_customer c

			on c.customer_code=s.customer_code

			where fiscal_year=2021

			group by market

			order by gross_sales_mln desc
		),

		cte2 as (

			select *,

			dense_rank() over(partition by region order by gross_sales_mln desc) as drnk

			from cte1
		)

	select * from cte2 where drnk<=2;
