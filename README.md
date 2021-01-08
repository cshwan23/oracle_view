# oracle_view
view 실시간 select한 결과물을 보여주는 가상테이블 


    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- View(뷰)
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm

    --++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    -- <1>VIEW
    --++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    -- ● 실시간으로 select 결과물을 테이블 처럼 가장해서 보여주는 [가상 테이블]이다.

    --++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    -- <2>VIEW 의 특징
    --++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    -- ● 객체로서 저장된다. 즉 이름을 가지고 저장된다.
    -- ● 뷰도 테이블이므로 select 대상이 된다.
    -- ● 뷰의 근원이 되는 실존 테이블의 데이터가 변경된 후 뷰가 실행되면 뷰에 그대로 반영돤다.
    -- (= 실존 테이블과 view 는 항상 동기화 된다.)
    -- ● 뷰를 가지고 또 다른 뷰를 만들 수 있다.

    --++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    -- <3>VIEW 의 사용목적 (기사,입사문제)
    --++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    -- ● join 등과 같은 길고 복잡한 select 문장을 뷰로 만들어 쿼리문을 단순화시켜 [편의성]을 제공한다.
    -- ● view 를 통해 특정 칼럼의 데이터만 볼 수 있으므로 실존 테이블에 대한 제한적인 권한을 부여하여 [보안성]을 부여한다.
    --      ▶ ︎employee 테이블에서 연봉 컬럼만 검색하지 못하게 하고 싶을 경우
    --         ▷ 사용자에게 employee 테이블에 select 권한을 주지 않고, 연봉 컬럼만 제외된 뷰를 select 할 수 있는 권한을 주면 된다.

    --++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    -- <4>VIEW 생성 형식 (기사,입사문제 : with check option)
    --++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
    ------------------------------------------------------------
    -- CREATE  [OR REPLACE]  [FORCE | NOFORCE] VIEW 뷰이름 AS select 문장 [WITH READ ONLY] [WITH CHECK OPTION];
    ------------------------------------------------------------
    -- ▶ OR REPLACE
    --      ▷ 동일한 뷰가 있으면 덮어쓴다. 즉 기존의 뷰가 갱신된다. 미사용 시 뷰 생성 불가능
    -- ▶ FORCE
    --      ▷ 존재하지 않는 테이블로부터 뷰를 만들 경우라도 뷰를 만든다.
    --      ▷ FORCE 에 의해 생성된 뷰를 select 로 검색하면 에러가 발생한다. 즉 이름만 존재하고 검색이 불가능한 뷰가 된다.
    --      ▷ 뷰 생성 시 거론한 존재하지 않는 테이블을 나중에 만들고 나면 뷰도 select 로 검색이 가능하다.
    -- ▶ NOFORCE
    --      ▷ 존재하지 않는 테이블에서 뷰를 만들 경우 뷰를 만들지 못한다.
    --      ▷ FORCE, NOFORCE 모두 생략 시 NOFORCE 설정과 같다.
    -- ▶ WITH READ ONLY
    --      ▷ 뷰에 대해 입력, 수정, 삭제 작업이 불가능하다.
    --      ▷ 생략 시 특정 조건하에 하나의 테이블에서 만들어진 뷰에 대해서는 입력, 수정, 삭제 작업을 할 수 있고
    --        실존 테이블에 입력, 수정, 삭제가 된다.
    -- ▶ WITH CHECK OPTION
    --      ▷ 뷰의 WHERE 구문 조건에 위배되는 방향으로 INSERT, UPDATE 작업은 불가능하다.
    --      ▷ <예> 뷰에 where salary >= 6000 이 있을 경우 salary >= 6000 조건에 위배되는 방향으로 입력, 수정이 불가능하다.
    --          예를 들어 salary >= 6000 조건에 해당하는 행의 salary를 6000 미만으로 입력, 수정하는 행위는 불가능하다.
    -- ▶ View 안에 나오는 select 문에서 실존 테이블에 존재하지 않는 컬럼은 반드시 별칭을 써야한다.


    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q.142 employee 테이블로부터 salary 컬럼만 제외하고 볼 수 있는 뷰 emloyee_vw1를 생성하면?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm

        create view employee_vw1 as select emp_no, emp_name, dep_no, jikup, hire_date, jumin_num, phone_num, mgr_emp_no from EMPLOYEE;

        select *from employee_vw1;


    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q.143 뷰 employee_vw1 에 데이터 '이승엽', 40, '과장', '1990-09-01', '7811231452719', '01090056376', 1 을 입력하면?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    insert into employee_vw1 (emp_no, emp_name, dep_no, jikup, hire_date, jumin_num, phone_num, mgr_emp_no

    )values(
            (select nvl(max(emp_no),0)+1 from employee_vw1),'이승엽', 40, '과장', to_date('1990-09-01','YYYY-MM-DD'), '7811231452719', '01090056376', 1
           );
    select * from employee_vw1 order by EMP_NO desc; -- 뷰를 검색
    select * from employee order by EMP_NO desc;     -- employee 테이블 검색
    commit ;

    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q.144 뷰 employee_vw1 에서 주민번호 '7811231452719', 직원명 '이승엽' 의 직급을 부장으로 수정하면?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm

    update employee_vw1 set JIKUP = '부장' where EMP_NAME='이승엽' and JUMIN_NUM ='7811231452719';

    select * from employee_vw1 order by EMP_NO desc; -- 뷰를 검색
    select * from employee order by EMP_NO desc;     -- employee 테이블 검색


    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q.145 뷰 employee_vw1 에서 주민번호 '7811231452719', 직원명 '이승엽' 인 직원을 제거하면?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm

    delete from employee_vw1 where JUMIN_NUM='7811231452719';


    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q.146 [직원번호], [직원명], [직급], [부서번호], [부서명] 을 출력하는 뷰 employee_vw2를 생성하면?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm

        create view employee_vw2 as
            select
                e.emp_no, e.emp_name, e.jikup, e.dep_no, d.DEP_NAME
            from dept d, employee e
            where d.DEP_NO = e.DEP_NO;

        select * from employee_vw2;


    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q.147 부서별, 직급별로 [부서번호], [부서명], [직급], [평균연봉] 을 출력하는 뷰 employee_vw3을 생성하면?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
        create view employee_vw3 as
            select
                   e.dep_no         "dep_no"
                 , d.dep_name       "dep_name"
                 , e.jikup          "jikup"
                 , round(avg(e.SALARY))    "avg_salary"
            from  employee e , dept d
            where d.DEP_NO = e.DEP_NO
            group by  d.dep_name, e.dep_no, e.jikup
            order by 1,4 desc;

        select * from employee_vw3;
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q.148 다음 뷰에 대한 질문에 대답하면?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    create view employee_vw4 as
        select emp_no, emp_name, dep_no from EMPLOYEE;
    ---------------------------------------------------------------
        -- <문> 아래의 코딩은 성공하나 실패하나?
        insert into employee_vw4 values(30, '사오순', 40);
            -- ▶︎ 실패한다. 뷰 employee_vw4 에 입력되는 데이터는 employee 테이블에 입력되는 것과 같다.
            -- employee 테이블에서 not null, unique 등의 제약조건에 위배되기 때문에 못들어 간다.

    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q.149 다음 뷰에 대한 질문에 대답하면?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
        insert into dept values(60,'전략부','부산');
        ----------------------------------------------------------
        create view dept_vw1 as
            select dep_no, dep_name, loc from DEPT
            where dep_no=60 with check option; -- where 조건을 위배하는 것은 보이지 않겠다.
        ----------------------------------------------------------
        --<문> 아래의 코딩을 실행하면?
        update dept_vw1 set dep_no=70 where dep_no=60;
             -- ▶︎ 실패한다. WITH CHECK OPTION 이 있으면 where 조건에 위배되는 입력, 수정은 불가능하다.
             -- ▶︎ 즉 dept_vw1 란 이름의 view 의 목적은 60번 부서를 검색하는게 목적이므로 이 목적을 위배하는 행위는 하지마라.
             --   위배하면 60번 부서를 검색하지 못하게 되니까 위배하지 마라의 의미.
        ----------------------------------------------------------
        --<문> 아래의 코딩을 실행하면?
        insert into dept_vw1 values(70,'미래부','대구'); -- 을 실행하면?
            -- ▶︎ 실패한다. WITH CHECK OPTION 이 있으면 where 조건에 위배되는 입력, 수정은 불가능하다.
            -- ▶︎ 70번 부서 입력해봤자 dept_vw1이란 이름의 view 를 통해 볼 수 없으므로 이런 짓 하지마라.
        ----------------------------------------------------------
        --<문> 아래의 코딩을 실행하면?
        delete from dept_vw1 where dep_no=60;
            -- ▶︎ 성공. WITH CHECK OPTION 이 있으면 where 조건에 위배되는 입력, 수정만 불가능하다. 삭제는 적용되지 않는다.

    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    -- Q.150 다음 뷰에 대한 질문에 대답하면?
    --mmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmmm
    ----------------------------------------------------------
        create view employee_vw6 as
            select emp_no, emp_name, dep_no, jikup, salary  from employee where salary>=3000 WITH CHECK OPTION;
        ----------------------------------------------------------
        --<문> 아래의 코딩을 실행하면?
        update employee_vw6 set salary=salary*0.95 where salary>=3000;
        -- ▶︎ 실패한다. WITH CHECK OPTION 이 있으면 where 조건에 위배되는 입력, 수정은 불가능하다.
        -- ▶︎ 수정후 3000만원 밑으로 내려가면 또 where 조건을 위배되서 검색이 안되므로 안됨
        ----------------------------------------------------------
        --<문> 아래의 코딩을 실행하면?
        update employee_vw6 set salary=salary*1.95 where salary>=3000;
        -- ▶︎ 성공. 업데이트로 3000 미만으로 내려가지 않아 조건에 위배되지 않으므로 성공한다.
