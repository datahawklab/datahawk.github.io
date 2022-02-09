---
layout: post
title: Springboot 2 oracle Universal Connection Pool configuration(UCP)
date: 2022-02-02 03:21
category: notes
author: swapan chakrabarty
header-img: "img/post-bg-2015.jpg"
catalog: true
tags:
    - Universal Connection Pool
    - Spring Boot
    - Oracle  
summary: Springboot 2 oracle Universal Connection Pool configuration(UCP) 
---

## Directory Structure

SpringBootSample
├── Readme.md
├── pom.xml
└── src
    └── main
        ├── java
        │   └── com
        │       └── oracle
        │           └── springapp
        │               ├── OracleJdbcApplication.java
        │               ├── dao
        │               │   ├── AllTablesDAO.java
        │               │   ├── EmployeeDAO.java
        │               │   └── impl
        │               │       ├── AllTablesDAOImpl.java
        │               │       └── EmployeeDAOImpl.java
        │               ├── model
        │               │   ├── AllTables.java
        │               │   └── Employee.java
        │               └── service
        │                   ├── EmployeeService.java
        │                   └── impl
        │                       └── EmployeeServiceImpl.java
        └── resources
            └── application.properties


## pom.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
	<modelVersion>4.0.0</modelVersion>
	<groupId>com.oracle</groupId>
	<artifactId>springapp</artifactId>
	<version>0.0.1-SNAPSHOT</version>
	<name>springapp</name>
	<url>http://www.oracle.com</url>
	<parent>
		<groupId>org.springframework.boot</groupId>
		<artifactId>spring-boot-starter-parent</artifactId>
		<version>2.2.6.RELEASE</version>
	</parent>
	<properties>
		<project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
		<maven.compiler.source>1.8</maven.compiler.source>
		<maven.compiler.target>1.8</maven.compiler.target>
	</properties>
	<dependencies>
		<dependency>
			<groupId>junit</groupId>
			<artifactId>junit</artifactId>
			<scope>test</scope>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter</artifactId>
		</dependency>
		<dependency>
			<groupId>org.springframework.boot</groupId>
			<artifactId>spring-boot-starter-jdbc</artifactId>
		</dependency>
		<dependency>
			<groupId>javax.inject</groupId>
			<artifactId>javax.inject</artifactId>
			<version>1</version>
		</dependency>
		<dependency>
			<groupId>com.oracle.database.jdbc</groupId>
			<artifactId>ojdbc8-production</artifactId>
			<version>21.1.0.0</version>
			<type>pom</type>
		</dependency>

	</dependencies>
	<build>
		<plugins>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
			</plugin>
			<plugin>
				<groupId>org.springframework.boot</groupId>
				<artifactId>spring-boot-maven-plugin</artifactId>
				<configuration>
					<mainClass>com.oracle.springapp.OracleJdbcApplication</mainClass>
				</configuration>
			</plugin>
		</plugins>
	</build>
</project>
```

```bash
mkdir src\main\java\com\oracle\springapp\dao 
```

```java
package com.oracle.springapp.dao;

import java.util.List;

import com.oracle.springapp.model.AllTables;

/**
 * Simple DAO interface for EMP table.
 *
 */
public interface AllTablesDAO {
	public List<AllTables> getTableNames();
}
```

```java
package com.oracle.springapp.dao;

import java.util.List;

import com.oracle.springapp.model.Employee;

/**
 * Simple DAO interface for EMP table.
 *
 */
public interface EmployeeDAO {
	public List<Employee> getAllEmployees();

	void insertEmployee(Employee employee);
}
```

```bash
mkdir src\main\java\com\oracle\springapp\dao\impl
```

```java
package com.oracle.springapp.dao.impl;

import java.util.List;

import javax.annotation.PostConstruct;
import javax.sql.DataSource;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.support.JdbcDaoSupport;
import org.springframework.stereotype.Repository;

import com.oracle.springapp.dao.AllTablesDAO;
import com.oracle.springapp.model.AllTables;
/**
 * Simple Java class which uses Spring's JdbcDaoSupport class to implement
 * business logic.
 *
 */
@Repository
public class AllTablesDAOImpl extends JdbcDaoSupport implements AllTablesDAO {
	@Autowired
	private DataSource dataSource;

	@PostConstruct
	public void initialize() {
		setDataSource(dataSource);
		System.out.println("Datasource used: " + dataSource);
	}

	@Override
	public List<AllTables> getTableNames() {
		final String sql = "SELECT owner, table_name, status, num_rows FROM all_tables where rownum < 20";
		return getJdbcTemplate().query(sql, 
				(rs, rowNum) -> new AllTables(rs.getString("owner"),
						rs.getString("table_name"),
						rs.getString("status"),
						rs.getInt("num_rows")			
						));	
		
	}
}
```

```java
package com.oracle.springapp.dao.impl;

import java.util.List;

import javax.annotation.PostConstruct;
import javax.sql.DataSource;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.support.JdbcDaoSupport;
import org.springframework.stereotype.Repository;

import com.oracle.springapp.dao.EmployeeDAO;
import com.oracle.springapp.model.Employee;

/**
 * Simple Java class which uses Spring's JdbcDaoSupport class to implement
 * business logic.
 *
 */
@Repository
public class EmployeeDAOImpl extends JdbcDaoSupport implements EmployeeDAO {
	
	@Autowired
	private DataSource dataSource;

	@PostConstruct
	public void initialize() {
		setDataSource(dataSource);
		System.out.println("Datasource used: " + dataSource);
	}

	@Override
	public List<Employee> getAllEmployees() {
		final String sql = "SELECT empno, ename, job, mgr, hiredate, sal, comm, deptno FROM emp";
		return getJdbcTemplate().query(sql, 
				(rs, rowNum) -> new Employee(rs.getInt("empno"),
						rs.getString("ename"),
						rs.getString("job"),
						rs.getInt("mgr"),
						rs.getDate("hiredate"),
						rs.getInt("sal"),
						rs.getInt("comm"),
						rs.getInt("deptno")
						));
		
		
	}
	
	@Override
	public void insertEmployee(Employee employee) {
		String sql = "Select max(empno) from emp"; 
	    int[] emp_no = new int[1];
	    //getJdbcTemplate().query(sql, (rs, rowNum) -> emp_no[rowNum] = rs.getInt(1));
	    getJdbcTemplate().query(sql, (resultSet, rowNumber) -> emp_no[rowNumber] = resultSet.getInt(1));	    
	    System.out.println("Max emploee number is: " + emp_no[0]);
	    
	    sql = "INSERT INTO EMP VALUES(?,?,?,?,?,?,?,?)";
	    int newEmpNo = emp_no[0]+1;
		getJdbcTemplate().update(sql, new Object[]{
				newEmpNo, 
				employee.getName()+(newEmpNo),
				employee.getJob(),
				employee.getManager(),
				employee.getHiredate(),
				employee.getSalary(),
				employee.getCommission(),
				employee.getDeptno()
		});
	}
}
```

```bash
mkdir src\main\java\com\oracle\springapp\model
```

```java
package com.oracle.springapp.model;

/**
 *  Simple model for ALL_TABLES
 */
public class AllTables {
	private String owner;
	private String table_name;
	private String status;
	private int num_rows;

	
	public AllTables(String _owner,
			String _table_name,
			String _status,
			int _num_rows) {
		owner = _owner;
		table_name = _table_name;
		status = _status;
		num_rows = _num_rows;
	}


	public String getOwner() {
		return owner;
	}

	public String getTableName() {
		return table_name;
	}

	public String getStatus() {
		return status;
	}

	public int getNumRows() {
		return num_rows;
	}

	public String toString() {
		return String.format("%25s %25s %25s %20d", owner, table_name,
				status, num_rows);

	}

}
```

```java
package com.oracle.springapp.model;

import java.sql.Date;
import java.sql.Timestamp;
/**
 * Simple model for EMP table.
 *
 */
public class Employee {
	private Integer empno;
	private String name;
	private String job;
	private Integer manager;
	private Date hiredate;
	private Integer salary;
	private Integer commission;
	private Integer deptno;
	
	public Employee(int _empno,
			String _name,
			String _job,
			int _mgr,
			Date date,
			int _sal,
			int _commission,
			int _deptno) {
		empno = _empno;
		name = _name;
		job = _job;
		manager = _mgr;
		hiredate = date;
		salary = _sal;
		commission = _commission;
		deptno = _deptno;
	}


	public Integer getEmpno() {
		return empno;
	}

	public void setEmpno(int empno) {
		this.empno = empno;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getJob() {
		return job;
	}

	public void setJob(String job) {
		this.job = job;
	}

	public Integer getManager() {
		return manager;
	}

	public void setManager(int mgr) {
		this.manager = mgr;
	}

	public Integer getSalary() {
		return salary;
	}

	public void setSalary(int sal) {
		this.salary = sal;
	}

	public Integer getDeptno() {
		return deptno;
	}
	
	public Integer getCommission() {
		return commission;
		
	}
	
	public Date getHiredate() {
		return hiredate;
	}
	
	public void setHiredate(Timestamp hiredate) {
		this.setHiredate(hiredate);
	}
	
	public void setCommission(int commission) {
		this.commission=commission;
		
	}

	public void setDeptno(int deptno) {
		this.deptno = deptno;
	}

	public String toString() {
		return String.format("%20s %20s %20s %20s %20s %20s %20s %20s", empno, name,
				job, manager, hiredate, salary, commission, deptno);

	}
}
```

```bash
mkdir src\main\java\com\oracle\springapp\service
```

```java
package com.oracle.springapp.service;

import com.oracle.springapp.model.Employee;

public interface EmployeeService {
	/**
	 * Display all employees.
	 */
	public void displayEmployees();
	
	/**
	 * Get table name of the top 20 tables
	 */
	
	public void displayTableNames();
	
	/**
	 * Create a new employee record
	 */
	
	public void insertEmployee(Employee employee);

	
}
```

```bash
mkdir src\main\java\com\oracle\springapp\service
```

```java
package com.oracle.springapp.service.impl;

import java.util.List;

import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import com.oracle.springapp.model.Employee;
import com.oracle.springapp.service.EmployeeService;
import com.oracle.springapp.dao.EmployeeDAO;

import com.oracle.springapp.model.AllTables;
import com.oracle.springapp.dao.AllTablesDAO;

@Service
public class EmployeeServiceImpl implements EmployeeService {

	@Autowired
	EmployeeDAO employeeDao;
	
	
	@Autowired
	AllTablesDAO allTablesDao;
	
	/**
	 *  Get the top 20 table names from all tables 
	 */
	@Override
	public void displayTableNames() {
		List<AllTables> allTables_list = allTablesDao.getTableNames();
		
		System.out.println(" Displaying Table Names ");

		System.out.println(String.format("%20s %20s %20s %20s \n", 
				"OWNER", "TABLE_NAME", "STATUS", "NUM_ROWS"));
		
		for(AllTables allTables: allTables_list)
			System.out.println(allTables);
	}
	
	/**
	 *  Displays the Employees from the EMP table 
	 */

	@Override
	public void displayEmployees() {
		List<Employee> employees = employeeDao.getAllEmployees();

		System.out.println(String.format("%20s %20s %20s %20s %20s %20s %20s %20s \n", 
				"EMPNO", "ENAME", "JOB", "MGR", "HIREDATE", "SALARY", "COMM", "DEPT"));
		
		for(Employee employee: employees)
			System.out.println(employee);
	}
	
	@Override
	public void insertEmployee(Employee employee) {
		employeeDao.insertEmployee(employee);	
	}
	
	
}
```

```bash
mkdir src\main\resources
```
application.properties
```properties
# For connecting to Autonomous Database (ATP) refer https://www.oracle.com/database/technologies/getting-started-using-jdbc.html
# Provide the database URL, database username and database password 
spring.datasource.url=jdbc:oracle:thin:@dbname_alias?TNS_ADMIN=/Users/test/wallet/wallet_dbname_alias
spring.datasource.username=<your-db-user>
spring.datasource.password=<your-db-password>

# Properties for using Universal Connection Pool (UCP)
# Note: These properties require JDBC version 21.0.0.0
spring.datasource.driver-class-name=oracle.jdbc.OracleDriver
spring.datasource.type=oracle.ucp.jdbc.UCPDataSource
spring.datasource.ucp.connection-factory-class-name=oracle.jdbc.replay.OracleDataSourceImpl
spring.datasource.ucp.sql-for-validate-connection=select * from dual
spring.datasource.ucp.connection-pool-name=connectionPoolName1
spring.datasource.ucp.initial-pool-size=15
spring.datasource.ucp.min-pool-size=10
spring.datasource.ucp.max-pool-size=30
```
