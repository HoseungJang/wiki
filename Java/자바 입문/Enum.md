자바에서 enum을 사용할 수 있다.

enum은 온전히 기능을 갖춘 클래스이다. enum은 내부적으로 클래스로 변환된다.

```java
enum Color {
    RED, BLUE, GREEN
}

// enum Color는 내부적으로 아래와 같음.
public class Color {
    public static final Color RED = new Color();
    public static final Color BLUE = new Color();
    public static final Color GREEN = new Color();
}
```

따라서 enum에 생성자를 만들고, 상수와 연결시키는 것도 가능하다. 메소드도 자유롭게 만들 수 있고, 따라서 enum과 연결된 상수를 활용할 수 있다.

```java
enum Flag {
    Y("1", true),
    N("0", false);

    private final String str;
    private final boolean bool;
    
    private Flag(String str, boolean bool) {
        this.str = str;
        this.bool = bool;
    }
    
    public String asString() {
        return this.str;
    }

    public boolean asBoolean() {
        return this.bool;
    }
}

public class Example {
    public static void main(String[] args) {
        Flag flag1 = Flag.Y;
        Flag flag2 = Flag.N;
        
        System.out.println(flag1.asString()); // "1"
        System.out.println(flag1.asBoolean()); // true

        System.out.println(flag2.asString()); // "0"
        System.out.println(flag2.asBoolean()); // false
    }
}
```
