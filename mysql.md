<!--数据库笔试面试题目汇总-->

1. year(curdate())：在当前日期中取出年份
2. top limit
3. new table crate table B select * from A


#学生人数
	select count(*) from Student;


#1号课程的平均成绩
	select  Course.cno, cname, avg(grade) from Student, sc, Course where Student.sno = sc.sno and sc.cno = Course.cno and Course.cno = 1;
    
#1号课程的最高成绩
	select  sname, max(grade) from Student, sc, Course where Student.sno = sc.sno and sc.cno = Course.cno and Course.cno = 1;

#每个学生的平均成绩
	select  sname,  avg(grade) from Student, sc, Course where Student.sno = sc.sno and sc.cno = Course.cno group by Student.sno order by avg(grade) desc;

#每门课的评价成绩
	select  cname, avg(grade) from Student, sc, Course where Student.sno = sc.sno and sc.cno = Course.cno group by sc.cno;

#每门课的最高分
	select cname, max(grade) from Student, sc, Course where Student.sno = sc.sno and sc.cno = Course.cno group by (sc.cno);

#每门课成绩最好的那个学生的详细信息
	select sno, sname, cname, grade, dname, max_grade from (select Student.sno, sname, Course.cno no, Course.cname, grade,dname from Student, Course, Department, sc where Student.sno = sc.sno and Course.cno = sc.cno and Student.deptno = Department.deptno ) t1 join (select cno, max(grade) max_grade from sc group by cno) t2 on (t1.no = t2.cno and t1.grade = t2.max_grade);
