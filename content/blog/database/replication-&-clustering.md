---
title: Replication & Clustering
date: 2021-02-07 20:02:47
category: Database
thumbnail: '../TIL/images/database.png'
draft: false
---

![](../TIL/images/database.png)

# Replication

![https://yongineer.duckdns.org/database/01.png](https://yongineer.duckdns.org/database/01.png)

## Replication은 무엇인가?

- 여러 개의 DB를 권한에 따라 Master Node와 Slave Node로 구성되는 것이 일반적이다.
- Master Node는 쓰기 작업만 담당
- Slave Node는 읽기 작업만 담당
- 비동기 방식으로 노드들 간 데이터를 동기화 한다.

## Replication의 처리 순서

1. Master Node에 쓰기 트랜잭션이 수행된다.
2. Master Node는 데이터를 저장하고 트랜잭션에 대한 로그를 BIN log에 기록한다.
3. Slave Node의 IO Thread는 Master Node의 BIN log를 Relay log에 복사한다.
4. Slave Node의 SQL Thread는 Relay log를 한 row씩 읽어가며 데이터를 저장한다.

## Replication의 장점

- DB요청의 대부분이 읽기 작업이므로 읽기와 쓰기를 분리하는 것만으로도 성능을 높일 수 있다.
- 비동기 방식으로 지연시간이 짧다.

## Replication의 단점

- 노드들 의 데이터 동기화가 보장되지 않아 일관성 있는 데이터를 얻지 못할 수 있다
- Master가 다운되면 복구 및 대처가 어렵다.

# Clustering

![https://yongineer.duckdns.org/database/02.png](https://yongineer.duckdns.org/database/02.png)

## Clustering은 무엇인가?

- 여러개의 DB를 수평적인 구조로 구축하는 방식
- 분산환경을 구성하여 Fail Over 시스템을 구축하기 위해서 사용
- 동기방식으로 Node들 간의 데이터를 동기화함

## Clustering의 처리 순서

1. 1개의 Node에 쓰기 트랜잭션이 수행되고 Commit을 실행한다.
2. 디스크에 내용을 쓰기전에 다른 노드로 데이터의 복제를 요청한다.
3. 다른 Node에서 복제 요청을 수락했다는 OK신호를 보내고, 디스크에 쓰기 시작한다.
4. 다른 Node로 부터 OK신호를 받으면 실제 디스크에 데이터를 저장한다.

## Clustering 장점

- Node들 간의 데이터를 동기화하여 항상 일관성 있는 데이터를 얻을 수 있다.
- 1개의 Node가 죽어도 다른 Node가 살아있다면 시스템을 계속 장애없이 운영할 수 있다.

## Clustering 단점

- 여러 Node들 간의 데이터를 동기화하는 시간이 필요하므로 Replication에 비해 쓰기 성능이 떨어진다.
- 장애가 전파된 경우 처리가 쉽지 않다.
- 데이터 동기화에 의해 스케일링에 한계가 있다.

## Clustering 구성하기

### Cluster하기 적합한 Table

- 조회가 자주 발생하고 수정이 거의 발생하지 않는 테이블
- 컬럼안의 많은 중복 데이터를 가지는 테이블
- 자주 Join이 되는 테이블

### Cluster Key선정하기

### 👍

- 데이터 값의 범위가 큰 column
- 테이블간의 조인에 사용되는 column

### 👎

- 특정 데이터 값이 적은 column
- 자주 데이터 수정이 발생하는 column
- LONG, LONG RAW column은 포함할 수 없다.

<style>
/* webkit printing magic: print all background colors */
html {
	-webkit-print-color-adjust: exact;
}
* {
	box-sizing: border-box;
	-webkit-print-color-adjust: exact;
}
<p>html,
body {
margin: 0;
padding: 0;
}
@media only screen {
body {
margin: 2em auto;
max-width: 900px;
color: rgb(55, 53, 47);
}
}</p>
<p>body {
line-height: 1.5;
white-space: pre-wrap;
}</p>
<p>a,
a.visited {
color: inherit;
text-decoration: underline;
}</p>
<p>.pdf-relative-link-path {
font-size: 80%;
color: #444;
}</p>
<p>h1,
h2,
h3 {
letter-spacing: -0.01em;
line-height: 1.2;
font-weight: 600;
margin-bottom: 0;
}</p>
<p>.page-title {
font-size: 2.5rem;
fot-weight: 700;
margin-top: 0;
margin-bottom: 0.75em;
}</p>
<p>h1 {
font-size: 1.875rem;
margin-top: 1.875rem;
}</p>
<p>2 {
font-size: 1.5rem;
margin-top: 1.5rem;
}</p>
<p>h3 {
font-size: 1.25rem;
margin-top: 1.25rem;
}</p>
<p>.source {
border: 1px solid #ddd;
border-radius: 3px;
padding: 1.5em;
word-break: break-all;
}</p>
<p>.callout {
border-radius: 3px;
padding: 1rem;
}</p>
<p>figure {
margin: 1.25em 0;
page-break-inside: avoid;
}</p>
<p>figcaption {
opacity: 0.5;
font-size: 85%;
margin-top: 0.5em;
}</p>
<p>mark {
background-color: transparent;
}</p>
<p>.indented {
padding-left: 1.5em;
}</p>
<p>hr {
background: transparent;
display: block;
width: 100%;
height: 1px;
visibility: visible;
border: none;
border-bottom: 1px solid rgba(55, 53, 47, 0.09);
}</p>
<p>img {
max-width: 100%;
}</p>
<p>@media only print {
img {
max-height: 100vh;
object-fit: contain;
}
}</p>
<p>@page {
margin: 1in;
}</p>
<p>.collection-content {
font-size: 0.875rem;
}</p>
<p>.column-list {
display: flex;
justify-content: space-between;
}</p>
<p>.column {
padding: 0 1em;
}</p>
<p>.column:first-child {
padding-left: 0;
}</p>
<p>.column:last-child {
padding-right: 0;
}</p>
<p>.table<em>of</em>contents-item {
display: block;
font-size: 0.875rem;
line-height: 1.3;
padding: 0.125rem;
}</p>
<p>.table<em>of</em>contents-indent-1 {
margin-left: 1.5rem;
}</p>
<p>.table<em>of</em>contents-indent-2 {
margin-left: 3rem;
}</p>
<p>.table<em>of</em>contents-indent-3 {
margin-left: 4.5rem;
}</p>
<p>.table<em>of</em>contents-link {
text-decoration: none;
opacity: 0.7;
border-bottom: 1px solid rgba(55, 53, 47, 0.18);
}</p>
<p>table,
th,
td {
border: 1px solid rgba(55, 53, 47, 0.09);
border-collapse: collapse;
}</p>
<p>table {
border-left: none;
border-right: none;
}</p>
<p>th,
td {
font-weight: normal;
padding: 0.25em 0.5em;
line-height: 1.5;
min-height: 1.5em;
text-align: left;
}</p>
<p>th {
color: rgba(55, 53, 47, 0.6);
}</p>
<p>ol,
ul {
margin: 0;
margin-block-start: 0.6em;
margin-block-end: 0.6em;
}</p>
<p>li > ol:first-child,
li > ul:first-child {
margin-block-start: 0.6em;
}</p>
<p>ul > li {
list-style: disc;
}</p>
<p>ul.to-do-list {
text-indent: -1.7em;
}</p>
<p>ul.to-do-list > li {
list-style: none;
}</p>
<p>.to-do-children-checked {
text-decoration: line-through;
opacity: 0.375;
}</p>
<p>ul.toggle > li {
list-style: none;
}</p>
<p>ul {
padding-inline-start: 1.7em;
}</p>
<p>ul > li {
padding-left: 0.1em;
}</p>
<p>ol {
padding-inline-start: 1.6em;
}</p>
<p>ol > li {
padding-left: 0.2em;
}</p>
<p>.mono ol {
padding-inline-start: 2em;
}</p>
<p>.mono ol > li {
text-indent: -0.4em;
}</p>
<p>.toggle {
padding-inline-start: 0em;
list-style-type: none;
}</p>
<p>/* Indent toggle children */
.toggle > li > details {
padding-left: 1.7em;
}</p>
<p>.toggle > li > details > summary {
margin-left: -1.1em;
}</p>
<p>.selected-value {
display: inline-block;
padding: 0 0.5em;
background: rgba(206, 205, 202, 0.5);
border-radius: 3px;
margin-right: 0.5em;
margin-top: 0.3em;
margin-bottom: 0.3em;
white-space: nowrap;
}</p>
<p>.collection-title {
display: inline-block;
margin-right: 1em;
}</p>
<p>time {
opacity: 0.5;
}</p>
<p>.icon {
display: inline-block;
max-width: 1.2em;
max-height: 1.2em;
text-decoration: none;
vertical-align: text-bottom;
margin-right: 0.5em;
}</p>
<p>img.icon {
border-radius: 3px;
}</p>
<p>.user-icon {
width: 1.5em;
height: 1.5em;
border-radius: 100%;
margin-right: 0.5rem;
}</p>
<p>.user-icon-inner {
font-size: 0.8em;
}</p>
<p>.text-icon {
border: 1px solid #000;
text-align: center;
}</p>
<p>.page-cover-image {
display: block;
object-fit: cover;
width: 100%;
height: 30vh;
}</p>
<p>.page-header-icon {
font-size: 3rem;
margin-bottom: 1rem;
}</p>
<p>.page-header-icon-with-cover {
margin-top: -0.72em;
margin-left: 0.07em;
}</p>
<p>.page-header-icon img {
border-radius: 3px;
}</p>
<p>.link-to-page {
margin: 1em 0;
padding: 0;
border: none;
font-weight: 500;
}</p>
<p>p > .user {
opacity: 0.5;
}</p>
<p>td > .user,
td > time {
white-space: nowrap;
}</p>
<p>input[type=“checkbox”] {
transform: scale(1.5);
margin-right: 0.6em;
vertical-align: middle;
}</p>
<p>p {
margin-top: 0.5em;
margin-bottom: 0.5em;
}</p>
<p>.image {
border: none;
margin: 1.5em 0;
padding: 0;
border-radius: 0;
text-align: center;
}</p>
<p>.code,
code {
background: rgba(135, 131, 120, 0.15);
border-radius: 3px;
padding: 0.2em 0.4em;
border-radius: 3px;
font-size: 85%;
tab-size: 2;
}</p>
<p>code {
color: #eb5757;
}</p>
<p>.code {
padding: 1.5em 1em;
}</p>
<p>.code-wrap {
white-space: pre-wrap;
word-break: break-all;
}</p>
<p>.code > code {
background: none;
padding: 0;
font-size: 100%;
color: inherit;
}</p>
<p>blockquote {
font-size: 1.25em;
margin: 1em 0;
padding-left: 1em;
border-left: 3px solid rgb(55, 53, 47);
}</p>
<p>.bookmark {
text-decoration: none;
max-height: 8em;
padding: 0;
display: flex;
width: 100%;
align-items: stretch;
}</p>
<p>.bookmark-title {
font-size: 0.85em;
overflow: hidden;
text-overflow: ellipsis;
height: 1.75em;
white-space: nowrap;
}</p>
<p>.bookmark-text {
display: flex;
flex-direction: column;
}</p>
<p>.bookmark-info {
flex: 4 1 180px;
padding: 12px 14px 14px;
display: flex;
flex-direction: column;
justify-content: space-between;
}</p>
<p>.bookmark-image {
width: 33%;
flex: 1 1 180px;
display: block;
position: relative;
object-fit: cover;
border-radius: 1px;
}</p>
<p>.bookmark-description {
color: rgba(55, 53, 47, 0.6);
font-size: 0.75em;
overflow: hidden;
max-height: 4.5em;
word-break: break-word;
}</p>
<p>.bookmark-href {
font-size: 0.75em;
margin-top: 0.25em;
}</p>
<p>.sans { font-family: -apple-system, BlinkMacSystemFont, “Segoe UI”, Helvetica, “Apple Color Emoji”, Arial, sans-serif, “Segoe UI Emoji”, “Segoe UI Symbol”; }
.code { font-family: “SFMono-Regular”, Consolas, “Liberation Mono”, Menlo, Courier, monospace; }
.serif { font-family: Lyon-Text, Georgia, YuMincho, “Yu Mincho”, “Hiragino Mincho ProN”, “Hiragino Mincho Pro”, “Songti TC”, “Songti SC”, “SimSun”, “Nanum Myeongjo”, NanumMyeongjo, Batang, serif; }
.mono { font-family: iawriter-mono, Nitti, Menlo, Courier, monospace; }
.pdf .sans { font-family: Inter, -apple-system, BlinkMacSystemFont, “Segoe UI”, Helvetica, “Apple Color Emoji”, Arial, sans-serif, “Segoe UI Emoji”, “Segoe UI Symbol”, ‘Twemoji’, ‘Noto Color Emoji’, ‘Noto Sans CJK SC’, ‘Noto Sans CJK KR’; }</p>
<p>.pdf .code { font-family: Source Code Pro, “SFMono-Regular”, Consolas, “Liberation Mono”, Menlo, Courier, monospace, ‘Twemoji’, ‘Noto Color Emoji’, ‘Noto Sans Mono CJK SC’, ‘Noto Sans Mono CJK KR’; }</p>
<p>.pdf .serif { font-family: PT Serif, Lyon-Text, Georgia, YuMincho, “Yu Mincho”, “Hiragino Mincho ProN”, “Hiragino Mincho Pro”, “Songti TC”, “Songti SC”, “SimSun”, “Nanum Myeongjo”, NanumMyeongjo, Batang, serif, ‘Twemoji’, ‘Noto Color Emoji’, ‘Noto Sans CJK SC’, ‘Noto Sans CJK KR’; }</p>
<p>.pdf .mono { font-family: PT Mono, iawriter-mono, Nitti, Menlo, Courier, monospace, ‘Twemoji’, ‘Noto Color Emoji’, ‘Noto Sans Mono CJK SC’, ‘Noto Sans Mono CJK KR’; }</p>
<p>.highlight-default {
}
.highlight-gray {
color: rgb(155,154,151);
}
.highlight-brown {
color: rgb(100,71,58);
}
.highlight-orange {
color: rgb(217,115,13);
}
.highlight-yellow {
color: rgb(223,171,1);
}
.highlight-teal {
color: rgb(15,123,108);
}
.highlight-blue {
color: rgb(11,110,153);
}
.highlight-purple {
color: rgb(105,64,165);
}
.highlight-pink {
color: rgb(173,26,114);
}
.highlight-red {
color: rgb(224,62,62);
}
.highlight-gray<em>background {
background: rgb(235,236,237);
}
.highlight-brown</em>background {
background: rgb(233,229,227);
}
.highlight-orange<em>background {
background: rgb(250,235,221);
}
.highlight-yellow</em>background {
background: rgb(251,243,219);
}
.highlight-teal<em>background {
background: rgb(221,237,234);
}
.highlight-blue</em>background {
background: rgb(221,235,241);
}
.highlight-purple<em>background {
background: rgb(234,228,242);
}
.highlight-pink</em>background {
background: rgb(244,223,235);
}
.highlight-red<em>background {
background: rgb(251,228,228);
}
.block-color-default {
color: inherit;
fill: inherit;
}
.block-color-gray {
color: rgba(55, 53, 47, 0.6);
fill: rgba(55, 53, 47, 0.6);
}
.block-color-brown {
color: rgb(100,71,58);
fill: rgb(100,71,58);
}
.block-color-orange {
color: rgb(217,115,13);
fill: rgb(217,115,13);
}
.block-color-yellow {
color: rgb(223,171,1);
fill: rgb(223,171,1);
}
.block-color-teal {
color: rgb(15,123,108);
fill: rgb(15,123,108);
}
.block-color-blue {
color: rgb(11,110,153);
fill: rgb(11,110,153);
}
.block-color-purple {
color: rgb(105,64,165);
fill: rgb(105,64,165);
}
.block-color-pink {
color: rgb(173,26,114);
fill: rgb(173,26,114);
}
.block-color-red {
color: rgb(224,62,62);
fill: rgb(224,62,62);
}
.block-color-gray</em>background {
background: rgb(235,236,237);
}
.block-color-brown<em>background {
background: rgb(233,229,227);
}
.block-color-orange</em>background {
background: rgb(250,235,221);
}
.block-color-yellow<em>background {
background: rgb(251,243,219);
}
.block-color-teal</em>background {
background: rgb(221,237,234);
}
.block-color-blue<em>background {
background: rgb(221,235,241);
}
.block-color-purple</em>background {
background: rgb(234,228,242);
}
.block-color-pink<em>background {
background: rgb(244,223,235);
}
.block-color-red</em>background {
background: rgb(251,228,228);
}
.select-value-color-default { background-color: rgba(206,205,202,0.5); }
.select-value-color-gray { background-color: rgba(155,154,151, 0.4); }
.select-value-color-brown { background-color: rgba(140,46,0,0.2); }
.select-value-color-orange { background-color: rgba(245,93,0,0.2); }
.select-value-color-yellow { background-color: rgba(233,168,0,0.2); }
.select-value-color-green { background-color: rgba(0,135,107,0.2); }
.select-value-color-blue { background-color: rgba(0,120,223,0.2); }
.select-value-color-purple { background-color: rgba(103,36,222,0.2); }
.select-value-color-pink { background-color: rgba(221,0,129,0.2); }
.select-value-color-red { background-color: rgba(255,0,26,0.2); }</p>
<p>.checkbox {
display: inline-flex;
vertical-align: text-bottom;
width: 16;
height: 16;
background-size: 16px;
margin-left: 2px;
margin-right: 5px;
}</p>
<p>.checkbox-on {
background-image: url(“data:image/svg+xml;charset=UTF-8,%3Csvg%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%0A%3Crect%20width%3D%2216%22%20height%3D%2216%22%20fill%3D%22%2358A9D7%22%2F%3E%0A%3Cpath%20d%3D%22M6.71429%2012.2852L14%204.9995L12.7143%203.71436L6.71429%209.71378L3.28571%206.2831L2%207.57092L6.71429%2012.2852Z%22%20fill%3D%22white%22%2F%3E%0A%3C%2Fsvg%3E”);
}</p>
<p>.checkbox-off {
background-image: url(“data:image/svg+xml;charset=UTF-8,%3Csvg%20width%3D%2216%22%20height%3D%2216%22%20viewBox%3D%220%200%2016%2016%22%20fill%3D%22none%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%3E%0A%3Crect%20x%3D%220.75%22%20y%3D%220.75%22%20width%3D%2214.5%22%20height%3D%2214.5%22%20fill%3D%22white%22%20stroke%3D%22%2336352F%22%20stroke-width%3D%221.5%22%2F%3E%0A%3C%2Fsvg%3E”);
}

</style>

<article id="311a5fdf-4cba-409e-8b1a-b95f4cb86178" class="page sans"><header><h1 class="page-title">Cluster Index VS Non-Cluster Index</h1></header><div class="page-body"><table class="collection-content"><thead><tr><th style="width:90px">비교</th><th>Cluster Index</th><th>Non-Cluster Index</th></tr></thead><tbody><tr id="6f676ee5-8aee-4a0c-9683-a36d1a3f817b"><td class="cell-title"><a href="https://www.notion.so/6f676ee58aee4a0c9683a36d1a3f817b">차이</a></td><td class="cell-kXK@">물리적으로 행을 재배열한다.</td><td class="cell-pM}T">물리적으로 재배열 하지 않는다.</td></tr><tr id="67421108-9fd4-4b3e-b39a-6a1de6d20907"><td class="cell-title"><a href="https://www.notion.so/674211089fd44b3eb39a6a1de6d20907">크기</a></td><td class="cell-kXK@">인덱스 페이지 용량이 Non-Clusted Index 페이지 용량 보다 작다</td><td class="cell-pM}T">Clusted Index 페이지 용량 보다 용량이 크다.</td></tr><tr id="d0636e09-f0f5-4784-85ae-c50fc63df25a"><td class="cell-title"><a href="https://www.notion.so/d0636e09f0f5478485aec50fc63df25a">선택도</a></td><td class="cell-kXK@">30% 이내에서 사용해야 좋다.</td><td class="cell-pM}T">3% 이내에서 사용해야 좋다.</td></tr><tr id="eaf2680f-af7d-494e-bf63-2ab6016bf04a"><td class="cell-title"><a href="https://www.notion.so/eaf2680faf7d494ebf632ab6016bf04a">최대갯수</a></td><td class="cell-kXK@">테이블당 1개</td><td class="cell-pM}T">테이블당 249개</td></tr><tr id="336eb630-ede5-4d99-a20f-9166897495f6"><td class="cell-title"><a href="https://www.notion.so/336eb630ede54d99a20f9166897495f6">생성</a></td><td class="cell-kXK@">Primary Key 설정시, 해당 컬럼은 자동적으로 Cluster Index 생성</td><td class="cell-pM}T">Non-Cluster Index를 명시해야 생성 (Index page를 따로 만들며 로그 파일에 저장된다.)</td></tr></tbody></table></div></article>

# Active-Active VS Active-Standby

## Active-Active

- Cluster를 이루는 DB 서버들이 모두 Active한 상태로 두는것
- 하나의 서버가 죽어도 다른 서버가 역할을 바로 수행하여 중단되는 시간이 없다.
- 서버의 물리적인 양이 증가되면 CPU와 Memory와 같은 리소스가 증가하기 때문에 성능향상을 기대할 수 있다.
- 디스크를 공유하면 이에 따른 Bottleneck이 발생 할 수 있다.
- 다수의 서버를 운영해야 하므로 비용이 증가된다.

## Active-Standby ✨

- HA(High Availability) Clustering 구축 방법 중 가장 대표적인 것 중 하나.
- Active-Standby구조는 구성과 운영이 쉽다.
- 운영서버, Active서버, Standby서버로 구성된다.
- 한 서버가 Active상태로 서비스 중일 때, Standby 서버는 준비 상태로 대기중인 상태다.

### 구성종류

- Cold-Standby : 평소에는 Standby 서버가 대기 상태로 있다가 Active 서버가 다운되는 시점 작동
- Hot-Standby : 평소에도 Standby 서버가 작동

Hot-Standby가 전환시간이 짧지만 그만큼 비용이 증가된다. 항상 DB서버 2대를 사용하지만 실제 동작하는 것이 Active 서버라면 그 전환시간을 줄이기 위해 Hot-Standby를 구성하는 것은 비효율적이다.

### 동작 방식

1. Active 서버에 장애가 발생해 서비스를 못하게 되면.
2. Standby 서버에 있는 HA가 운영 서버에 장애 발생을 보고한다.
3. HA가 자동으로 Standby 서버에 모든 서비스를 올리고 서비스를 이어간다.
4. 기존 Active서버에 접속했던 Client는 접속이 끝어지겠지만, 재접속 시 바로 서비스를 이어 나갈 수 있다.
