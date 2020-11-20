[595. Big Countries](https://leetcode.com/problems/big-countries/)
```sql
select name, population, area from world where area > 3000000 or population > 25000000
```

[620. Not Boring Movies](https://leetcode.com/problems/not-boring-movies/)
```sql
select * from cinema where (id % 2) = 1 and description != 'boring' order by rating desc
```

[627. Swap Salary](https://leetcode.com/problems/swap-salary/)
```sql
UPDATE salary
SET sex = 
    (CASE sex
        WHEN 'm' THEN 'f'
        ELSE 'm'
    END)
```

