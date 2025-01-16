---
layout: single
title: "Java_14_Adaptor와 default 메서드"
date: 2024-09-14
categories:
  - Java
tags:
  - Java
  - Adaptor
  - default method
---

<br>

---

## Adaptor

자바의 인터페이스는 메서드의 바디가 없다.

즉 코드 로직 없이 구현체에 의해 실체화될 수 있는 형태인데

인터페이스를 구현하게 되면 해당 인터페이스의 모든 추상 메서드를 오버라이딩해야하기 때문에

필요하지 않은 기능까지 구현해야하는 경우가 생길 수 있다.

이때 필요한 개념이 Adaptor 이다.

어댑터는 인터페이스를 미리 구현하여 빈 구현부를 만들어둔 클래스로

이 어댑터를 상속하게 되면 필요한 기능만을 사용할 수 있게 된다.

<br>

## default 메서드
이 점을 개선하기 위해 인터페이스 내에 default 키워드를 사용한 메서드를 정의할 수 있게 되었는데

dafault 키워드를 작성할 경우 구현부까지 작성할 수 있고 
abstract 메서드와 달리 오버라이딩을 강제하지 않는다.

구현부 작성이 가능해지면서 static으로 선언할 수 있게 되었고
함수 역할을 할 수 있게 되었다. 
(@FunctionalInterface)

<pre><code>
public interface Ability {
    static voie sayHello() {
        System.out.println("Hello");
    }
}

public Class Main {
    public static void main(String[] args) {
        Ability.sayHello(); // Hello
    }
}
</code></pre>

더욱 강력한 부분은 다중 상속이 불가능한 자바에서 
다중 구현을 통해 기능을 확장할 수 있다는 점이다.

<br>

<pre><code>
public interface Sleepable {
    static voie sleep() {
        System.out.println("Sleep");
    }
}

public interface Walkable {
    static voie walk() {
        System.out.println("Walk");
    }
}
public interface Eatable {
    static voie eat() {
        System.out.println("Eat");
    }
}

public Class Human implements Sleepable, Walkable, Eatable {

    public void active() {
        Human easehee = new Human();
        easehee.sleep();
        easehee.walk();
        easehee.eat();
    }
}
</code></pre>



<br>

<dt>
