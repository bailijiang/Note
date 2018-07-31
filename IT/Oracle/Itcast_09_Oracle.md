<!-- MarkdownTOC -->

1. [Oracle基本命令](#oracle基本命令)
1. [PL/SQL中文乱码问题](#plsql中文乱码问题)
1. [单行\(数据\)函数](#单行数据函数)
1. [条件判断if...else...](#条件判断ifelse)
1. [分组函数](#分组函数)
1. [多表查询](#多表查询)
1. [__子查询__](#__子查询__)
1. [单行/多行子查询](#单行多行子查询)
1. [集合运算](#集合运算)
1. [SQL语言的类型](#sql语言的类型)
1. [__事务__](#__事务__)
1. [创建表](#创建表)
1. [修改表](#修改表)
1. [闪回](#闪回)
1. [__约束__](#__约束__)
1. [视图view](#视图view)
1. [序列](#序列)
1. [索引Index](#索引index)
1. [Pro*C/C++概念](#procc概念)
1. [Pro*C/C++操作](#procc操作)

<!-- /MarkdownTOC -->

<a id="oracle基本命令"></a>
#### Oracle基本命令
* select * from tab;
* Oracle中字符串使用单引号
    - `select userenv('language') from dual;`
* NULL值问题: 
    - 包含NULL值的表达式的值都为NULL
    - 解决: 滤空函数nvl(a, 0)
    - `select sal SALARY, comm COMMISION, sal*12 ANNUAL, sal*12+comm TOTAL 
from emp;`
    - `select sal SALARY, comm COMMISION, sal*12 ANNUAL, sal*12+nvl(comm, 0) TOTAL from emp;`
* NULL != NULL: 
    - `select empno, ename, comm from emp where comm is NULL;`
    - NULL不是空格或0
* 连接字符串: 
    - `select 'hello' || 'world' from dual;`
    - `select concat('hello', 'world') from dual`
    - 函数可以嵌套调用 `select concat(ename, concat(' is a ', job)) from emp`
* `select * from emp where deptno = 10`
* 日期:
    - `alter session set NLS_DATE_FORMAT = 'YYYY-MM-DD';`
    - `select * from emp where hiredate = '1981-11-17';`
* `select * from emp where sal between 1000 and 2000;`
* `select * from emp where deptno in (20, 30);`
    - not in 不能带NULL值
* `select * from emp where ename like 'S%';`
* `insert into emp(empno, ename, sal, deptno) values(1001, 'TOM_ABC', 3000, 10);`
* `select * from emp where ename like '%R_%' escape 'R';`
* `select ename, sal, sal*12 from emp order by 2 desc;`
* `select * from emp order by comm desc nulls last;`


<a id="plsql中文乱码问题"></a>
#### PL/SQL中文乱码问题
* 原因: 数据库字符集与本地plsql字符集不一致导致
* 解决: 在win中设置环境变量, NLS_LANG = AMERICAN_AMERICA.AL32UTF8

<a id="单行数据函数"></a>
#### 单行(数据)函数
* nvl, concat
* 字符串: 
    - `select lower('HELlO'), upper('worLd'), initcap('hello, world') from dual;`
    - substr, length, trim, instr, lpad, replace
* 数值: round, trunc, mod(求余)
* 日期: 
    - `select to_char(sysdate, 'yyyy-mm-dd hh24:mi:ss') from dual;`
    - `select to_char(sysdate, 'day') from dual;`
    - `select sysdate-1, sysdate, sysdate+1 from dual;`
    - 日期相减得到天数: `select round(sysdate-hiredate, 0) from emp;`
    - `select ename, round(months_between(sysdate, hiredate), 0) from emp;`
* 转换函数: to_char(数据, 格式), to_number, to_date
    - 字符串转日期: `select to_date('2004-05-07 13:23:44','yyyy-mm-dd hh24:mi:ss') from dual;`
    - `select to_char(sal, 'L9,999.99') from emp;`

<a id="条件判断ifelse"></a>
#### 条件判断if...else...
* case when then end
```
select ename, job, sal BEFORE, case job when 'PRESIDENT' then sal+1000
                                        when 'MANAGER' then sal+800
                                        else sal+400
                               end
from emp;
```
* decode:
```
select ename, job, sal BEFORE, decode(job, 'PRESIDENT', sal+1000,
                                           'MANAGER', sal+800,
                                           sal+400) AFTER
from emp;
```

<a id="分组函数"></a>
#### 分组函数
* 多行变换, 产生一个结果
* AVG/COUNT/MAX/MIN/SUM
    - 组函数自动滤空(屏蔽滤空用nvl)
    - `SELECT sum(sal) FROM emp;`
    - `SELECT count(*) FROM emp;` count(1)比count(*)快
    - `SELECT avg(sal) FROM emp;`
    - `SELECT count(distinct job) FROM emp;`
* GROUP BY:
    - 非组函数列必须出现在GROUP BY 后面(必须明确指定以谁分组)
    - `SELECT deptno,avg(sal) FROM emp GROUP BY deptno;`
    - GROUP BY ROLLUP():
```
        Group by  rollup(A ,B,C)

        产生的分组种数：4 种；
        第一种：group by  A,B,C
        第二种：group by  A,B
        第三种：group by  A
        第四种：group by  NULL (不分组)

        返回结果集：为以上四种分组统计结果集的并集且未去掉重复数据。
```
* HAVING: 先分组再过滤
    - HAVING可以与GROUP BY 连用, 放在其后面, 而WHERE不行
    - 功能与WHERE相同,进行筛选

<a id="多表查询"></a>
#### 多表查询
* 笛卡尔集:
    - 场景: 合并两张表(合并两个集合) 
    - 笛卡尔集的行数 = table1的行数 x table2的行数
    - 笛卡尔集的列数 = table1的列数 + table2的列数
    - 笛卡尔全集产生大量无效数据
    - 产生方式: 多表查询没有where过滤条件
    - 避免方法: 
        + `SELECT e.ename, e.job, e.sal, d.dname FROM emp e, dept d WHERE e.deptno = d.deptno;`
        + N张表至少有N-1个过滤条件
* 连接方式: 等值连接, 不等值连接, 外连接, 自连接
    - 不等值连接: `SELECT e.ename, e.job, e.sal, s.grade FROM emp e, salgrade s WHERE e.sal BETWEEN s.losal AND s.hisal;`
* 外连接: (填充)包含条件不成立的数据
    - 左外连接: 包含=号左边的表中不满足条件的数据, 在=号右边(+)
    - 右外连接: 包含=号右边的表中不满足条件的数据, 在=号左边(+)
* 统计所有部门的员工个数:
```
    SELECT d.deptno, d.dname, count(1)
    FROM emp e, dept d
    WHERE e.deptno(+) = d.deptno
    GROUP BY d.deptno, d.dname;

    1   10  ACCOUNTING  4
    2   40  OPERATIONS  0
    3   20  RESEARCH    5
    4   30  SALES       6
```
* 自连接: 一张表当两张表
```
    SELECT e.ename || ' boss is ' || nvl(b.ename, 'himself')
    FROM emp e, emp b
    WHERE e.mgr = b.empno(+)
    ;
```
* 层次查询: level
    - 伪列: 查则有, 不查则无
    - connect by prior  start with

<a id="__子查询__"></a>
#### __子查询__
* 一步不能求解的问题使用子查询
```
SELECT *
FROM emp
WHERE sal > (SELECT sal 
             FROM emp 
             WHERE ename = 'SCOTT')
;
```
* SELECT/FROM/WHERE/HAVING后均可放置子查询
* GROUP BY 后不能放子查询
* 子查询中不使用ORDER BY, TOP-N情况(分页)除外
* 查询SALES部门的所有员工信息
```
    SELECT *
    FROM emp
    WHERE deptno = (SELECT deptno 
                    FROM dept 
                    WHERE dname = 'SALES')
    ;
```
* 在SELECT后必须是单行子查询
* 找到工资最高的前3名
    - rownum: 伪列, 不能使用>, >=符号
    - Oracle分页:
```
        SELECT rownum, empno, ename, sal
        FROM (SELECT empno, ename, sal
              FROM emp
              ORDER BY sal DESC)
        WHERE rownum <= 4
        ;

        1   1   7839    KING    5000.00
        2   2   7902    FORD    3000.00
        3   3   1001    TOM_ABC 3000.00
        4   4   7788    SCOTT   3000.00
```
```
        SELECT *
        FROM (SELECT rownum rn, empno, ename, sal
              FROM (SELECT empno, ename, sal
                    FROM emp
                    ORDER BY sal DESC) a
              WHERE rownum <= 8) b
        WHERE b.rn > 4
        ;

        1   5   7566    JONES   2975.00
        2   6   7698    BLAKE   2850.00
        3   7   7782    CLARK   2450.00
        4   8   7499    ALLEN   1600.00
```
* 从员工表中找出大于本部门平均工资的员工
    - 思路: 将部门平均工资作为一张表d -> 找本部门且工资大于平均工资的员工
```
    SELECT *
    FROM emp e, (SELECT e2.deptno, AVG(e2.sal) davg
                 FROM emp e2
                 GROUP BY e2.deptno) d
    WHERE e.deptno = d.deptno AND e.sal > d.davg
    ;
```
* 相关子查询(内联):
    - 概念: 将主查询的某个值作为参数传递给子查询
    - 默认情况下子查询先于主查询执行, 但相关子查询例外
    - 从外往里写
```
    SELECT empno, ename, sal, (SELECT AVG(sal)
                               FROM emp e2
                               WHERE e1.deptno = e2.deptno) avgsal
    FROM emp e1
    WHERE e1.sal > (SELECT AVG(sal)
                    FROM emp e2
                    WHERE e1.deptno = e2.deptno)
    ;

    1   7499    ALLEN   1600.00 1566.66666666667
    2   7566    JONES   2975.00 2175
    3   7698    BLAKE   2850.00 1566.66666666667
    4   7788    SCOTT   3000.00 2175
    5   7839    KING    5000.00 2937.5
    6   7902    FORD    3000.00 2175
    7   1001    TOM_ABC 3000.00 2937.5
```
* 统计每年入职的员工个数
    - 思路: 先得到1981年入职员工个数: to_char转成年->decode与1981比较->sum求和
        + `SELECT SUM(DECODE(TO_CHAR(e.hiredate, 'yyyy'), '1981', 1, 0)) FROM emp e;`
```
    SELECT 
           COUNT(1) "TOTAL",
           SUM(DECODE(TO_CHAR(e.hiredate, 'yyyy'), '1980', 1, 0)) "1980",
           SUM(DECODE(TO_CHAR(e.hiredate, 'yyyy'), '1981', 1, 0)) "1981",
           SUM(DECODE(TO_CHAR(e.hiredate, 'yyyy'), '1982', 1, 0)) "1982",
           SUM(DECODE(TO_CHAR(e.hiredate, 'yyyy'), '1987', 1, 0)) "1987"
    FROM emp e
    ;
         TOTAL       1980       1981       1982       1987
    ---------- ---------- ---------- ---------- ----------
            15          1         10          1          2
```

<a id="单行多行子查询"></a>
#### 单行/多行子查询
* 单行子查询: >, <, =, <>, >=, <=, !=
* 多行子查询: in, not in, any, all
* 查询部门名称为SALES和ACCOUNTING的所有员工信息
```
    SELECT e.empno, e.ename, d.dname
    FROM emp e, dept d
    WHERE e.deptno = d.deptno
      AND d.dname in ('ACCOUNTING', 'SALES')
    ;
```
* 包含NULL值时不能用NOT IN:
    - 原因: NOT IN 是和子查询集合中所有数值比较, 当与NULL值比较时 NULL != NULL, 导致WHERE判定结果为假
```
    SELECT *
    FROM emp
    WHERE empno NOT IN (SELECT mgr
                          FROM emp)
    ;
    // 查询没有mgr的员工, 结果为空
```

<a id="集合运算"></a>
#### 集合运算
* 交集, 并集, 差集
* UNION / UNION ALL(重叠部分记录2次), INTERSECT, MINUS
* 参与运算的各个集合必须列数相同, 且类型一致, 否则出错
    - 可以通过 to_char(null), to_number(null) 去凑成一致的列数类型
```
    SELECT *
    FROM emp
    WHERE deptno = 10
    UNION
    SELECT *
    FROM emp
    WHERE deptno = 20
    ;
```
* 打开SQL语句执行时间显示: `set timing on`

<a id="sql语言的类型"></a>
#### SQL语言的类型
* DML: insert, update, delete, select
    - insert into values
    - update set where
    - delete from where
* DDL: truncate, create, alter, drop
    - truncate: 不可回滚rollback
* DCL: commit, rollback(必须在commit之前使用才有效)

<a id="__事务__"></a>
#### __事务__
* 多条SQL语句
* 特点: 要么都成功, 要么都失败
* 特性: ACID 原子性, 一致性, 隔离性, 持久性
    - Oracle默认隔离级别: Read Committed 读已提交
* 事务的起始结束标志: DML语句开启事务, commit/rollback都是事务的结束标志
    - 隐式提交: DDL语句(create...), 正常退出(exit/quit)
* 事务控制: savepoint
    - SAVEPOINT A
    - SAVEPOINT B
    - ROLLBACK TO SAVEPOINT A
    - ROLLBACK TO SAVEPOINT B

<a id="创建表"></a>
#### 创建表
* create table test1(tid number, tname varchar2(20), hiredate date default sysdate);
    - default 作用: 当向表中insert into数据时, 没有指定时间的情况下, 所使用的默认时间
* 创建表并填充数据: create table test2 as select ...
    - select中对表达式用别名

<a id="修改表"></a>
#### 修改表
* ALTER TABLE test1 ADD image blob
* ALTER TABLE test1 MODIFY...
* ALTER TABLE test1 DROP COLUMN...
* ALTER TABLE test1 RENAME COLUMN...TO...

<a id="闪回"></a>
#### 闪回
* 概念: 闪回技术是Oracle强大数据库备份恢复机制的一部分，在数据库发生逻辑错误的时候，闪回技术能提供快速且最小损失的恢复（多数闪回功能都能在数据库联机状态下完成）。需要注意的是，闪回技术旨在快速恢复逻辑错误，对于物理损坏或是介质丢失的错误，闪回技术就回天乏术了，还是得借助于Oracle一些高级的备份恢复工具如RMAN去完成（这才是Oracle强大备份恢复机制的精髓）
* 闪回操作
    - `SQL> flashback table dept to timestamp to_timestamp('2016-09-10 11:00:00','yyyy-mm-dd hh24:mi:ss');`

<a id="__约束__"></a>
#### __约束__
* 约束的种类: NOT NULL / UNIQUE / PRIMARY KEY / FOREIGN KEY / CHECK
* 多个约束之间用空格隔开, 不用,号
* __外键约束__: 
    - emp表中 deptno列是外键
    - 先有dept表, 后有emp表
    - dept是主表, emp是从表
    - 在从表中创建外键约束
    - dept表约束了emp表
    - 外键约束保持了主从表之间的数据的一致性
    - 设置外键时通过 REFERENCES 指定参数来定义删除记录的行为: 
        + ON DELETE CASCADE : 关联删除
        + ON DELETE SET NULL: 关联置空
* 创建表时使用 CONSTRAINT 进行外键约束
    - `CREATE TABLE student(deptno NUMBER CONSTRAINT student_FK REFERENCES dept(deptno) ON DELETE SET NULL)`
* 只有主表的主键可以作为从表的外键
* 查看指定表的外键约束:
```
    //表名大写
    SELECT *
    FROM user_constraints c
    WHERE c.constraint_type = 'R'
          AND c.table_name = 'EMP'
    ;
```

<a id="视图view"></a>
#### 视图view
* 作用: 
    - 简化复杂查询: 给多表查询创建视图
    - 限制数据访问
* 创建视图: `CREATE VIEW ...`
* 视图不能提高性能
* 视图不能被修改

<a id="序列"></a>
#### 序列
* 游标机制
* 在内存中, 访问效率高
* 一般用于创建主键
* 创建序列: `CREATE SEQUENCE myseq`
* 使用序列插入数据: `INSERT INTO table1 VALUES(myseq.nextval, 'aaa');`
* 删除序列: `DROP SEQUENCE myseq;`
* 序列是多张表的共有对象
* ROLLBACK/掉电会造成数据不连续

<a id="索引index"></a>
#### 索引Index
* 功能: 查询速度快, 提高查询效率
* 在列上创建索引: `CREATE INDEX myindex ON emp(deptno);`
* 适合索引使用场景: 
    - 数据分布广
    - 列经常在WHERE子句或连接条件中出现
    - 表经常被访问且数据量大, 被访问的数据占数据总量的2%~%4之间(多个SELECT, 每个查询数据量都在总数据量的2%~%4之间)
* 索引表是独立存储
* 当删除一张表是, 基于这张表的所有索引都会被删除

<a id="procc概念"></a>
#### Pro*C/C++概念
* 相当于一门新语言
* 功能: Linux/Win下通过Proc连接Oracle数据库进行CRUD操作
* 用struct映射操作单行多列(相当于ORM)
* 使用流程: 先用proc编译器将.pc文件编译后生成.c文件再编译成可执行文件
* Linux C/C++程序远程连接Oracle需要安装Oracle client客户端, 安装proc编译环境, 使用proc
* 使用proc前需要配置pcscfg.cfg文件(参照RedHat安装Oracle11gR2文档)
```
    // /usr/lib/gcc/x86_64-redhat-linux/4.1.1/include 改成 /usr/lib/gcc/x86_64-redhat-linux/4.4.4/include
    [oracle@oracledb proc_test]$ cat /opt/oracle/app/product/11.2.0/dbhome_1/precomp/admin/pcscfg.cfg 
    sys_include=($ORACLE_HOME/precomp/public,/usr/include,/usr/lib/gcc-lib/x86_64-redhat-linux/3.2.3/include,/usr/lib/gcc/x86_64-redhat-linux/4.4.4/include,/usr/lib64/gcc/x86_64-suse-linux/4.1.0/include,/usr/lib64/gcc/x86_64-suse-linux/4.3/include)
    ltype=short
    define=__x86_64__

```
* proc helloworld:
```
    // hello.pc
    #include <stdio.h>
    #include <sqlca.h>

    EXEC SQL BEGIN DECLARE SECTION;

        char *serverid = "scott/scott@orcl";

    EXEC SQL END DECLARE SECTION;


    int main(void)
    {
        EXEC SQL CONNECT :serverid;

        printf("hello\n");


        EXEC SQL COMMIT RELEASE;

        return 0;
    }
    
    proc hello.pc
    gcc hello.c -o hello -L /opt/oracle/app/product/11.2.0/dbhome_1/lib 
                         -I /opt/oracle/app/product/11.2.0/dbhome_1/precomp/public/ 
                         -l clntsh
```
* proc 的 makefile:
```

    C_FILES = $(wildcard *.pc)
    C_MIDDLE1 = $(patsubst %.pc, %.c, $(C_FILES))
    C_MIDDLE2 = $(patsubst %.pc, %.lis, $(C_FILES))
    C_TARGET = $(patsubst %.pc, %, $(C_FILES))

    lib_path = ${ORACLE_HOME}/lib
    inc_path = ${ORACLE_HOME}/precomp/public

    all:$(C_TARGET) 

    $(C_TARGET):%:%.c
        gcc $< -o $@ -L$(lib_path) -I$(inc_path) -lclntsh
    %.c:%.pc
        proc $<

    .PHONY:clean all

    clean:
        -rm -rf $(C_TARGET) $(C_MIDDLE1) $(C_MIDDLE2) *~ 
```

<a id="procc操作"></a>
#### Pro*C/C++操作
* Connect数据库进行INSERT/UPDATE/DELETE
```
    //01_ins_upd_del.pc   sqlca.sqlcode
    #include <stdio.h>
    #include <sqlca.h>

    EXEC SQL BEGIN DECLARE SECTION;

        char *serverid = "scott/scott@orcl";    //宿主变量

    EXEC SQL END DECLARE SECTION;

    int main(void)
    {
        int ret;

        EXEC SQL CONNECT :serverid;
        
        if(sqlca.sqlcode != 0)
        {
            ret = sqlca.sqlcode;
            printf("Connect Oracle Failed, Error: %d\n", sqlca.sqlcode);
            return ret;
        }
        printf("Connect OK...\n");

        // EXEC SQL INSERT INTO dept(deptno, dname, loc) VALUES(50, 'Engineer', 'Beijing');
        // if(sqlca.sqlcode != 0)
        // {
        //  ret = sqlca.sqlcode;
        //  printf("INSERT Failed, Error: %d\n", sqlca.sqlcode);
        //  return ret;
        // }
        // printf("INSERT OK...\n");

        // EXEC SQL UPDATE dept SET dname='Dev', loc='Shanghai' WHERE deptno=50; 
        // if(sqlca.sqlcode != 0)
        // {
        //  ret = sqlca.sqlcode;
        //  printf("UPDATE Failed, Error: %d\n", sqlca.sqlcode);
        //  return ret;
        // }
        // printf("UPDATE OK...\n");

        EXEC SQL DELETE FROM dept WHERE deptno=50; 
        if(sqlca.sqlcode != 0)
        {
            ret = sqlca.sqlcode;
            printf("DELETE Oracle Failed, Error: %d\n", sqlca.sqlcode);
            return ret;
        }
        printf("DELETE OK...\n");

        EXEC SQL COMMIT RELEASE;
        if(sqlca.sqlcode != 0)
        {
            ret = sqlca.sqlcode;
            printf("RELEASE Oracle Failed, Error: %d\n", sqlca.sqlcode);
            return ret;
        }
        printf("RELEASE OK...\n");

        return 0;
    }
```
* Linux下查看ORA错误信息: 
    - `oerr ora 1`
    - `oerr ora 1017`
* SELECT操作-自动类型转换
```
    #include <stdio.h>
    #include <sqlca.h>

    EXEC SQL BEGIN DECLARE SECTION;

        char *serverid = "scott/scott@orcl";    //宿主变量

        int deptno;
        varchar dname[20];
        varchar loc[20];

    EXEC SQL END DECLARE SECTION;

    int main(void)
    {
        int ret;

        EXEC SQL CONNECT :serverid;
        
        if(sqlca.sqlcode != 0)
        {
            ret = sqlca.sqlcode;
            printf("Connect Oracle Failed, Error: %d\n", sqlca.sqlcode);
            return ret;
        }
        printf("Connect OK...\n");

        EXEC SQL SELECT deptno, dname, loc INTO :deptno, :dname, :loc FROM dept WHERE deptno=10;
        if(sqlca.sqlcode != 0)
        {
            ret = sqlca.sqlcode;
            printf("SELECT Error: %d\n", sqlca.sqlcode);
            return ret;
        }
        printf("SELECT OK...\n");
        printf("deptno:%d|dname:%s|loc:%s\n", deptno, dname.arr, loc.arr);
        // /* varchar loc[20];*/ 
        // struct { unsigned short len; unsigned char arr[20]; } loc;


        EXEC SQL COMMIT RELEASE;
        if(sqlca.sqlcode != 0)
        {
            ret = sqlca.sqlcode;
            printf("RELEASE Oracle Failed, Error: %d\n", sqlca.sqlcode);
            return ret;
        }
        printf("RELEASE OK...\n");

        return 0;
    }
```
* 编写Proc程序时只使用Proc数据类型(Host Variables)
    - 数据类型转换流程: Proc数据类型->C/C++数据类型->Oracle数据类型
```
    Table 4–4 C Datatypes for Host Variables
    C Datatype or Pseudotype Description
    char        single character
    char[n]     n-character array (string)
    int         integer
    short       small integer
    long        large integer
    long long   very large (8-byte) integer
    float       floating-point number (usually single precision)
    double      floating-point number (always double precision)
    VARCHAR[n]  variable-length string
```
* SELECT操作-手动类型转换
    - 流程: typedef定义类型->EXEC SQL TYPE注册->定义变量->填充数据->执行SQL
```
    #include <stdio.h>
    #include <string.h>
    #include <sqlca.h>
    
    // char dnameType[20]; 定义变量
    typedef char dnameType[20]; // 定义变量类型

    EXEC SQL BEGIN DECLARE SECTION;

        char *serverid = "scott/scott@orcl";    //宿主变量

        EXEC SQL TYPE dnameType is string(20);  //注册

        int deptno;
        dnameType dname;
        dnameType loc;

    EXEC SQL END DECLARE SECTION;

    int main(void)
    {
        int ret;

        EXEC SQL CONNECT :serverid;
        
        if(sqlca.sqlcode != 0)
        {
            ret = sqlca.sqlcode;
            printf("Connect Oracle Failed, Error: %d\n", sqlca.sqlcode);
            return ret;
        }
        printf("Connect OK...\n");


        deptno = 80;
        strcpy(dname, "dept80");
        strcpy(loc, "GuangZhou");

        EXEC SQL INSERT INTO dept(deptno, dname, loc) VALUES(:deptno, :dname, :loc);
        if(sqlca.sqlcode != 0)
        {
            ret = sqlca.sqlcode;
            printf("INSERT Error: %d\n", sqlca.sqlcode);
            return ret;
        }
        printf("INSERT OK...\n");

        EXEC SQL COMMIT;

        EXEC SQL SELECT deptno, dname, loc INTO :deptno, :dname, :loc FROM dept WHERE deptno=:deptno;
        if(sqlca.sqlcode != 0)
        {
            ret = sqlca.sqlcode;
            printf("SELECT Error: %d\n", sqlca.sqlcode);
            return ret;
        }
        printf("SELECT OK...\n");
        printf("deptno:%d|dname:%s|loc:%s\n", deptno, dname, loc);

        EXEC SQL COMMIT RELEASE;
        if(sqlca.sqlcode != 0)
        {
            ret = sqlca.sqlcode;
            printf("RELEASE Oracle Failed, Error: %d\n", sqlca.sqlcode);
            return ret;
        }
        printf("RELEASE OK...\n");

        return 0;
    }
```




