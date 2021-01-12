---
title: Mongo DB? (feat. vs SQL)
date: 2021-01-13 03:01:06
category: MongoDB
thumbnail: 'https://yongnas.iptime.org/mongoDB/title.png'
draft: false
---
![https://yongnas.iptime.org/mongoDB/title.png](https://yongnas.iptime.org/mongoDB/title.png)

# Intro

- `C++`로 작성된 문서지향적 크로스 플랫폼 데이터베이서
- 뛰어난 확장성과 성능

## No SQL?

- SQL이 없는것이 아님
- Not Only SQL이 핵심
- 기존 RDBMS의 한계를 뛰어넘기 위해 만들어진 새로운 형태의 데이터 베이스
- 관계형 DB가 아니므로 고정된 스키마와 JOIN이 존재하지 않는다

## Document

- RDBMS의 레코드와 비슷한 개념
- 데이터 구조는 `Key:Value`형태로 이루어져 있다.

```json
{
	"_id" : ObjectId("5ffdd9cfb98851a7adad7e84"),
	"author" : "jim",
	"title" : "Brewing Methods"
}
```

> `_id`

- 12Bytes의 `hexadecimal`값
- 각 문서의 유일한 값을 제공하며 자동 생성되나 `insert`시 지정함으로써 커스텀이 가능하다.

    ```json
    > db.users.insert({"_id" : "jim", "email" : "jim@email.com"})
    { "_id" : "jim", "email" : "jim@email.com" }
    ```

- 자동 생성된`_id`는 `timestamp`, `machine id`, `process id`, 순차번호로 이루어져 있어 추가 될 때마다 값이 높아진다.

## Collection

- `Document`의 그룹
- 문서들이 `Collection`내부에 위치하고 있다.
- RDBMS의 `table`과 비슷하지만 스키마를 따로 갖고 있지 않는다.

## Database

- `Collection`들의 물리적인 컨테이너
- 각 데이터베이스는 파일시스템에 여러파일들로 저장된다.

# SQL과 비교해보기

![https://yongnas.iptime.org/mongoDB/3.jpg](https://yongnas.iptime.org/mongoDB/3.jpg)

<article id="6a878bf7-40c5-4d37-a0bf-520fd904ab44" class="page sans"><header><h1 class="page-title">SQL vs NoSQL</h1></header><div class="page-body"><table class="collection-content"><thead><tr><th><span class="icon property-icon"><svg viewBox="0 0 14 14" style="width:14px;height:14px;display:block;fill:rgba(55, 53, 47, 0.4);flex-shrink:0;-webkit-backface-visibility:hidden" class="typesTitle"><path d="M7.73943662,8.6971831 C7.77640845,8.7834507 7.81338028,8.8943662 7.81338028,9.00528169 C7.81338028,9.49823944 7.40669014,9.89260563 6.91373239,9.89260563 C6.53169014,9.89260563 6.19894366,9.64612676 6.08802817,9.30105634 L5.75528169,8.33978873 L2.05809859,8.33978873 L1.72535211,9.30105634 C1.61443662,9.64612676 1.2693662,9.89260563 0.887323944,9.89260563C0.394366197,9.89260563 0,9.49823944 0,9.00528169 C0,8.8943662 0.0246478873,8.7834507 0.0616197183,8.6971831 L2.46478873,2.48591549 C2.68661972,1.90669014 3.24119718,1.5 3.90669014,1.5 C4.55985915,1.5 5.12676056,1.90669014 5.34859155,2.48591549 L7.73943662,8.6971831 Z M2.60035211,6.82394366 L5.21302817,6.82394366 L3.90669014,3.10211268 L2.60035211,6.82394366 Z M11.3996479,3.70598592 C12.7552817,3.70598592 14,4.24823944 14,5.96126761 L14,9.07922535 C14,9.52288732 13.6549296,9.89260563 13.2112676,9.89260563 C12.8169014,9.89260563 12.471831,9.59683099 12.4225352,9.19014085 C12.028169,9.6584507 11.3257042,9.95422535 10.5492958,9.95422535 C9.60035211,9.95422535 8.47887324,9.31338028 8.47887324,7.98239437 C8.47887324,6.58978873 9.60035211,6.08450704 10.5492958,6.08450704 C11.3380282,6.08450704 12.040493,6.33098592 12.4348592,6.81161972 L12.4348592,5.98591549 C12.4348592,5.38204225 11.9172535,4.98767606 11.1285211,4.98767606 C10.6602113,4.98767606 10.2411972,5.11091549 9.80985915,5.38204225 C9.72359155,5.43133803 9.61267606,5.46830986 9.50176056,5.46830986 C9.18133803,5.46830986 8.91021127,5.1971831 8.91021127,4.86443662 C8.91021127,4.64260563 9.0334507,4.44542254 9.19366197,4.34683099 C9.87147887,3.90316901 10.6232394,3.70598592 11.3996479,3.70598592 Z M11.1778169,8.8943662 C11.6830986,8.8943662 12.1760563,8.72183099 12.4348592,8.37676056 L12.4348592,7.63732394 C12.1760563,7.29225352 11.6830986,7.11971831 11.1778169,7.11971831 C10.5616197,7.11971831 10.056338,7.45246479 10.056338,8.0193662 C10.056338,8.57394366 10.5616197,8.8943662 11.1778169,8.8943662 Z M0.65625,11.125 L13.34375,11.125 C13.7061869,11.125 14,11.4188131 14,11.78125 C14,12.1436869 13.7061869,12.4375 13.34375,12.4375 L0.65625,12.4375 C0.293813133,12.4375 4.43857149e-17,12.1436869 0,11.78125 C-4.43857149e-17,11.4188131 0.293813133,11.125 0.65625,11.125 Z"></path></svg></span>개념</th><th><span class="icon property-icon"><svg viewBox="0 0 14 14" style="width:14px;height:14px;display:block;fill:rgba(55, 53, 47, 0.4);flex-shrink:0;-webkit-backface-visibility:hidden" class="typesText"><path d="M7,4.56818 C7,4.29204 6.77614,4.06818 6.5,4.06818 L0.5,4.06818 C0.223858,4.06818 0,4.29204 0,4.56818 L0,5.61364 C0,5.88978 0.223858,6.11364 0.5,6.11364 L6.5,6.11364 C6.77614,6.11364 7,5.88978 7,5.61364 L7,4.56818 Z M0.5,1 C0.223858,1 0,1.223858 0,1.5 L0,2.54545 C0,2.8216 0.223858,3.04545 0.5,3.04545 L12.5,3.04545 C12.7761,3.04545 13,2.8216 13,2.54545 L13,1.5 C13,1.223858 12.7761,1 12.5,1 L0.5,1 Z M0,8.68182 C0,8.95796 0.223858,9.18182 0.5,9.18182 L11.5,9.18182 C11.7761,9.18182 12,8.95796 12,8.68182 L12,7.63636 C12,7.36022 11.7761,7.13636 11.5,7.13636 L0.5,7.13636 C0.223858,7.13636 0,7.36022 0,7.63636 L0,8.68182 Z M0,11.75 C0,12.0261 0.223858,12.25 0.5,12.25 L9.5,12.25 C9.77614,12.25 10,12.0261 10,11.75 L10,10.70455 C10,10.4284 9.77614,10.20455 9.5,10.20455 L0.5,10.20455 C0.223858,10.20455 0,10.4284 0,10.70455 L0,11.75 Z"></path></svg></span>SQL (MySQL)</th><th><span class="icon property-icon"><svg viewBox="0 0 14 14" style="width:14px;height:14px;display:block;fill:rgba(55, 53, 47, 0.4);flex-shrink:0;-webkit-backface-visibility:hidden" class="typesText"><path d="M7,4.56818 C7,4.29204 6.77614,4.06818 6.5,4.06818 L0.5,4.06818 C0.223858,4.06818 0,4.29204 0,4.56818 L0,5.61364 C0,5.88978 0.223858,6.11364 0.5,6.11364 L6.5,6.11364 C6.77614,6.11364 7,5.88978 7,5.61364 L7,4.56818 Z M0.5,1 C0.223858,1 0,1.223858 0,1.5 L0,2.54545 C0,2.8216 0.223858,3.04545 0.5,3.04545 L12.5,3.04545 C12.7761,3.04545 13,2.8216 13,2.54545 L13,1.5 C13,1.223858 12.7761,1 12.5,1 L0.5,1 Z M0,8.68182 C0,8.95796 0.223858,9.18182 0.5,9.18182 L11.5,9.18182 C11.7761,9.18182 12,8.95796 12,8.68182 L12,7.63636 C12,7.36022 11.7761,7.13636 11.5,7.13636 L0.5,7.13636 C0.223858,7.13636 0,7.36022 0,7.63636 L0,8.68182 Z M0,11.75 C0,12.0261 0.223858,12.25 0.5,12.25 L9.5,12.25 C9.77614,12.25 10,12.0261 10,11.75 L10,10.70455 C10,10.4284 9.77614,10.20455 9.5,10.20455 L0.5,10.20455 C0.223858,10.20455 0,10.4284 0,10.70455 L0,11.75 Z"></path></svg></span>NoSQL</th></tr></thead><tbody><tr id="68c10579-05a3-4cfb-a6be-43762720f8b7"><td class="cell-title"><a href="https://www.notion.so/Database-68c1057905a34cfba6be43762720f8b7">Database</a></td><td class="cell-_i@B">Database</td><td class="cell-\Mvy">Database</td></tr><tr id="d42db464-8af8-4490-9cc3-b2b477e1ba9b"><td class="cell-title"><a href="https://www.notion.so/Table-d42db4648af844909cc3b2b477e1ba9b">Table</a></td><td class="cell-_i@B">Table</td><td class="cell-\Mvy">Collection</td></tr><tr id="03d452ac-960e-4b5e-9a76-a604d0544874"><td class="cell-title"><a href="https://www.notion.so/Row-03d452ac960e4b5e9a76a604d0544874">Row</a></td><td class="cell-_i@B">Row / Record</td><td class="cell-\Mvy">Document / BSON</td></tr><tr id="56427042-a4d3-4758-a604-c3397da59c10"><td class="cell-title"><a href="https://www.notion.so/Column-56427042a4d34758a604c3397da59c10">Column</a></td><td class="cell-_i@B">Column</td><td class="cell-\Mvy">Key / Field</td></tr><tr id="6d6f1a18-b124-4580-a1a8-40809982538c"><td class="cell-title"><a href="https://www.notion.so/Join-6d6f1a18b1244580a1a840809982538c">Join</a></td><td class="cell-_i@B">Table Join</td><td class="cell-\Mvy">Embedded Documents / linking</td></tr><tr id="de3a6faa-794c-47d1-8a75-e69945c12581"><td class="cell-title"><a href="https://www.notion.so/Index-de3a6faa794c47d18a75e69945c12581">Index</a></td><td class="cell-_i@B">index</td><td class="cell-\Mvy">index</td></tr><tr id="c03197fd-9829-46d7-8cce-8adf2a17f9f6"><td class="cell-title"><a href="https://www.notion.so/Aggregattion-c03197fd982946d78cce8adf2a17f9f6">Aggregattion</a></td><td class="cell-_i@B">Aggregation (ex. GROUP BY)</td><td class="cell-\Mvy">Aggergation Framework</td></tr><tr id="af3b3081-b438-4fd2-af5e-ddbb9d00642a"><td class="cell-title"><a href="https://www.notion.so/Primary-Key-af3b3081b4384fd2af5eddbb9d00642a">Primary Key</a></td><td class="cell-_i@B">Primary Key (PK)</td><td class="cell-\Mvy">Primary Key (_id)</td></tr><tr id="5ecaf27c-b93c-4e27-b551-9273ee33c1f9"><td class="cell-title"><a href="https://www.notion.so/Database-Server-5ecaf27cb93c4e27b5519273ee33c1f9">Database Server</a></td><td class="cell-_i@B">mysqld</td><td class="cell-\Mvy">mongod</td></tr><tr id="2f54e0b8-a86c-4c2b-b7df-822aac4b52da"><td class="cell-title"><a href="https://www.notion.so/Database-Client-2f54e0b8a86c4c2bb7df822aac4b52da">Database Client</a></td><td class="cell-_i@B">mysql</td><td class="cell-\Mvy">mongo</td></tr></tbody></table></div></article> 
