#单例

##饿汉模式

```java

public class AmericaPresident {
	private static final AmericaPresident thePresident = new AmericaPresident();
 
	private AmericaPresident() {}
 
	public static AmericaPresident getPresident() {
		return thePresident;
	}
}
```

##懒汉模式

```java
public class AmericaPresident {
	private static volatile AmericaPresident thePresident = null; // volatile is important!
 
	private AmericaPresident() {}
 
	public static AmericaPresident getPresident() {
		if (thePresident == null) { // First check
			synchronized(AmericaPresident.class) {
			 	if (thePresident == null){ // Double check
					thePresident = new AmericaPresident();
				}
			}
		}
		return thePresident;
	}
}
```

##枚举实现

```java
public enum AmericaPresident{
	INSTANCE;
 
	public static void doSomething(){
		//do something
	}
}
```

##静态内部类

```java
class AmericaPresident {
    private static class AmericaPresidentHolder {
        public static AmericaPresident thePresident = new AmericaPresident();
    }

    public static AmericaPresident getPresident() {
        return AmericaPresidentHolder.thePresident;
    }
}
```
