---
title: Mysql_Mariadb
tags: db
---

* ERROR 1101 (42000) at line 1020: BLOB, TEXT, GEOMETRY or JSON column 'aos' can't have a default value

MYSQL 버전차이. MariaDB에서 JSON은 LONGTEXT의 별칭이다.   
MySQL 8.0.13, MariaDB 10.2.1 해당 버전 이전에는 MYSQL에서 JSON 열에 대해 NULL 이외의 DEFAULT 값을 허용하지 않았습니다.   
MySQL 8.0.13, MariaDB 10.2.1 버전 이후에는 다른 DEFAULT 값도 허용.   
   
참고 : https://stackoverflow.com/questions/66311662/mysql-error-blob-text-geometry-or-json-colum-cant-have-default-value

* ERROR 1227 (42000) at line 1850: Access denied; you need (at least one of) the SUPER or SET_USER_ID privilege(s) for this operation

기본적으로 CREATE FUNCTION ...으로 생성하면 명령어를 친 유저 소유로 생성이 되지만, DEFINER를 사용하면 다른 유저의 소유로 생성할 수 있다.   
참고 : https://bae9086.tistory.com/286 https://myinfrabox.tistory.com/232

* Docker_MySQL Access denied for user 'root'@'172.17.0.1' (using password: YES)

참고 : https://csksoft.tistory.com/69 https://medium.com/tech-learn-share/docker-mysql-access-denied-for-user-172-17-0-1-using-password-yes-c5eadad582d3