# Interview questions:

- **Differences interface vs abstract classes:**
  -  `abstract` keyword is used to create an `abstract class` and it can be used with methods also whereas `interface` keyword is used to create `interface` and it can’t be used with methods.
  - Subclasses use extends keyword to extend an abstract class and they need to provide implementation of all the declared methods in the abstract class unless the subclass is also an abstract class whereas subclasses use implements keyword to implement interfaces and should provide implementation for all the methods declared in the interface.
  - Abstract classes can have methods with implementation whereas interface provides absolute abstraction and can’t have any method implementations. Note that from Java 8 onwards, we can create default and static methods in interface that contains the method implementations. 
  - Abstract classes can have constructors but interfaces can’t have constructors.
  - Abstract class have all the features of a normal java class except that we can’t instantiate it. We can use abstract keyword to make a class abstract but interfaces are a completely different type and can have only public static final constants and method declarations.
  -  Abstract classes methods can have access modifiers as public, private, protected, static but interface methods are implicitly public, private (static, non-static - Since Java 9) and abstract, we can’t use any other access modifiers with interface methods.
  -  A subclass can extend only one abstract class but it can implement multiple interfaces.
  -  Abstract classes can extend other class and implement interfaces but interface can only extend other interfaces.
  -  We can run an abstract class if it has main() method but we can’t run an interface because they can’t have main method implementation.
  -  Interfaces are used to define contract for the subclasses whereas abstract class also define contract but it can provide other methods implementations for subclasses to use.

- **Difference between `queue` and `topic` in message brokers:**
  - A queue can have only one consumer, whereas a topic can have multiple subscribers.

- **HashMap vs HashTable:** 
  - Since JDK 1.8, Hashtable has been deprecated 
  - Firstly, Hashtable is thread-safe and can be shared between multiple threads in the application. HashMap is not synchronized.
  - Another difference is null handling. HashMap allows adding one Entry with null as key as well as many entries with null as value. In contrast, Hashtable doesn’t allow null at all.
  - HashMap uses Iterator to iterate over values, whereas Hashtable has Enumerator for the same. 
    - `Iterator является преемником Enumerator, который устраняет его несколько недостатков.`
      The Iterator is a successor of Enumerator that eliminates its few drawbacks. For example, Iterator has a remove() method to remove elements from underlying collections. The Iterator is a fail-fast iterator. In other words, it throws a ConcurrentModificationException when the underlying collection is modified while iterating. `Итератор — это отказоустойчивый итератор. Другими словами, он генерирует исключение ConcurrentModificationException, когда базовая коллекция изменяется во время итерации.`
    	
- **Optimistic vs pessimistic locking:**
  - Optimistic locking is a technique for SQL database applications that does not hold row locks between selecting and updating or deleting a row. The application is written to optimistically assume that unlocked rows are unlikely to change before the update or delete operation.
	
- **`transient` key word:**
  - [ ] When we mark any variable as transient, then that variable is not serialized.
	
- **Circuit bracker pattern:** - 
  - One of the implementation is (`Resilience4j`).
  - Helps us in preventing a cascade of failures when a remote service is down. After a number of failed attempts, we can consider that the service is unavailable/overloaded and eagerly reject all subsequent requests to it
	
- **Singleton, prototype in Spring Boot:**
  - Pattern definition `Singleton pattern involves a single class responsible for creating an object and ensuring that only a single instance ever gets created. `
    1. Hiding all constructors by implementing a single private constructor
    2. Creating an instance only when it doesn’t exist and storing it in a private static variable
    3. Providing simple access to that instance using a public, static getter
  - A bean in the Spring framework is an object created, managed, and destroyed in the Spring IoC Container.
    - The latest version of the `Spring Framework` defines **six** types of scopes:
      1. singleton
      2. prototype
      3. request
      4. session
      5. application
      6. websocket
    
- **Disadvantages of microservice architecture:**
  - High latency
	
- **What have less coupling, Inheritance vs. Aggregation**
  - Inheritance leads to higher coupling. Composition = less coupling
    1. Problem of Inheritance - When reading through a procedure that lives in an inheritance stack you find yourself bouncing up and down levels of inheritance and getting lost. It’s called the yo-yo problem.
		
-  **Aggregation vs Composition:**
   - Aggregation differs from ordinary composition in that it does not imply ownership. In Spring Boot using Singlenton in class (variable) is Aggregation, but using Prototype - is Composition (The prototype will destroy then the used class will destroy as well). But in aggregation - singlenton not will destroy.
   
- **Difference map() vs flatMap() in streams:**
    - `map` для каждого объекта в стриме возвращает по 1 объекту, потом преобразует все объекты в итоговый стрим.
    - `flatMap` возвращает по стриму для каждого объекта в первоначальном стриме, а затем результирующие потоки объединяются в исходный стрим.
    Пример из документации Java: Если orders — стрим заказов, а каждый заказ включает в себя несколько купленных вещей (line items), то получить стрим из всех купленных вещей можно так:
    `orders.flatMap(order -> order.getLineItems().stream())`
    
- **New laptop, maven not load dependency:**
    - setting.xml should be configured in .m2 for repository
    
- **How skip tests in mvn:**
    - mvn install -DskipTests  - compile the tests but not run it
    - mvn install -Dmaven.test.skip=true - property to skip compiling the tests
    
- **How run Spring Boot project with mvn in console:**
    - mvn spring-boot:run
    - mvn clean install | java -jar target/myapplication-0.0. 1-SNAPSHOT.jar
    - gradle bootRun - using gradle
    
- **Method missing exception (for example in cjLib package) - during booling:**
    - Check class path vesrion of this library using: 
        - mvn help:effective-pom - for check version of dependency, OR in IDE see dependency graph (not shown version)
        - transitive deppendency not shown in effective-pom
        - fro this use: mvn dependency:tree - see all dependency
        - if needed new version:
            - BAD practice - add needed version of dependency
            - GOOD practice - use <dependencyManagement> tag for OVERRIDE needed version of dependency
- **SLA** - A service-level agreement (SLA) defines the level of service you expect from a vendor, laying out the metrics by which service is measured.
  - Соглашение об уровне обслуживания (SLA) определяет уровень обслуживания, которого вы ожидаете от поставщика, с изложением показателей, по которым измеряется обслуживание.
    
- **With SLA method should work 4 sec max - but the method work 40sec, how investigate:**
  - Run async profiler, invoke the method and see logs. you can siple see logs and maybe a lot of transactions to DB occures.
    
- **Problem that @Transactional not work:**
 The Interanl method marked as @Transactional and this method in `for loop` invoke external class with @Transactional method - but we have not a one transaction but many transactions:
    Problem is: for have a transaction we should get into the method (войти в метод) during Proxy of the class - but we use `this` to invoke @Transaction, and not use Proxy. The one solution is `self-injection` `autowired` itself - and use injected object - this allow use Proxy and open transaction.
    
- **Lambda how use local variables:**
  - Variable should be effectively final. Why? Life cycle object from lambda is different with life cycle where this object use.
    
- **How create Immutable class:**
  - class should be final
  - all members private and final
  - constructor - deep copy
  - getters - should return deep copy of variables
    
- **Positive using immutable:**
  - Safe of using this object - they not changed

- **Garbage collector, is the object was deleted if this object contain a reference to itself?**
  - Tracing GC if used - this GC will remove this object
  - Trace start from GC root: static, threads, class, object from ocal stack
    
- **Weak reference, strong or soft references**
    - 
- **New features in Spring Boot 2.x**
    - 
    


**Rules**
- Too much information is bad
- Never say no - Say `I not use this tech in production but know and learn about this..or I know good alternative.`
- Finish meet with positive
