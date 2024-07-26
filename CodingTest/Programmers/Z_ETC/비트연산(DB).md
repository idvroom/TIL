# 비트연산(DB)

``` sql
select * from skillcodes s ;
NAME      |CATEGORY |CODE |
----------+---------+-----+
C++       |Back End |    4|
JavaScript|Front End|   16|
Java      |Back End |  128|
Python    |Back End |  256|
C#        |Back End | 1024|
React     |Front End| 2048|
Vue       |Front End| 8192|
Node.js   |Back End |16384|

select * from developers d ;
ID  |FIRST_NAME|LAST_NAME |EMAIL                   |SKILL_CODE|
----+----------+----------+------------------------+----------+
D161|Carsen    |Garza     |carsen_garza@grepp.co   |      2048|
D162|Cade      |Cunningham|cade_cunningham@grepp.co|      8452|
D163|Luka      |Cory      |luka_cory@grepp.co      |     16384|
D164|Kelly     |Grant     |kelly_grant@grepp.co    |      1024|
D165|Jerami    |Edwards   |jerami_edwards@grepp.co |       400|

-- s.code = d.skill_code&s.code 연산 부분
select d.id, d.email, d.first_name, d.last_name, s.code, d.skill_code, s.name
from developers d join skillcodes s on s.code = d.skill_code&s.code
order by d.id;
ID  |EMAIL                   |FIRST_NAME|LAST_NAME |code |SKILL_CODE|name      |
----+------------------------+----------+----------+-----+----------+----------+
D161|carsen_garza@grepp.co   |Carsen    |Garza     | 2048|      2048|React     |
D162|cade_cunningham@grepp.co|Cade      |Cunningham|    4|      8452|C++       |
D162|cade_cunningham@grepp.co|Cade      |Cunningham|  256|      8452|Python    |
D162|cade_cunningham@grepp.co|Cade      |Cunningham| 8192|      8452|Vue       |
D163|luka_cory@grepp.co      |Luka      |Cory      |16384|     16384|Node.js   |
D164|kelly_grant@grepp.co    |Kelly     |Grant     | 1024|      1024|C#        |
D165|jerami_edwards@grepp.co |Jerami    |Edwards   |   16|       400|JavaScript|
D165|jerami_edwards@grepp.co |Jerami    |Edwards   |  128|       400|Java      |
D165|jerami_edwards@grepp.co |Jerami    |Edwards   |  256|       400|Python    |

-- 비트 변환
select conv(4,10,2), conv(256,10,2), conv(8192,10,2), conv(8452,10,2) from dual;
conv(4,10,2)|conv(256,10,2)|conv(8192,10,2)|conv(8452,10,2)|
------------+--------------+---------------+---------------+
100         |100000000     |10000000000000 |10000100000100 |

```

[참고 프로그래머스](https://school.programmers.co.kr/learn/courses/30/lessons/276035)