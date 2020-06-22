# Using Hibernate with MySQL
Prepare your eclipse environment to have the ""SQL development tools"" and ""JBoss Tools" installed

* Search marketplace for tools

![JBOSS1](img/E1.png)

* Install JBOSS which includes the hibernate tools

![JBOSS2](img/E2.png)

* Add MySQL Driver to Database Management
* Specifiy the mysql jdbc driver
* create the user account with ""mysql_native_password""
* Create a new java class "Student_info1"
```
package com.claydesk1.hibernate;

import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.Table;

@Entity
@Table
public class Student_info1 {
	private int roll_no;
	private String name;
	
	@Id
	public int getRoll_no() {
		return roll_no;
	}
	
	public void setRoll_no(int roll_no) {
		this.roll_no = roll_no;
	}
	public String getName() {
		return name;
	}
	public void setName(String name) {
		this.name = name;
	}
}
```

* Creat a hibernate mapping on Student_info1

```


```





