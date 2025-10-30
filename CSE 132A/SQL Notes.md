# DIVIDE

select e.studentid, studentname  
from enroll as e, courses as c, student as s  
where e.courseid = c.courseid and s.studentid = e.studentid  
group by e.studentid, studentname having count(distinct e.courseid) = (select count(*) from courses)

Above removes students who have not taken all courses. 