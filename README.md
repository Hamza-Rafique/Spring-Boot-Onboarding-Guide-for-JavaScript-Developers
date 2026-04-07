# 🚀 Spring Boot Onboarding Guide for JavaScript Developers
> **From your Lead Developer** — You already know how to build great software. This guide is about translating that knowledge into the Java/Spring Boot world so you can contribute to the project within 1–2 weeks. Let's go.

---

## Chapter 1: Mindset Shift — Java vs JavaScript

Before touching any code, understand these fundamental differences. They explain *why* Spring Boot works the way it does.

### 1.1 Strongly Typed & Compiled

```java
// Java — you MUST declare the type
String name = "Alice";
int age = 30;
List<String> items = new ArrayList<>();

// This will NOT compile — wrong type
String count = 42; // ❌ Compile error
```

```javascript
// JavaScript — types are flexible
let name = "Alice";
let age = 30;
let items = [];
let count = 42; // ✅ No problem
```

> **Lead Dev Tip:** Think of Java like TypeScript with strict mode turned all the way up — and enforced at *compile time*, not runtime. If it compiles, types are correct. This catches a whole class of bugs before the app even runs.

### 1.2 Object-Oriented First

Java is built around **classes and objects** — everything lives inside a class. There are no standalone functions floating around like in JS.

```java
// Java — everything is a class
public class UserService {
    public String getGreeting(String name) {
        return "Hello, " + name;
    }
}
```

```javascript
// JavaScript equivalent
function getGreeting(name) {
    return `Hello, ${name}`;
}
// OR as a class
class UserService {
    getGreeting(name) {
        return `Hello, ${name}`;
    }
}
```

### 1.3 The Key Mental Model

| Concept | JavaScript / Node.js | Java / Spring Boot |
|---|---|---|
| Runtime | Node.js | JVM (Java Virtual Machine) |
| Package Manager | npm / yarn | Maven / Gradle |
| Package file | `package.json` | `pom.xml` / `build.gradle` |
| Entry point | `index.js` | `@SpringBootApplication` main class |
| Environment config | `.env` | `application.yml` / `application.properties` |
| Framework | Express / Fastify | Spring Boot |
| ORM | Sequelize / Prisma / Mongoose | Spring Data JPA / Hibernate |
| Test framework | Jest / Mocha | JUnit 5 + Mockito |

---

## Chapter 2: Environment Setup

### 2.1 Install the JDK

Download **JDK 21** (LTS) from [Adoptium](https://adoptium.net/).

Verify installation:
```bash
java -version
# java version "21.x.x"

javac -version
# javac 21.x.x
```

> **Lead Dev Tip:** Always use an LTS version of the JDK on projects. JDK 21 is our standard. Don't install random JDKs from Oracle — use Adoptium (formerly AdoptOpenJDK).

### 2.2 Install IntelliJ IDEA

Download **IntelliJ IDEA Community** (free) or Ultimate from [JetBrains](https://www.jetbrains.com/idea/).

> IntelliJ is to Java what VS Code is to JavaScript — but with far more built-in intelligence. Use it. Don't try to use VS Code for Java on a real project.

### 2.3 Bootstrap a Project with Spring Initializr

Go to [https://start.spring.io](https://start.spring.io) and configure:

| Field | Value |
|---|---|
| Project | Maven |
| Language | Java |
| Spring Boot | 3.x (latest stable) |
| Java | 21 |
| Dependencies | Spring Web, Spring Data JPA, PostgreSQL Driver, Lombok, Spring Boot DevTools, Validation |

Click **Generate**, unzip, and open in IntelliJ.

### 2.4 Maven — Your New npm

```bash
# npm install  →  mvn install
mvn install

# npm run dev  →  mvn spring-boot:run
mvn spring-boot:run

# npm test     →  mvn test
mvn test

# npm run build → mvn package
mvn package
```

Dependencies live in `pom.xml`:
```xml
<!-- pom.xml — like package.json -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

---

## Chapter 3: Project Structure

```
my-project/
├── src/
│   ├── main/
│   │   ├── java/com/company/myproject/
│   │   │   ├── MyProjectApplication.java   ← Entry point (like index.js)
│   │   │   ├── controller/                 ← Route handlers (like Express routers)
│   │   │   ├── service/                    ← Business logic
│   │   │   ├── repository/                 ← DB queries (like Sequelize models)
│   │   │   ├── model/ (or entity/)         ← Data models / DB entities
│   │   │   └── dto/                        ← Data Transfer Objects (request/response shapes)
│   │   └── resources/
│   │       ├── application.yml             ← Config (like .env + config files)
│   │       └── application-dev.yml         ← Dev environment config
│   └── test/
│       └── java/com/company/myproject/     ← Tests (mirrors main structure)
└── pom.xml                                 ← package.json equivalent
```

### Node/Express Equivalent Comparison

```
Express Project          Spring Boot Project
─────────────────        ──────────────────────
index.js             →   MyProjectApplication.java
routes/users.js      →   controller/UserController.java
services/userSvc.js  →   service/UserService.java
models/User.js       →   entity/User.java
.env                 →   application.yml
```

---

## Chapter 4: Core Concepts — Annotations & Dependency Injection

This is the most important chapter. Spring Boot is **annotation-driven**. Annotations are like decorators in Angular — they tell the framework what each class is and how to wire things together.

### 4.1 The Key Annotations

```java
@SpringBootApplication   // Marks the entry point — boots the whole app
@RestController          // This class handles HTTP requests (like an Express Router)
@Service                 // Business logic layer
@Repository              // Database access layer
@Component               // Generic Spring-managed bean
@Autowired               // Inject a dependency (like Angular's constructor injection)
@GetMapping("/path")     // Handle GET request
@PostMapping("/path")    // Handle POST request
@PutMapping("/path")     // Handle PUT request
@DeleteMapping("/path")  // Handle DELETE request
@RequestBody             // Parse JSON body (like req.body in Express)
@PathVariable            // URL path param (like req.params.id)
@RequestParam            // Query param (like req.query.page)
```

### 4.2 Dependency Injection — The Angular Way You Already Know

If you've used Angular, you already understand DI. Spring works the same way.

```typescript
// Angular
@Injectable()
export class UserService {
  getUsers() { ... }
}

@Component({ ... })
export class UserComponent {
  constructor(private userService: UserService) {} // Angular injects this
}
```

```java
// Spring Boot — identical concept
@Service
public class UserService {
    public List<User> getUsers() { ... }
}

@RestController
public class UserController {
    private final UserService userService;

    // Spring injects UserService automatically
    public UserController(UserService userService) {
        this.userService = userService;
    }
}
```

> **Lead Dev Tip:** Always use **constructor injection** (like above), not `@Autowired` on the field. It's cleaner, testable, and the Spring team recommends it. You'll see `@Autowired` in old code — know what it is, but don't write it that way.

---

## Chapter 5: Building a REST API

Let's build a complete Users API — something you've definitely done in Express.

### 5.1 The Entity (Database Model)

```java
// entity/User.java — like a Sequelize/Mongoose model
@Entity
@Table(name = "users")
@Data                    // Lombok: auto-generates getters, setters, toString
@NoArgsConstructor       // Lombok: generates no-arg constructor
@AllArgsConstructor      // Lombok: generates all-arg constructor
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY) // AUTO_INCREMENT
    private Long id;

    @Column(nullable = false)
    private String name;

    @Column(unique = true, nullable = false)
    private String email;
}
```

> **Lead Dev Tip:** **Lombok** is a lifesaver. `@Data` alone replaces ~50 lines of boilerplate getters/setters. Always use it. Install the Lombok plugin in IntelliJ.

### 5.2 The Repository (Database Access)

```java
// repository/UserRepository.java
// This interface gives you CRUD for FREE — no SQL needed
@Repository
public interface UserRepository extends JpaRepository<User, Long> {

    // Spring auto-generates SQL from method names!
    Optional<User> findByEmail(String email);
    List<User> findByNameContaining(String name);
    boolean existsByEmail(String email);
}
```

```javascript
// Node.js/Sequelize equivalent
const User = require('../models/User');
const user = await User.findOne({ where: { email } });
```

### 5.3 The Service (Business Logic)

```java
// service/UserService.java
@Service
@RequiredArgsConstructor  // Lombok: constructor injection for all final fields
public class UserService {

    private final UserRepository userRepository;

    public List<User> getAllUsers() {
        return userRepository.findAll();
    }

    public User getUserById(Long id) {
        return userRepository.findById(id)
            .orElseThrow(() -> new RuntimeException("User not found with id: " + id));
    }

    public User createUser(User user) {
        if (userRepository.existsByEmail(user.getEmail())) {
            throw new RuntimeException("Email already in use");
        }
        return userRepository.save(user);
    }

    public void deleteUser(Long id) {
        userRepository.deleteById(id);
    }
}
```

### 5.4 The Controller (Route Handler)

```java
// controller/UserController.java
@RestController
@RequestMapping("/api/users")   // Base path — like Express Router prefix
@RequiredArgsConstructor
public class UserController {

    private final UserService userService;

    // GET /api/users
    @GetMapping
    public ResponseEntity<List<User>> getAllUsers() {
        return ResponseEntity.ok(userService.getAllUsers());
    }

    // GET /api/users/42
    @GetMapping("/{id}")
    public ResponseEntity<User> getUserById(@PathVariable Long id) {
        return ResponseEntity.ok(userService.getUserById(id));
    }

    // POST /api/users
    @PostMapping
    public ResponseEntity<User> createUser(@Valid @RequestBody User user) {
        User created = userService.createUser(user);
        return ResponseEntity.status(HttpStatus.CREATED).body(created);
    }

    // DELETE /api/users/42
    @DeleteMapping("/{id}")
    public ResponseEntity<Void> deleteUser(@PathVariable Long id) {
        userService.deleteUser(id);
        return ResponseEntity.noContent().build();
    }
}
```

```javascript
// Express.js equivalent for comparison
router.get('/', async (req, res) => {
    const users = await userService.getAllUsers();
    res.json(users);
});

router.post('/', async (req, res) => {
    const user = await userService.createUser(req.body);
    res.status(201).json(user);
});
```

---

## Chapter 6: Configuration & Environments

### 6.1 application.yml — Your .env on Steroids

```yaml
# src/main/resources/application.yml
spring:
  datasource:
    url: jdbc:postgresql://localhost:5432/mydb
    username: ${DB_USERNAME}          # reads from environment variable
    password: ${DB_PASSWORD}
  jpa:
    hibernate:
      ddl-auto: update               # auto-creates/updates DB tables
    show-sql: true                   # logs SQL queries (like Sequelize logging)
  profiles:
    active: dev                      # which profile is active

server:
  port: 8080
```

```javascript
// .env equivalent
DB_HOST=localhost
DB_PORT=5432
DB_NAME=mydb
DB_USERNAME=postgres
DB_PASSWORD=secret
PORT=8080
```

### 6.2 Multiple Environments

```yaml
# application-dev.yml
spring:
  jpa:
    show-sql: true
logging:
  level:
    root: DEBUG
```

```yaml
# application-prod.yml
spring:
  jpa:
    show-sql: false
logging:
  level:
    root: WARN
```

Activate via environment variable:
```bash
# Like NODE_ENV=production
java -jar app.jar --spring.profiles.active=prod
```

---

## Chapter 7: Error Handling & Validation

### 7.1 Global Exception Handler

```java
// exception/GlobalExceptionHandler.java
@RestControllerAdvice   // Like Express error middleware — catches all errors
public class GlobalExceptionHandler {

    @ExceptionHandler(RuntimeException.class)
    public ResponseEntity<Map<String, String>> handleRuntimeException(RuntimeException ex) {
        Map<String, String> error = Map.of(
            "message", ex.getMessage(),
            "status", "error"
        );
        return ResponseEntity.status(HttpStatus.BAD_REQUEST).body(error);
    }

    @ExceptionHandler(EntityNotFoundException.class)
    public ResponseEntity<Map<String, String>> handleNotFound(EntityNotFoundException ex) {
        return ResponseEntity.status(HttpStatus.NOT_FOUND)
            .body(Map.of("message", ex.getMessage()));
    }
}
```

```javascript
// Express equivalent
app.use((err, req, res, next) => {
    res.status(400).json({ message: err.message });
});
```

### 7.2 Request Validation

```java
// entity/User.java — add validation annotations
public class User {
    @NotBlank(message = "Name is required")
    private String name;

    @Email(message = "Invalid email format")
    @NotBlank(message = "Email is required")
    private String email;

    @Min(value = 0, message = "Age must be positive")
    private int age;
}

// Controller — use @Valid to trigger validation
@PostMapping
public ResponseEntity<User> createUser(@Valid @RequestBody User user) {
    // If validation fails, Spring throws MethodArgumentNotValidException automatically
}
```

```javascript
// Node.js/Joi equivalent
const schema = Joi.object({
    name: Joi.string().required(),
    email: Joi.string().email().required(),
    age: Joi.number().min(0)
});
```

---

## Chapter 8: Testing

### 8.1 Unit Testing — JUnit + Mockito (like Jest + jest.mock)

```java
// test/service/UserServiceTest.java
@ExtendWith(MockitoExtension.class)   // Like jest test runner
class UserServiceTest {

    @Mock
    private UserRepository userRepository;   // Like jest.mock()

    @InjectMocks
    private UserService userService;         // Class under test with mocks injected

    @Test
    void shouldReturnUserById() {
        // Arrange — like jest beforeEach setup
        User mockUser = new User(1L, "Alice", "alice@example.com");
        when(userRepository.findById(1L)).thenReturn(Optional.of(mockUser));

        // Act
        User result = userService.getUserById(1L);

        // Assert — like expect()
        assertNotNull(result);
        assertEquals("Alice", result.getName());
        verify(userRepository, times(1)).findById(1L);
    }

    @Test
    void shouldThrowWhenUserNotFound() {
        when(userRepository.findById(99L)).thenReturn(Optional.empty());

        assertThrows(RuntimeException.class, () -> userService.getUserById(99L));
    }
}
```

```javascript
// Jest equivalent
jest.mock('../repositories/userRepository');

describe('UserService', () => {
    it('should return user by id', async () => {
        userRepository.findById.mockResolvedValue({ id: 1, name: 'Alice' });
        const result = await userService.getUserById(1);
        expect(result.name).toBe('Alice');
    });
});
```

### 8.2 Integration Testing

```java
@SpringBootTest
@AutoConfigureMockMvc
class UserControllerTest {

    @Autowired
    private MockMvc mockMvc;  // Like supertest in Node.js

    @Test
    void shouldReturnAllUsers() throws Exception {
        mockMvc.perform(get("/api/users"))
            .andExpect(status().isOk())
            .andExpect(jsonPath("$").isArray());
    }
}
```

---

## Chapter 9: Your 2-Week Onboarding Battle Plan

### ✅ Week 1 — Foundation & Read the Codebase

| Day | Goals |
|---|---|
| **Day 1** | Setup JDK, IntelliJ, Maven. Run the project locally. Hit an endpoint with Postman. |
| **Day 2** | Read the project structure. Map Controllers → Services → Repositories. Draw a diagram. |
| **Day 3** | Understand the entity models and DB schema. Run the existing tests. |
| **Day 4** | Add a simple new field to an existing entity + migration. Submit a PR. |
| **Day 5** | Write 2–3 unit tests for an existing Service class. Submit a PR. |

### ✅ Week 2 — Build & Contribute

| Day | Goals |
|---|---|
| **Day 6** | Pick up a small ticket — add a new endpoint to an existing Controller. |
| **Day 7** | Add validation and error handling to your new endpoint. |
| **Day 8** | Write unit tests + integration test for your endpoint. |
| **Day 9** | Code review cycle — address feedback, understand team conventions. |
| **Day 10** | Pick up a medium ticket independently. You're now contributing. 🎉 |

---

## Chapter 10: Lead Dev's Top Tips for JS Developers

> These are the things I wish someone told me when I switched stacks.

1. **Read compile errors carefully.** Java errors are verbose but precise — they tell you exactly what's wrong and on which line. Don't panic, read the full message.

2. **Lombok is not optional.** Use `@Data`, `@Builder`, `@RequiredArgsConstructor`. Without it, Java is unbearably verbose.

3. **Don't fight the framework.** Spring Boot has strong conventions. Learn them instead of working around them. The structure exists for a reason.

4. **`Optional<T>` replaces null checks.** Use `.orElseThrow()` and `.orElse()` instead of null checks. Think of it like JS optional chaining but for data access.

5. **JPA is powerful but dangerous.** Understand N+1 query problems. Use `@EntityGraph` or `JOIN FETCH` for related entities. Always check the SQL it generates with `show-sql: true`.

6. **IntelliJ is your best friend.** Use `Ctrl+Click` to navigate, `Alt+Enter` for quick fixes, `Shift+Shift` to search everything. Learn the shortcuts — they're worth it.

7. **Types are documentation.** In Java, you never wonder what shape a function returns — the type signature tells you. Embrace this.

8. **`System.out.println` → use a Logger.** Use `@Slf4j` (Lombok) and `log.info(...)` / `log.error(...)`. Never use sysout in production code.

```java
@Slf4j  // Lombok — adds a 'log' variable automatically
@Service
public class UserService {
    public User getUserById(Long id) {
        log.info("Fetching user with id: {}", id);  // Like console.log but proper
        ...
    }
}
```

---

## Quick Reference Cheat Sheet

```
JavaScript/Node.js          →   Java/Spring Boot
──────────────────────────────────────────────────
npm install                 →   mvn install
npm run dev                 →   mvn spring-boot:run
.env                        →   application.yml
console.log()               →   log.info() (@Slf4j)
async/await                 →   synchronous by default
req.body                    →   @RequestBody
req.params.id               →   @PathVariable Long id
req.query.page              →   @RequestParam int page
res.json(data)              →   ResponseEntity.ok(data)
res.status(201).json(data)  →   ResponseEntity.status(CREATED).body(data)
try/catch middleware         →   @RestControllerAdvice
jest.mock()                 →   @Mock (Mockito)
expect().toBe()             →   assertEquals() (JUnit)
supertest                   →   MockMvc
Sequelize/Mongoose          →   Spring Data JPA
Joi/Zod validation          →   @Valid + Bean Validation annotations
```

---

*Guide written by your Lead Developer. Welcome to the team — you've got this. 💪*
