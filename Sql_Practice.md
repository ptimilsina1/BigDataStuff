==**SQL Exercises:**==

**1Write a query to Get a List of Employee who have a one part name.**   
select concat(first_name,' ',last_name)  from employees where last_name IS NULL or last_name = '' or  first_name IS NULL or  first_name = '' ;  

**Write a query to Get a List of Employee who have a three part name. **  
Select concat(first_name,' ',last_name) as 'full_name' from employees where (length(concat(first_name,' ',last_name) )-length(replace(concat(first_name,' ',last_name) ,' ','')))>2 ;  

**Write a query to get a list of Employee who have a First Middle Or last name as Ram, and not any thing else **  
select concat(first_name,' ',last_name) from employees where  first_name like "Ram%" or last_name like "Ram%" ;  

**Write a query which gives employee types in the organization.**  
select concat(first_name,' ',last_name) as "full_name", departments.dept_name from employees join dept_emp on employees.emp_no=dept_emp.emp_no 
join departments on dept_emp.dept_no=departments.dept_no limit 10;   

**Write a Query to get all employees where reminder of employee number by 10 is a power of two **  
select * from employees where ((emp_no%10)%2)=0;  

**Write a query to get all employees sorted by Service Type and Name within a given Center **  
	select concat(first_name,' ',last_name) as "full_name", departments.dept_name from employees 	join dept_emp on employees.emp_no=dept_emp.emp_no  join departments on 	dept_emp.dept_no=departments.dept_no order by last_name, dept_name limit 10;  

**Write a query to find the all the names which contain the word or a part of a word Suresh, sort the result in the order of similarity. Ex : Suresh, Sures, Sure, Sur, Su, S (Names containing the word Suresh, later names containing Sures and so on) 
Display all names from tblEmployees by appending it with INDIAN at the end if the name starts from A-M, for names starting from N-Z append AMERICAN at the end. Ex : Shyam should display as ShyamAMERICAN and Abdul should display as AbdulINDIAN **  
select concat(first_name,'Indian')  from employees where first_name REGEXP '^[A-M]' order by first_name;
select concat(first_name,'American')  from employees where first_name REGEXP '^[N-Z]' order by first_name;  

**Write a query to find the name(s) having the largest number of characters in it. (Hint: Use aggregate functions) **  
Select concat(first_name,' ',last_name) as 'full_name',(length(concat(first_name,' ',last_name))) as 'length of name' from employees order by (length(concat(first_name,last_name))) desc limit 1;  

**Write a query to list all the employees whose name starts and ends with same character. **  
Select concat(first_name,' ',last_name) from employees where left(first_name,1) =right(last_name,1);  

**Write a query to list all employees whose first and second character in their names are similar. **  
Select concat(first_name,' ',last_name) from employees where left(first_name,1) =right(left(first_name,2),1);  

**Write a query to get Max salary and Min salary of all the employees. (NOTE: You can skip any two of the above questions if you execute the query without aggregate functions) **  
select max(salary),min(salary) from  salaries;  

**Write a query to display the current date in all possible formats**  
SELECT CURDATE();
SELECT CURDATE() + 0;  

**Write a Query to List out all employees where the present basic is perfectly rounded of to 100. <br> Ex : If Basic of A is 2011, Basic of B is 2100 , Basic of C is 2101 and Basic of D is 2200 . Then Only B and D should be displayed **  
Select Concat(first_name,' ',last_name) as "full_name",salaries.salary from employees join salaries on employees.emp_no=salaries.emp_no where salaries.salary%100=0;  

**Write a query to find out employees whose names have Leading or Trailing spaces **  
Select first_name from employees where rtrim(first_name)=' ' or ltrim(first_name)=' ';  

**Write a update query to remove trailing spaces from the employee names. Ex: If the employee name is Naseeruddin Shah , then after running the update query the name should be Naseeruddin Shah.(without any spaces at the end) **  
Update employees set
first_name= ltrim(first_name), 
last_name= ltrim(last_name);  

**Write a similar update query to remove the leading spaces from the employee names **  
UPDATE employees set
first_name= ltrim(first_name), 
last_name= ltrim(last_name);  


**Write a query to get the top 4 salaried employees**  
select concat(first_name,' ',last_name) as 'full_name' ,salaries.salary from employees join salaries on employees.emp_no=salaries.emp_no order by salaries.salary desc limit 4;  

**Write a query to get 10th top salaried employee**  
	select concat(first_name,' ',last_name) as 'full_name' ,salaries.salary from employees join 	salaries on employees.emp_no=salaries.emp_no order by salaries.salary desc limit 1 offset 9;  

**Write a query to get all the employees who are ranked between 8 and 15 based on the present --basic paid to them**  
select concat(first_name,' ',last_name) as 'full_name' ,salaries.salary from employees join salaries on employees.emp_no=salaries.emp_no order by salaries.salary desc limit 7 offset 8;  

**Write a query to get all the departments WHERE sum of the gross salary paid in any of the payperiod is greater than Rs.30000. Order by payment Period and gross salary **  
Select dept_emp.dept_no, salaries.salary  from   dept_emp   join salaries on dept_emp.emp_no=salaries.emp_no where salaries.salary>30000  order by salaries.from_date limit 10;  

**Write a query to get Employeenumber, payFromDate , payTodate, Grosspay, deductions , netPay, BASIC ,HRA,DA,VDA,LWW,ADMNAL,CNV,BonUS,NFH,PF , ESI. for all Pay periods **  
Select tpep.EmployeeNumber,tpe.Grosspay,tpe.Deductions,tpe.NetPay,tpm.FromDate,tpm.ToDate,tpep.ParamCode from  tblpayemployeeparamdetails as tpep join tblpayemployees as tpe on tpep.NoteNumber=tpe.NoteNumber  join tblpaymaster as tpm on tpe.NoteNumber=tpm.NoteNumber where tpep.ParamCode in ('BASIC' ,'HRA','DA','VDA','LWW','ADMNAL','CNV','BonUS','NFH','PF' , 'ESI') limit 10;  

**Company Has decided to Pay a bonus to all its employees. The criteria is as follows:
a. Service Type 1. Employee Type 1. Minimum service is 10 . Minimum service left should be 15 Years . Retirement age will be 60 Years 
b. Service Type 1. Employee Type 2. Minimum service is 12 . Minimum service left should be 14 Years . Retirement age will be 55 Years 
c. Service Type 1. Employee Type 3. Minimum service is 12 . Minimum service left should be 12 Years . Retirement age will be 55 Years 
d. For Service Type 2,3,4. Employee Type 3. Minimum Service should Be 15 and Minimum service left should be 20 Years . Retirement age will be 65 Years. 
Write a query to find out the employees who are eligible for bonus **  




**Write a query which returns Name, FatherName, DOB of employees whose PresentBasic is 
Greater than 30000. 
Select Name,FatherName,DOB from tblemployees where PresentBasic>30000;
Less than 3000. **  
Select Name,FatherName,DOB from tblemployees where PresentBasic<3000;
Between 3000 and 5000 
Select Name,FatherName,DOB from tblemployees where PresentBasic between 3000 and 5000;  

**Write a query which returns All the details of employees whose Name 
Ends with 'KHAN' **  
SELECT * from tblemployees where Name like '%Khan';  

**Starts with 'CHANDRA' **  
SELECT * from tblemployees where Name like 'Chandra%';  

**Is 'RAMESH' and their initial will be in the range of alphabets a-t. Ex: If an employee name is T.Ramesh, then your query should return his record. If an employee name is W Ramesh, then your query shouldn’t return his record.**  
SELECT Name from tblemployees where Name REGEXP '^[a-t]' and Name like '%Ramesh' ;  

**Select all the centers where max Length of the employee name is twice the min length of the employee name **  
select Name, length( Name ) from tblemployees where length( Name ) =2* ( select min( length( Name ) ) from tblemployees );  

**Write a query to find out all the departments where no employee has the Present Basic rounded of to 100 **  
Select DepartmentCode,Count(*) from tblemployees where not exists (Select round(PresentBasic,-2) from tblemployees) group by DepartmentCode;  

**Write a query to find out all the departments where all employee have their Present Basic rounded of to 100 **  
Select DepartmentCode,Count(*) from tblemployees where exists (Select round(PresentBasic,-2) from tblemployees) group by DepartmentCode;  

**Write a Query to find a list of employees and Payments where the employee is Paid VDA , NHF and LWW but not PF (Employee Number , From Date, to Date, Name , Designation Description, Service Type Description and ServiceStatus Description for the time period, VDA amount ,NHF amount, LWW amount) **  
select EmployeeNumber,Amount from tblpayemployeeparamdetails where ParamCode in ('VDA' ,'NHF','LWW') or ParamCode !='PF' limit 10;  

**Write a query to list top n employees from tblEmployees table based on present basic. Use rank functions (n can take any positive value like 1,2,3, so on...) **  
select Name,PresentBasic from tblemployees order by PresentBasic desc limit 10;