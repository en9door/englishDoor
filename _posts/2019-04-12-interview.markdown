---
layout: post
title:  "메소드 시그니처(Method signature)"
date:   2019-04-11 21:20:32
author: Dongy
categories: Java
cover:
---

## 메소드 시그니처(Method signature)
컴파일러가 메소드를 구분할 수 있게 해주는 서명(Signature)입니다.<br>
<br>
자바에서는 메소드의 이름과 파라미터 만을 메소드의 시그니처(Method Signature)라고 합니다.<br>
<br>
아래 코드를 살펴보면 <br>
draw() 메소드들은 메소드의 이름은 같지만 파라미터의 갯수와 타입이 달라 여러 번 선언할 수 있습니다. (메소드 오버로딩)<br>
<br>


### Code
```

public class Pen {
    
    public void draw(String s) {
        //…
    }

    public void draw(int i) {
        //…
    }

    public void draw(double f) {
        //…
    }

    public void draw(int i, double f) {
        //…
    }

    //위 draw 메소드와 시그니처가 같아 선언 불가능
    //public int draw(int i, double f) {
    //    //…
	//  return 0;
    //}

}

```


단, 리턴타입은 메소드 시그니처에 포함되지 않습니다.<br>
따라서 컴파일러는 메소드를 구분할 때 리턴 타입을 고려하지 않기 때문에 <br>
서로 다른 리턴 타입을 가져도 동일한 시그니처를 가진 2개의 메소드를 선언할 수 없습니다.<br>
<br>