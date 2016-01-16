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
	private static AmericaPresident thePresident;
 
	private AmericaPresident() {}
 
	public static AmericaPresident getPresident() {
		if (thePresident == null) {
			thePresident = new AmericaPresident();
		}
		return thePresident;
	}
}
```
