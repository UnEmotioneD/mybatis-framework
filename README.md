# MyBatis

## What is MyBatis?

**MyBatis** is a lightweight Java framework that simplifies working with databases by acting as a bridge between your Java application and SQL. It helps you write and execute SQL queries efficiently, map database records to Java objects, and handle complex database operations with minimal code.

### In Simple Terms:
- You focus on **SQL** for database interactions.
- MyBatis handles the boring parts, like mapping query results to objects and vice versa.
- It’s perfect if you want control over your SQL without the overhead of managing boilerplate code.

## Why Use MyBatis?
- **Custom SQL**: Write your own queries for complete control.
- **Easy Mapping**: Automatically maps database results to Java objects.
- **Flexible**: Supports complex queries and dynamic SQL generation.
- **Lightweight**: No heavy setup or overhead like some ORMs (e.g., Hibernate).

---

### Resources
- [Official MyBatis Documentation](https://mybatis.org/mybatis-3/)
- [GitHub Repository](https://github.com/mybatis/mybatis-3)
---

## Table of Contents

- [MyBatis Settings](#mybatis-settings)
- [Aliases](#aliases)
- [Data Type](#data-type-inside-xml-files)
- [Dynamic If](#dynamic-if)
- [Dynamic For](#dynamic-for)

---

## MyBatis Settings

---

#### Window / Preferences / XML / XML Catalog / User Specified Entries / Catalog Entry

##### Add config

###### location: 
```bash 
http://mybatis.org/dtd/mybatis-3-config.dtd
``` 
###### key: 
```bash 
-//mybatis.org//DTD Config 3.0//EN
```

##### Add mapper
###### location : 
```bash
http://mybatis.org/dtd/mybatis-3-mapper.dtd
```
###### key : 
```bash
-//mybatis.org//DTD Mapper 3.0//EN
```

---

#### Window / Preferences / XML (Wild Web Developer) /
###### check
`download external resources like referenced dtd, xsd`

---

##### Create new `source folder` under the project directory named `resources`
##### Create `XML file` under the resource folder named `mybatis-config`
##### Click next and check
`create file using a DTD or XML scheme file`
##### Click next select `xml catalog entry`
##### Click `User Specified Entries` select the one with `config`
##### next and finish

#### Inside the `mybatis-config.xml` under inside `<configuration>` tag
``` xml
<settings>
    <setting name="jdbcTypeForNull" value="NULL" />
</settings>

<environments default="development">
    <environment id="development">
      <transactionManager type="JDBC" />
      <dataSource type="POOLED">
          <property name="driver" value="oracle.jdbc.driver.OracleDriver" />
          <property name="url" value="jdbc:oracle:thin:@127.0.0.1:1521:xe" />
          <property name="username" value="mybatis_test" />
          <property name="password" value="1234" />
      </dataSource>
    </environment>
</environments>
```

---

##### Create new `Folder` under the resources named `mapper`
##### Create `XML File` under the mapper folder named `member-mapper`
###### (member from member-mapper can be anything you want)
##### Click next and check
`create file using a DTD or XML scheme file`
##### Click next select `XML Catalog Entry`
##### Click `User Specified Entries` select the one with `mapper`
##### next and finish

###### This is where SQL is written

---
## Aliases
### To use `Aliases` inside the `mapper.xml` file
##### Added this code under the `configuration` tab inside the `config.xml` 
- for `parameterType` and `resultType` you can now use `member` instead of full path
```xml
<typeAliases>
    <typeAlias type="kr.or.iei.member.model.vo.Member" alias="member" />
</typeAliases>
```

---

## Data Type inside .XML files

| Type   | MyBatis Type |
|------- | --------|
| int    |  _int
| short  |  _short
| byte   |  _byte
| float  |  _float
| double |  _double
|        |        |
| String |  string
| Map    |  map
| Hash   |  hashmap (or map)
| Date   |  date

---

### Dynamic IF
- Selecting `a` ensures the query remains valid by providing a constant fallback column, preventing errors when no dynamic columns are selected.
```sql
<select id="selDynamicIfTest"
parameterType="member"
resultType="member">
select
	<if test="sFlag1 != null and sFlag1 == 'on'">
	member_no    as "memberNo",
	</if>
	<if test="sFlag2 != null and sFlag2 == 'on'">
	member_id    as "memberId",
	</if>
	<if test="sFlag3 != null and sFlag3 == 'on'">
	member_name  as "memberName",
	</if>
	<if test="sFlag4 != null and sFlag4 == 'on'">
	member_email as "memberEmail",
	</if>
	<if test="sFlag5 != null and sFlag5 == 'on'">
	member_phone as "memberPhone",
	</if>
	'a'
from tbl_member
</select>

```

---

### Dynamic FOR
- `collection` : arguments type
- `item` : name to use inside `foreach` tag
- `open` : before the first element
- `separator` : between elements
- `close` : after the last element

```sql
<select id="selDynamicForTest"
parameterType="map"
resultType="member">
select
	member_no    as "memberNo",
	member_id    as "memberId",
	member_pw    as "memberPw",
	member_name  as "memberName",
	member_email as "memberEmail",
	member_phone as "memberPhone",
	member_addr  as "memberAddr",
	member_level as "memberLevel",
	enroll_date  as "enrollDate"
from tbl_member
where member_id in
<foreach collection="array" item="id" open="(" separator="," close=")">
#{id}
</foreach>
</select>
```

---

#### Happy hacking
