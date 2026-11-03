## 1. Directory & Layered Architecture

Code execution flow must be strictly unidirectional:
`Middleware` ➔ `Handler` ➔ `Business` ➔ `Repo` ➔ `Database`

### Layer Responsibilities & Boundaries:

- **middleware/**: Infrastructure Gatekeeper (System-level checks: Auth Token presence, Rate Limiting, CORS, IP whitelisting).
- **handler/**: HTTP Layer & Input Validation (Parses payloads, validates data schemas/formats via Zod/Joi, maps domain errors to HTTP responses, and delegates clean data to Business).
- **business/**: Core Business Logic & Domain Rules (Completely decoupled from HTTP frameworks, web context, or raw SQL). Controls database transaction boundaries.
- **repo/**: Data Access Layer (Executes DB queries, ORM calls, and data persistence).
- **db/migrations/**: Schema migration scripts managed via CLI/DevOps (Outside runtime code flow).

> **Rule**: Each layer can ONLY invoke the layer directly below it (`Handler` must NEVER call `Repo` directly).

---

## 2. Error Handling & HTTP Mapping

- **Domain Errors**: The Business layer must throw custom domain errors (e.g., `EntityNotFoundError`, `UnauthorizedError`, `ValidationError`).
- **HTTP Mapping**: The Handler layer captures domain errors and maps them to appropriate HTTP Status Codes (e.g., 404, 401, 400).
- **Standardized Response Payload**: All API responses must follow a unified structure:
  ```json
  {
    "success": false,
    "error": {
      "code": "USER_NOT_FOUND",
      "message": "User with specified ID does not exist"
    }
  }
  ```

---

## 3. Clean Code & Function Naming Rules

### 3.1 Intent-Driven Naming

- Encapsulate data transformations and syntax logic inside descriptive functions.
- **DO**: Call `formatDisplayPrice(amount)` in the Business layer.
- **DON'T**: Inline `amount.toFixed(2).trim()` directly inside business logic.

### 3.2 Pragmatic Abstraction Guidelines

- **DO NOT abstract**: Simple single-method syntax (e.g., `items.length` or `str.toLowerCase()`).
- **MUST EXTRACT** into a standalone function if ANY apply:

1. Method chaining contains **2 or more operations** (e.g., `num.toFixed(2).trim()`).
2. The logic or transformation is used **2 or more times (>= 2)** across the codebase.
3. Raw syntax fails to clearly express domain or business intent.

---

## 4. Strict AI Safety Guardrails (DOs & DON'Ts)

- **DO NOT** modify existing database schema or execute destructive migrations without explicit user instruction.
- **DO NOT** introduce new third-party libraries if current stack capabilities can solve the problem.
- **DO NOT** expose API keys, secrets, or raw database entities in public payloads.
- **ALWAYS** write Unit Tests for newly generated Business logic.
- **ALWAYS** verify that `pnpm build` and `pnpm lint` pass before completing tasks.

---

## 5. Code Example

```typescript
// ❌ BAD: Mixed layers and inline syntax chaining
async function handleUpdateUser(req: Request, res: Response) {
  if (req.headers["authorization"]) {
    // Auth check inside handler
    const user = await db.user.find(req.params.id); // Direct DB query from handler
    if (user.roles.length > 0 && user.status === "ACTIVE") {
      const formatted = user.balance.toFixed(2).trim();
      res.json({ balance: `$${formatted}` });
    }
  }
}

// ✅ GOOD: Clean separation of concerns and intent-driven helpers
// 1. Helper (Intent-Driven Abstraction)
function formatCurrencyDisplay(amount: number): string {
  return `$${amount.toFixed(2).trim()}`;
}

// 2. Business Layer
class UserService {
  async getUserBalance(userId: string): Promise<string> {
    const user = await this.userRepo.findById(userId);
    if (!user) throw new EntityNotFoundError("User not found");
    return formatCurrencyDisplay(user.balance);
  }
}

// 3. Handler Layer
class UserHandler {
  async getBalance(req: Request, res: Response) {
    // Input Schema Validation
    const { userId } = parseUserIdSchema(req.params);
    const balance = await this.userService.getUserBalance(userId);
    return res.status(200).json({ success: true, data: { balance } });
  }
}
```
