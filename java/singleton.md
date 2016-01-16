#单例

##~~饿汉模式~~

```java

public class AmericaPresident {
	private static final AmericaPresident thePresident = new AmericaPresident();
 
	private AmericaPresident() {}
 
	public static AmericaPresident getPresident() {
		return thePresident;
	}
}
```

##~~懒汉模式(Double check)~~

```java
public class AmericaPresident {
	private static volatile AmericaPresident thePresident = null; // volatile is important!
 
	private AmericaPresident() {
		// to prevent instantiating by Reflection call
		if (thePresident != null) {
      			throw new IllegalStateException("Already initialized.");
    		}
	}
 
	public static AmericaPresident getPresident() {
		// local variable increases performance by 25 percent
    		// Joshua Bloch "Effective Java, Second Edition", p. 283-284
		AmericaPresident result = thePresident;
		if (result == null) { // First check
			synchronized(AmericaPresident.class) {
				result = thePresident;
			 	if (result == null){ // Double check
					thePresident = result = new AmericaPresident();
				}
			}
		}
		return result;
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

##~~静态内部类~~

```java
class AmericaPresident {

    private AmericaPresident() {}

    private static class AmericaPresidentHolder {
        public static AmericaPresident thePresident = new AmericaPresident();
    }

    public static AmericaPresident getPresident() {
        return AmericaPresidentHolder.thePresident;
    }
}
```
