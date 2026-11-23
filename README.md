# 🌐 ASP.NET Core REST API: Top 50 Interview Q&A

This guide covers 50 essential questions and answers regarding **ASP.NET Core Web API**, ranging from basics to advanced topics like performance tuning, security, and architecture.

## 📄 Table of Contents

| # | Question |
| :-: | :--- |
| 1 | [What is the **Middleware Pipeline** in ASP.NET Core?](#q-1) |
| 2 | [Explain the three **Service Lifetimes** in Dependency Injection (DI).](#q-2) |
| 3 | [**Scenario:** You inject a Scoped service into a Singleton service. What happens? (Captive Dependency)](#q-3) |
| 4 | [What is the difference between **`IActionResult`** and **`ActionResult<T>`**?](#q-4) |
| 5 | [How does **Async/Await** improve the scalability of a Web API?](#q-5) |
| 6 | [What is the role of the **`[ApiController]`** attribute?](#q-6) |
| 7 | [**Scenario:** How do you handle **Global Exception** logging in a standardized way?](#q-7) |
| 8 | [Explain **Model Binding** and the sources it reads from.](#q-8) |
| 9 | [What is the difference between **Authentication** and **Authorization**?](#q-9) |
| 10 | [**Scenario:** How do you implement **JWT (JSON Web Token)** authentication?](#q-10) |
| 11 | [What are **Filters** in ASP.NET Core and what is their execution order?](#q-11) |
| 12 | [**Scenario:** Your frontend (React) fails to call your API due to **CORS**. How do you fix it?](#q-12) |
| 13 | [What is **Content Negotiation**?](#q-13) |
| 14 | [Difference between **PUT** and **PATCH** verbs.](#q-14) |
| 15 | [**Scenario:** How do you handle **API Versioning** without breaking existing clients?](#q-15) |
| 16 | [What are **DTOs** (Data Transfer Objects) and why use them over Entity Models?](#q-16) |
| 17 | [How do you prevent **Over-Posting** (Mass Assignment) attacks?](#q-17) |
| 18 | [**Scenario:** The database is slow. How do you implement **Caching**?](#q-18) |
| 19 | [What is the difference between **Minimal APIs** and **Controller-based APIs**?](#q-19) |
| 20 | [How do you validate user input? (Data Annotations vs FluentValidation)](#q-20) |
| 21 | [**Scenario:** You need to run a background task (e.g., send email) every hour. How do you do it?](#q-21) |
| 22 | [What is **Rate Limiting** and how is it implemented in .NET 7+?](#q-22) |
| 23 | [How do you manage configuration (appsettings.json) across environments?](#q-23) |
| 24 | [**Scenario:** Your API is returning a 500 error, but you only see generic messages. How do you debug Development errors?](#q-24) |
| 25 | [What is **Entity Framework Core** and what is the **N+1 Problem**?](#q-25) |
| 26 | [What are **Health Checks**?](#q-26) |
| 27 | [How do you document your API automatically? (Swagger/OpenAPI)](#q-27) |
| 28 | [**Scenario:** You need to restrict access to an endpoint to only "Admin" users.](#q-28) |
| 29 | [What is the **`Program.cs`** file (in .NET 6+)?](#q-29) |
| 30 | [What is **Idempotency** in REST APIs?](#q-30) |
| 31 | [What is **Kestrel** and why do we often use a **Reverse Proxy** (IIS/Nginx) in front of it?](#q-31) |
| 32 | [What is the **Options Pattern** (`IOptions`, `IOptionsSnapshot`, `IOptionsMonitor`)?](#q-32) |
| 33 | [**Scenario:** How do you perform **Integration Testing** for your API endpoints?](#q-33) |
| 34 | [What is **SignalR** and when would you use it in a Web API?](#q-34) |
| 35 | [How does **Response Compression** work in ASP.NET Core?](#q-35) |
| 36 | [**Scenario:** You need to secure sensitive dev secrets (like API keys) locally.](#q-36) |
| 37 | [What is **Structured Logging** (e.g., Serilog)?](#q-37) |
| 38 | [Should you implement the **Repository Pattern** with EF Core?](#q-38) |
| 39 | [Difference between **Custom Middleware** and **Filters**?](#q-39) |
| 40 | [**Scenario:** A client is uploading a 1GB file. How do you handle it efficiently?](#q-40) |
| 41 | [What is `AsNoTracking()` in EF Core and when should you use it?](#q-41) |
| 42 | [What is **Output Caching** (introduced in .NET 7)?](#q-42) |
| 43 | [How do you **Dockerize** an ASP.NET Core application?](#q-43) |
| 44 | [What are **HSTS** and **HTTPS Redirection**?](#q-44) |
| 45 | [What is **OData** and does ASP.NET Core support it?](#q-45) |
| 46 | [**Scenario:** Compare `System.Text.Json` vs `Newtonsoft.Json`.](#q-46) |
| 47 | [How do you track a request across microservices? (**Correlation IDs**)](#q-47) |
| 48 | [What are **Source Generators** in .NET?](#q-48) |
| 49 | [**Scenario:** How do you return standard error formats? (**ProblemDetails**)](#q-49) |
| 50 | [What is **GraphQL** and how does it compare to REST in .NET?](#q-50) |

---

## 💡 Questions and Explanations

### <a id="q-1"></a>1. What is the **Middleware Pipeline** in ASP.NET Core?

**Answer:** The middleware pipeline is a series of components (delegates) that handle requests and responses.

**Brief Explanation:** When a request comes in, it passes through the middleware one by one. Each middleware can:
1.  Process the request and pass it to the next middleware (`next()`).
2.  Short-circuit the pipeline (return a response immediately).
3.  Process the response on its way back out.
* **Order matters:** Authentication must run before Authorization.

[Go to Top](#)

---

### <a id="q-2"></a>2. Explain the three **Service Lifetimes** in Dependency Injection (DI).

**Answer:**
1.  **Transient (`AddTransient`):** A new instance is created **every time** it is requested. Lightweight services.
2.  **Scoped (`AddScoped`):** A new instance is created once per **Client Request** (HTTP request). Used for DB Contexts.
3.  **Singleton (`AddSingleton`):** A single instance is created the **first time** it is requested and used everywhere for the application's lifetime. Used for Caching services.

[Go to Top](#)

---

### <a id="q-3"></a>3. **Scenario:** You inject a Scoped service into a Singleton service. What happens? (Captive Dependency)

**Answer:** This causes a **Captive Dependency** (Memory Leak/Bug).

**Brief Explanation:** The Singleton service is created only once. Therefore, it holds onto the specific instance of the Scoped service that was created at that time. That "Scoped" service effectively becomes a Singleton and is never disposed of (until the app shuts down), potentially causing DB connection issues or stale data.

[Go to Top](#)

---

### <a id="q-4"></a>4. What is the difference between **`IActionResult`** and **`ActionResult<T>`**?

**Answer:**
* **`IActionResult`:** Returns strictly an HTTP response (e.g., `Ok()`, `NotFound()`). It does not define the schema of the data.
* **`ActionResult<T>`:** Returns either an HTTP response OR the specific type `T`.

**Brief Explanation:** `ActionResult<T>` is better for Swagger/OpenAPI documentation because it explicitly tells the documentation generator what data type (schema) the API returns.

[Go to Top](#)

---

### <a id="q-5"></a>5. How does **Async/Await** improve the scalability of a Web API?

**Answer:** It allows the server to handle more concurrent requests by not blocking threads.

**Brief Explanation:** When an API hits an I/O operation (Database, File, External API) using `await`, the thread is released back to the Thread Pool to handle *other incoming requests*. Once the I/O finishes, a thread picks up where it left off. It does **not** make the individual request faster; it makes the **server** capable of handling high throughput.

[Go to Top](#)

---

### <a id="q-6"></a>6. What is the role of the **`[ApiController]`** attribute?

**Answer:** It is a decorator applied to Controller classes that enables opinionated API behaviors.

**Brief Explanation:** It automatically handles:
1.  **Attribute Routing requirement:** Enforces usage of `[Route]`.
2.  **Automatic HTTP 400 responses:** Automatically checks `ModelState.IsValid` and returns 400 if validation fails.
3.  **Binding source inference:** Automatically assumes `[FromBody]` for complex types.

[Go to Top](#)

---

### <a id="q-7"></a>7. **Scenario:** How do you handle **Global Exception** logging in a standardized way?

**Answer:** Use **Middleware**.

**Brief Explanation:**
1.  **Development:** Use `app.UseDeveloperExceptionPage()`.
2.  **Production:** Create a custom `ExceptionMiddleware` (or use `app.UseExceptionHandler`). Wrap the `next()` call in a `try-catch` block. Inside `catch`, log the error and write a standardized JSON response (e.g., `ProblemDetails`) to the HTTP stream with a 500 status code.

[Go to Top](#)

---

### <a id="q-8"></a>8. Explain **Model Binding** and the sources it reads from.

**Answer:** Model Binding is the process of mapping data from the HTTP request to the Action Method parameters.

**Brief Explanation:** Common attributes include:
* `[FromBody]`: Request Body (JSON/XML).
* `[FromQuery]`: Query String parameters (`?id=1`).
* `[FromRoute]`: Route data (`/api/users/{id}`).
* `[FromHeader]`: HTTP Headers.
* `[FromForm]`: Form data (File uploads).

[Go to Top](#)

---

### <a id="q-9"></a>9. What is the difference between **Authentication** and **Authorization**?

**Answer:**
* **Authentication (AuthN):** Verifying **who** the user is (e.g., Login, validating a Token). "Here are my credentials."
* **Authorization (AuthZ):** Verifying **what** the user can do. "Do you have permission to access this resource?" (Roles, Policies).

[Go to Top](#)

---

### <a id="q-10"></a>10. **Scenario:** How do you implement **JWT (JSON Web Token)** authentication?

**Answer:**
1.  **Login:** Verify credentials. If valid, generate a signed JWT (containing Claims, Expiry, Secret Key) and return it to the client.
2.  **Configuration:** In `Program.cs`, add `AddAuthentication().AddJwtBearer(...)`. Configure the Token Validation Parameters (Issuer, Audience, Signing Key).
3.  **Usage:** Client sends the token in the `Authorization: Bearer <token>` header. The middleware validates the signature before reaching the controller.

[Go to Top](#)

---

### <a id="q-11"></a>11. What are **Filters** in ASP.NET Core and what is their execution order?

**Answer:** Filters allow code to run before or after specific stages in the request processing pipeline.

**Brief Explanation:** The order is:
1.  **Authorization Filters:** Run first.
2.  **Resource Filters:** Run after Auth, before Model Binding.
3.  **Action Filters:** Run before/after the Action Method.
4.  **Exception Filters:** Run if an exception occurs.
5.  **Result Filters:** Run before/after the Result execution.

[Go to Top](#)

---

### <a id="q-12"></a>12. **Scenario:** Your frontend (React) fails to call your API due to **CORS**. How do you fix it?

**Answer:** Configure **Cross-Origin Resource Sharing (CORS)** in the backend.

**Brief Explanation:** Browsers block requests from different origins (domains/ports) by default.
In `Program.cs`:
1.  `builder.Services.AddCors(...)` to define a policy (allowing specific origins, methods, and headers).
2.  `app.UseCors("MyPolicy")` **must** be placed between `UseRouting` and `UseAuthorization`.

[Go to Top](#)

---

### <a id="q-13"></a>13. What is **Content Negotiation**?

**Answer:** The process of selecting the best representation for a given response when there are multiple representations available.

**Brief Explanation:** The client sends an `Accept` header (e.g., `application/xml`). The API checks if it has a formatter for XML. If yes, it returns XML; otherwise, it might fallback to JSON or return 406 (Not Acceptable).

[Go to Top](#)

---

### <a id="q-14"></a>14. Difference between **PUT** and **PATCH** verbs.

**Answer:**
* **PUT:** Full Update. It replaces the **entire** resource. If you send a PUT request with only one field, other fields might be set to null/default.
* **PATCH:** Partial Update. It modifies **only** the fields provided in the request body, leaving others unchanged.

[Go to Top](#)

---

### <a id="q-15"></a>15. **Scenario:** How do you handle **API Versioning** without breaking existing clients?

**Answer:** Use the `Microsoft.AspNetCore.Mvc.Versioning` package.

**Brief Explanation:** Strategies include:
1.  **URL Path:** `/api/v1/products` (Most common).
2.  **Query String:** `/api/products?api-version=1.0`.
3.  **Header:** `X-Version: 1.0`.
* You decorate controllers with `[ApiVersion("1.0")]` and `[MapToApiVersion("1.0")]`.

[Go to Top](#)

---

### <a id="q-16"></a>16. What are **DTOs** (Data Transfer Objects) and why use them over Entity Models?

**Answer:** DTOs are objects used to transfer data between processes.

**Brief Explanation:** You should not return Database Entities (EF Core models) directly because:
1.  **Security:** Prevent exposing sensitive columns (PasswordHash).
2.  **Coupling:** Decouple the internal DB schema from the external API contract.
3.  **Circular References:** Prevent serialization errors with navigation properties.

[Go to Top](#)

---

### <a id="q-17"></a>17. How do you prevent **Over-Posting** (Mass Assignment) attacks?

**Answer:** By using **DTOs** (Data Transfer Objects).

**Brief Explanation:** If you bind a request directly to a User Entity, a hacker could include `IsAdmin: true` in the JSON payload, potentially overwriting that field in the database. By using a `UserUpdateDto` that *only* contains the fields allowed to be updated (e.g., `FirstName`, `Email`), you ignore any malicious extra properties.

[Go to Top](#)

---

### <a id="q-18"></a>18. **Scenario:** The database is slow. How do you implement **Caching**?

**Answer:**
1.  **In-Memory Caching (`IMemoryCache`):** Store data in the server's RAM. Good for single-instance apps.
2.  **Distributed Caching (`IDistributedCache`):** Use Redis or SQL Server. Essential for microservices or load-balanced apps (multiple instances) so they share the same cache state.
3.  **Response Caching:** Use the `[ResponseCache]` attribute to tell clients/proxies how long to cache the HTTP response.

[Go to Top](#)

---

### <a id="q-19"></a>19. What is the difference between **Minimal APIs** and **Controller-based APIs**?

**Answer:**
* **Controller-based:** The traditional MVC approach with classes deriving from `ControllerBase`. Rich features, built-in filters, structured organization.
* **Minimal APIs:** Introduced in .NET 6. Define endpoints directly in `Program.cs` using `app.MapGet()`. Much less boilerplate, higher performance, ideal for microservices with few endpoints.

[Go to Top](#)

---

### <a id="q-20"></a>20. How do you validate user input? (Data Annotations vs FluentValidation)

**Answer:**
* **Data Annotations:** Attributes on the DTO properties (e.g., `[Required]`, `[MaxLength(50)]`). Simple and built-in.
* **FluentValidation:** A third-party library that separates validation logic from the model class using Rule sets. Better for complex scenarios, conditional validation, and keeping models clean.

[Go to Top](#)

---

### <a id="q-21"></a>21. **Scenario:** You need to run a background task (e.g., send email) every hour. How do you do it?

**Answer:** Use **Hosted Services**.

**Brief Explanation:** Create a class that implements `IHostedService` or inherits from `BackgroundService`. Register it in DI using `builder.Services.AddHostedService<MyWorker>()`. For complex scheduling (Cron jobs), libraries like **Hangfire** or **Quartz.NET** are preferred.

[Go to Top](#)

---

### <a id="q-22"></a>22. What is **Rate Limiting** and how is it implemented in .NET 7+?

**Answer:** Rate Limiting controls the number of requests a client can make within a time period to prevent abuse (DDoS).

**Brief Explanation:** .NET 7 introduced built-in Rate Limiting middleware.
1.  Register: `builder.Services.AddRateLimiter(...)`.
2.  Define Policies: Fixed Window, Sliding Window, Token Bucket, or Concurrency.
3.  Use: `app.UseRateLimiter()` and apply `[EnableRateLimiting]` on controllers.

[Go to Top](#)

---

### <a id="q-23"></a>23. How do you manage configuration (appsettings.json) across environments?

**Answer:** ASP.NET Core uses **Environment Variables**.

**Brief Explanation:** It loads configuration in layers:
1.  `appsettings.json` (Global).
2.  `appsettings.{Environment}.json` (e.g., `appsettings.Development.json`).
3.  Environment Variables (Docker/Cloud overrides).
4.  Secrets Manager (Dev).
5.  Key Vault (Prod).
* The specific file loaded depends on the `ASPNETCORE_ENVIRONMENT` variable.

[Go to Top](#)

---

### <a id="q-24"></a>24. **Scenario:** Your API is returning a 500 error, but you only see generic messages. How do you debug Development errors?

**Answer:** Ensure the **Developer Exception Page** is enabled.

**Brief Explanation:** In `Program.cs`, check:
```csharp
if (app.Environment.IsDevelopment())
{
    app.UseSwagger();
    app.UseDeveloperExceptionPage(); // Shows stack trace in browser
}
```

[Go to Top](#)

### <a id="q-25"></a>25. What is **Entity Framework Core** and what is the **N+1 Problem**?

**Answer:** EF Core is the ORM (Object-Relational Mapper). The **N+1 Problem** is a performance issue where code executes 1 query to fetch a list of parent items, and then N additional queries to fetch child items for *each* parent.

**Solution:** Use Eager Loading via `.Include()` to fetch related data in a single query.

[Go to Top](#)

---

### <a id="q-26"></a>26. What are **Health Checks**?

**Answer:** Endpoints that allow monitoring systems (like Kubernetes or Azure Load Balancers) to verify if the application is running and its dependencies (DB, Cache) are responsive.

**Brief Explanation:**
1.  Register: `builder.Services.AddHealthChecks().AddSqlServer(...)`.
2.  Map: `app.MapHealthChecks("/health")`.
* If the DB is down, the endpoint returns `503 Unhealthy`, prompting the load balancer to stop sending traffic.

[Go to Top](#)

---

### <a id="q-27"></a>27. How do you document your API automatically? (Swagger/OpenAPI)

**Answer:** Use **Swashbuckle**.

**Brief Explanation:**
1.  Install `Swashbuckle.AspNetCore`.
2.  `builder.Services.AddSwaggerGen()` to generate the OpenAPI JSON document by reflecting on your Controllers and Models.
3.  `app.UseSwaggerUI()` to provide the interactive HTML page to test endpoints.

[Go to Top](#)

---

### <a id="q-28"></a>28. **Scenario:** You need to restrict access to an endpoint to only "Admin" users.

**Answer:** Use **Role-based** or **Policy-based** Authorization.

**Brief Explanation:**
1.  Ensure the Claims Principal has a Role claim of "Admin".
2.  Decorate the controller/action:
    ```csharp
    [Authorize(Roles = "Admin")]
    public IActionResult DeleteDatabase() { ... }
    ```

[Go to Top](#)

---

### <a id="q-29"></a>29. What is the **`Program.cs`** file (in .NET 6+)?

**Answer:** It is the entry point of the application.

**Brief Explanation:** In modern .NET (Top-level statements), it unifies the `Main` method and the old `Startup.cs`.
1.  **Builder:** Configure DI container (`builder.Services.Add...`).
2.  **Build:** `var app = builder.Build();`.
3.  **Pipeline:** Configure Middleware (`app.Use...`).
4.  **Run:** `app.Run();`.

[Go to Top](#)

---

### <a id="q-30"></a>30. What is **Idempotency** in REST APIs?

**Answer:** An operation is idempotent if making the same request multiple times produces the same result as making it once.

**Brief Explanation:**
* `GET`, `PUT`, `DELETE` should be idempotent.
* `POST` is **not** idempotent (calling it 3 times creates 3 resources).
* Implementing idempotency keys (headers) helps prevent duplicate processing in distributed networks (e.g., retrying a payment).

[Go to Top](#)

---

### <a id="q-31"></a>31. What is **Kestrel** and why do we often use a **Reverse Proxy** (IIS/Nginx) in front of it?

**Answer:** **Kestrel** is the default, cross-platform web server for ASP.NET Core.

**Brief Explanation:** While Kestrel is fast, it was historically not as hardened against internet threats as IIS or Nginx.
* **Reverse Proxy (Nginx/IIS):** Used for SSL termination, Request filtering, Static asset serving, and Port sharing (hosting multiple apps on port 80/443).

[Go to Top](#)

---

### <a id="q-32"></a>32. What is the **Options Pattern** (`IOptions`, `IOptionsSnapshot`, `IOptionsMonitor`)?

**Answer:** A pattern to access strongly-typed configuration settings from `appsettings.json`.

**Brief Explanation:**
* **`IOptions<T>`:** Singleton. Reads config once at startup. Cannot read changes after start.
* **`IOptionsSnapshot<T>`:** Scoped. Reads config on every request. Useful for reloading config without restarting.
* **`IOptionsMonitor<T>`:** Singleton. Provides an `OnChange` listener to detect config updates in real-time.

[Go to Top](#)

---

### <a id="q-33"></a>33. **Scenario:** How do you perform **Integration Testing** for your API endpoints?

**Answer:** Use **`Microsoft.AspNetCore.Mvc.Testing`** and `WebApplicationFactory`.

**Brief Explanation:** This library spins up an in-memory Test Server (bootstrapping your actual `Program.cs`). You can use `HttpClient` to call your endpoints (`GET /api/products`) and assert responses without hosting the app on a real port.

[Go to Top](#)

---

### <a id="q-34"></a>34. What is **SignalR** and when would you use it in a Web API?

**Answer:** **SignalR** is a library for adding real-time web functionality.

**Brief Explanation:** Standard Web API is request-response. Use SignalR when you need **Server-to-Client** push notifications (e.g., Chat applications, Live Dashboards, Gaming) via WebSockets.

[Go to Top](#)

---

### <a id="q-35"></a>35. How does **Response Compression** work in ASP.NET Core?

**Answer:** It reduces the size of the HTTP response (usually JSON) before sending it to the client.

**Brief Explanation:** Middleware (Brotli or Gzip providers) compresses the response body. This reduces bandwidth usage and speeds up client downloads, though it uses slightly more CPU on the server.
* Enable via: `builder.Services.AddResponseCompression()`.

[Go to Top](#)

---

### <a id="q-36"></a>36. **Scenario:** You need to secure sensitive dev secrets (like API keys) locally.

**Answer:** Use the **Secret Manager** (User Secrets).

**Brief Explanation:** Do not store secrets in `appsettings.json` (which is committed to Git). The Secret Manager stores a `secrets.json` file in your user profile directory (`%APPDATA%`), separate from the project folder.

[Go to Top](#)

---

### <a id="q-37"></a>37. What is **Structured Logging** (e.g., Serilog)?

**Answer:** Logging data as structured objects (JSON) rather than plain text strings.

**Brief Explanation:** Instead of `Log("User " + id + " failed")`, you do `Log("User {UserId} failed", id)`.
* **Benefit:** Log aggregation tools (ELK Stack, Splunk, Seq) can index and query the specific field `UserId`, making debugging vastly easier.

[Go to Top](#)

---

### <a id="q-38"></a>38. Should you implement the **Repository Pattern** with EF Core?

**Answer:** It depends (Controversial topic).

**Brief Explanation:**
* **Yes:** If you need to mock data access for Unit Testing or might swap EF Core for another ORM later.
* **No:** EF Core's `DbContext` is *already* an implementation of the Repository/Unit of Work pattern. Adding another layer often leads to unnecessary abstraction without value.

[Go to Top](#)

---

### <a id="q-39"></a>39. Difference between **Custom Middleware** and **Filters**?

**Answer:**
* **Middleware:** Acts on the entire HTTP Request/Response pipeline. Good for cross-cutting concerns agnostic of MVC (e.g., Logging, Error Handling, CORS).
* **Filters:** Acts specifically within the MVC pipeline (access to `ModelState`, `ActionArguments`, `Controller` context). Good for validation, caching, or controller-specific logic.

[Go to Top](#)

---

### <a id="q-40"></a>40. **Scenario:** A client is uploading a 1GB file. How do you handle it efficiently?

**Answer:** Do **not** buffer the entire file in memory. Use **Streaming**.

**Brief Explanation:**
* Disable the default request size limit (`[DisableRequestSizeLimit]`).
* Read the `Request.Body` stream directly using `MultipartReader` instead of binding to `IFormFile` (which buffers).
* Stream the bytes directly to a file on disk or Azure Blob Storage.

[Go to Top](#)

---

### <a id="q-41"></a>41. What is `AsNoTracking()` in EF Core and when should you use it?

**Answer:** A method that tells EF Core **not** to track changes for the retrieved entities.

**Brief Explanation:**
* **Use it for:** Read-only queries (e.g., `GET` requests).
* **Benefit:** Significantly improves performance and reduces memory usage because EF Core doesn't need to snapshot the data for change detection.

[Go to Top](#)

---

### <a id="q-42"></a>42. What is **Output Caching** (introduced in .NET 7)?

**Answer:** A new middleware that caches the entire HTTP response.

**Brief Explanation:** Unlike Response Caching (which relies on client headers), Output Caching is server-controlled. It can cache responses in Redis or Memory based on Tags, Policy, or Query parameters.
* `app.UseOutputCache()` and `[OutputCache(Duration = 10)]`.

[Go to Top](#)

---

### <a id="q-43"></a>43. How do you **Dockerize** an ASP.NET Core application?

**Answer:** By creating a **Dockerfile**.

**Brief Explanation:**
1.  **Base Image:** `FROM mcr.microsoft.com/dotnet/aspnet:8.0`.
2.  **Build Stage:** Use the SDK image to `dotnet restore` and `dotnet publish`.
3.  **Final Stage:** Copy the published output to the Base (Runtime) image and define the `ENTRYPOINT ["dotnet", "MyApp.dll"]`.

[Go to Top](#)

---

### <a id="q-44"></a>44. What are **HSTS** and **HTTPS Redirection**?

**Answer:** Security best practices enforced by middleware.
* **HTTPS Redirection:** Forces all HTTP requests to redirect to HTTPS (`307 Temporary Redirect`).
* **HSTS (HTTP Strict Transport Security):** A header telling the browser to *always* use HTTPS for this domain for a set period (preventing protocol downgrade attacks).

[Go to Top](#)

---

### <a id="q-45"></a>45. What is **OData** and does ASP.NET Core support it?

**Answer:** **OData (Open Data Protocol)** allows clients to define the query logic (filtering, sorting, selecting fields) in the URL (e.g., `/users?$filter=Age gt 20&$select=Name`).

**Brief Explanation:** Yes, supported via `Microsoft.AspNetCore.OData`. It converts the URL query directly into an Entity Framework LINQ expression (`IQueryable`), optimizing the SQL query.

[Go to Top](#)

---

### <a id="q-46"></a>46. **Scenario:** Compare `System.Text.Json` vs `Newtonsoft.Json`.

**Answer:**
* **`System.Text.Json`:** Default in .NET Core 3+. **High performance**, low memory allocation, strict compliance standards.
* **`Newtonsoft.Json` (Json.NET):** Older standard. More features (handling loops, flexible type conversion), but slower.
* **Advice:** Stick to `System.Text.Json` unless you have legacy feature requirements.

[Go to Top](#)

---

### <a id="q-47"></a>47. How do you track a request across microservices? (**Correlation IDs**)

**Answer:** By passing a unique ID header (e.g., `X-Correlation-Id` or `TraceParent`).

**Brief Explanation:**
1.  Middleware checks for the header. If missing, it creates one.
2.  Middleware adds this ID to the Logging scope (`LogContext`).
3.  `HttpClient` automatically forwards this header to downstream services.
* This allows you to filter logs in your centralized logging system to see the full chain of a request.

[Go to Top](#)

---

### <a id="q-48"></a>48. What are **Source Generators** in .NET?

**Answer:** A compiler feature that generates C# source code during the compilation process.

**Brief Explanation:** In ASP.NET Core, they are used for **High-Performance Logging** (`[LoggerMessage]`) and **JSON Serialization**. They replace runtime Reflection (which is slow) with compile-time code generation, improving startup time and throughput.

[Go to Top](#)

---

### <a id="q-49"></a>49. **Scenario:** How do you return standard error formats? (**ProblemDetails**)

**Answer:** Use the **RFC 7807 Problem Details** standard.

**Brief Explanation:** Instead of returning a random string "Error occurred", return a JSON object:
```json
{
  "type": "[https://example.com/errors/validation](https://example.com/errors/validation)",
  "title": "Validation Failed",
  "status": 400,
  "detail": "Email is required."
}
```

[Go to Top](#)

---

<a id="q-50"></a>50. What is GraphQL and how does it compare to REST in .NET?
Answer: A query language for APIs that allows clients to ask for exactly the data they need.

Brief Explanation:

REST: Fixed endpoints, over-fetching (getting too much data) or under-fetching (need multiple calls).

GraphQL: Single endpoint (/graphql). Client specifies structure.

.NET Implementation: Typically using the Hot Chocolate library.
