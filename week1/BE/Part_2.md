# Phần 2: OOP 
## a. OOP trong Java
## b. Dependency Injection(DI)
### Khái niệm :
- Dependency Injection (DI) là kỹ thuật lập trình mà đối tượng (object) không tự tạo ra các phụ thuộc (dependency) của nó, mà các phụ thuộc đó được “tiêm vào” (injected) từ bên ngoài.
- “Thay vì tự new trong class, thì có người khác new giúp và đưa sẵn cho mình dùng.”

- Vấn đề trước khi có DI
```java
class Engine {
    void start() {
        System.out.println("Engine started!");
    }
}

class Car {
    private Engine engine = new Engine(); // <-- phụ thuộc cứng

    void start() {
        engine.start();
    }
}

```

- Giải pháp : DI
```java 
class Engine {
    void start() {
        System.out.println("Động cơ xăng khởi động!");
    }
}

class ElectricEngine extends Engine {
    @Override
    void start() {
        System.out.println("Động cơ điện khởi động!");
    }
}

class Car {
    private Engine engine;

    // Constructor Injection
    public Car(Engine engine) {
        this.engine = engine;
    }

    void start() {
        engine.start();
    }
}

public class Main {
    public static void main(String[] args) {
        Engine engine = new ElectricEngine(); // inject dependency
        Car car = new Car(engine);
        // Car không phụ thuộc cứng vào engine nữa
        car.start();
    }
}

```
## c. Inversion of Control(Ioc)
### Khái niệm : 
- Bình thường code của bạn gọi thư viện, còn trong IoC, thư viện (hoặc framework) sẽ gọi code của bạn.
- --> Quyền điều khiển bị đảo ngược — nên gọi là Inversion of Control.
- Khi chưa có IoC
```java
class Database {
    void connect() {
        System.out.println("Kết nối tới Database!");
    }
}

class App {
    public static void main(String[] args) {
        Database db = new Database();  // Tự new
        db.connect();                  // Tự gọi
        System.out.println("Ứng dụng chạy!");
    }
}
```

- Khi có IoC (Spring)
```java
@Component
class Database {
    void connect() {
        System.out.println("Database đã được container kết nối!");
    }
}

@Component
class AppRunner implements CommandLineRunner {
    private final Database db;

    @Autowired
    public AppRunner(Database db) { // Spring bơm (inject)
        this.db = db;
    }

    @Override
    public void run(String... args) {
        db.connect(); // Spring gọi run() -> bạn chỉ viết logic
        System.out.println("Ứng dụng chạy!");
    }
}

@SpringBootApplication
public class MainApp {
    public static void main(String[] args) {
        SpringApplication.run(MainApp.class, args);
    }
    // Mình tự custom logic và để Spring quản lý và gọi 
    // -> IoC
}
```