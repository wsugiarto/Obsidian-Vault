# DIVIDE

```sql
with courses as(  
select distinct courseid  
from professor as p, teaches as t  
where p.profid = 1 and p.profid = t.profid  
)  
  
select e.studentid, studentname  
from enroll as e, courses as c, student as s  
where e.courseid = c.courseid and s.studentid = e.studentid  
group by e.studentid, studentname having count(distinct e.courseid) = (select count(*) from courses)
```

Above removes students who have not taken all courses. 