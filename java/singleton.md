#单例

https://github.com/iluwatar/java-design-patterns/tree/master/singleton/src/main/java/com/iluwatar/singleton

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


#现有框架内的实现

##Spring-framework

spring-framework/spring-jdbc/src/main/java/org/springframework/jdbc/datasource/embedded/HsqlEmbeddedDatabaseConfigurer.java

```java
final class HsqlEmbeddedDatabaseConfigurer extends AbstractEmbeddedDatabaseConfigurer {

	private static HsqlEmbeddedDatabaseConfigurer instance;

	private final Class<? extends Driver> driverClass;


	/**
	 * Get the singleton {@link HsqlEmbeddedDatabaseConfigurer} instance.
	 * @return the configurer
	 * @throws ClassNotFoundException if HSQL is not on the classpath
	 */
	@SuppressWarnings("unchecked")
	public static synchronized HsqlEmbeddedDatabaseConfigurer getInstance() throws ClassNotFoundException {
		if (instance == null) {
			instance = new HsqlEmbeddedDatabaseConfigurer( (Class<? extends Driver>)
					ClassUtils.forName("org.hsqldb.jdbcDriver", HsqlEmbeddedDatabaseConfigurer.class.getClassLoader()));
		}
		return instance;
	}


	private HsqlEmbeddedDatabaseConfigurer(Class<? extends Driver> driverClass) {
		this.driverClass = driverClass;
	}

	@Override
	public void configureConnectionProperties(ConnectionProperties properties, String databaseName) {
		properties.setDriverClass(this.driverClass);
		properties.setUrl("jdbc:hsqldb:mem:" + databaseName);
		properties.setUsername("sa");
		properties.setPassword("");
	}

}
```

spring-framework/spring-test/src/main/java/org/springframework/test/annotation/SystemProfileValueSource.java

```java
public class SystemProfileValueSource implements ProfileValueSource {

	private static final SystemProfileValueSource INSTANCE = new SystemProfileValueSource();


	/**
	 * Obtain the canonical instance of this ProfileValueSource.
	 */
	public static final SystemProfileValueSource getInstance() {
		return INSTANCE;
	}


	/**
	 * Private constructor, enforcing the singleton pattern.
	 */
	private SystemProfileValueSource() {
	}

	/**
	 * Get the <em>profile value</em> indicated by the specified key from the
	 * system properties.
	 * @see System#getProperty(String)
	 */
	@Override
	public String get(String key) {
		Assert.hasText(key, "'key' must not be empty");
		return System.getProperty(key);
	}

}
```

spring-framework/spring-aop/src/main/java/org/springframework/aop/framework/adapter/GlobalAdvisorAdapterRegistry.java

```java
public abstract class GlobalAdvisorAdapterRegistry {

	/**
	 * Keep track of a single instance so we can return it to classes that request it.
	 */
	private static AdvisorAdapterRegistry instance = new DefaultAdvisorAdapterRegistry();

	/**
	 * Return the singleton {@link DefaultAdvisorAdapterRegistry} instance.
	 */
	public static AdvisorAdapterRegistry getInstance() {
		return instance;
	}

	/**
	 * Reset the singleton {@link DefaultAdvisorAdapterRegistry}, removing any
	 * {@link AdvisorAdapterRegistry#registerAdvisorAdapter(AdvisorAdapter) registered}
	 * adapters.
	 */
	static void reset() {
		instance = new DefaultAdvisorAdapterRegistry();
	}

}
```