---
title: TIL.03) CSS Layout
date: 2020-07-27 02:07:66
category: TIL 
thumbnail: './images/CSS3.png'
draft: false
---

![](./images/CSS3.png)

# position
## relative
- 기본위치 (static) 기준으로 좌표 프로퍼티의 값을 참조하여 위치를 이동시킨다.
- 프로퍼티는 top, bottom, left, right를 사용가능하다.
- 프로퍼티가 없으면 static과 동일하다.

## absolute
- 부모위치 기준으로 좌표 프로퍼티의 값을 참조하여 위치를 이동시킨다.
- 다만, 부모의 위치가 static일 경우는 조상의 위치를 참조한다.
- 조상의 위치도 static일 경우는 document body의 위치를 참조한다.
- 따라서 부모요소에 relative가 정의되어 있어야 absolute를 사용할수 있다.
- 프로퍼티는 top, bottom, left, right를 사용가능하다.

## fixed
- 브라우저의 보이는 부분 (viewport) 기준으로 좌표 프로퍼티의 값을 참조하여 요소를 위치시킨다.
- 따라서 스크롤이 되더라도 항상 같은위치에 요소가 노출되게 된다.
- 프로퍼티는 top, bottom, left, right를 사용한다.

# display

## block
- header, footer, p, li, table, div, h1 등이 해당된다.
- 새 줄로 시작하여 좌우로 늘어나는 형태로 해당 요소에는 다른 요소를 붙일수 없다.
- inline 요소에 해당 속성을 부여하면 block 형태로 변한다.

## inline
- span, a, img 등이 해당된다.
- 단락 중간에 사용 가능하므로 inline 요소 끼리는 같은 줄에서 서로 옆에 위치할수 있게된다.

## inline-block
- block 요소에 해당 속성을 부여하면 inline 속성으로 변하게 된다.

# float
- 이미지 주위에 텍스트를 배치하기 위해 사용하나 웹페이지의 레이아웃을 잡기위해서도 널리 사용됨.
- left, right, none중 하나를 값으로 사용한다.
- float속성을 가진 요소는 부모가 그 요소의 높이를 알수 없기 때문에 요소가 부모와 분리되는 현상이 발생한다.
- 따라서 clear를 통해 해결해야 한다.
## clear
- float 사용 후 부유한 요소를 정리하는데 사용한다.
- 프로퍼티는 left, right, both 사용한다.

