### C4 System Context Diagram: *"What is Chanchito and who interacts with it?"*
![Untitled - Context](https://github.com/user-attachments/assets/146f0573-3307-4f56-bc1e-fa67e4ad843d)


**Key Decisions & Justifications**:

| **Decision**          | **Why It Was Made**                                                                                          |
| --------------------- | ------------------------------------------------------------------------------------------------------------ |
| Target User: Gen Z | They share expenses often, use mobile apps, and expect minimal friction.                                     |
| Firebase IAM       | We avoid building login/auth infra and gain secure, scalable authentication via Firebase \[OIDC over HTTPS]. |
| Gmail SMTP          | To send reminders and confirmations without owning SMTP infrastructure. Simple and reliable.                 |
| Flutter App        | We need fast, cross-platform mobile development. Flutter offers consistency and speed.                       |
| Keep SQLite Local  | MVP focus. Offline-first capability and no external DB management required.                                  |

---

### C4 Container Diagram: *"What are the main parts of the system?"*
![Untitled - Copy of Context](https://github.com/user-attachments/assets/120157d2-503c-491e-8d84-961281c022bf)
**Containers**:

* **Mobile App (Flutter)**: User-facing interface with fast flows, minimal steps.
* **Service (C# .NET)**: Hosts all backend logic, REST API, and modules.
* **Database (SQLite)**: Embedded local storage — fast, offline-ready.
* **External Systems**: Firebase (IAM), Gmail SMTP (Emails).

**Design/Architecture Decisions**:
![Untitled - Copy of Context (1)](https://github.com/user-attachments/assets/e5b64011-b334-47c4-b64b-14a7811eb93b)

| **Component**                                                             | **Why**                                                                                                                  |
| ------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| Split Modules by Domain (Users, Products, Transactions, Notifications) | Ensures modularity, makes testing and iteration easier.                                                                  |
| Use IAM Module + Firebase SDK                                          | Outsources token validation and identity logic; avoids managing passwords and sessions ourselves.                        |
| Notifications Module                                                   | Handles scheduled email notifications (e.g., pending payments). Separated for future extensibility (e.g., push, in-app). |
| API Layer                                                              | Centralized point to expose business logic and orchestrate other modules.                                                |
| Data Module                                                            | Encapsulates DB access, promotes clean separation between logic and persistence.                                         |

---

### Why These Architectural Choices?

We're an early-stage product solving a **social finance coordination problem**. Our **main goals** are:

* Fast iteration
* Lightweight infra
* Mobile-first experience
* Low ops overhead

Hence:

* Firebase reduces auth complexity.
* SQLite is easy to use, fast, and ideal for MVP.
* Modular backend lets us replace, scale, or refactor parts without rewriting the core.
* Email is done via Gmail SMTP to stay simple and focused.


Enter the diagrams [here](https://miro.com/welcomeonboard/Q2x5dE5UVXNjd0FmUjAvOHZHWWRONzV1cW5USThzVGh2dXQ3WE0vbUxaYVB6bitCZW5LK0VWNXFWbHI3ZG96bGQzNnFSa2xCQk5SOTROU1RHSkZaVkhnRVgybGVKRlNHcS9VS3JnNHNoM0dlTjZiSkFmcW1pQUVLYm9ZOEhRdnhQdGo1ZEV3bUdPQWRZUHQzSGl6V2NBPT0hdjE=?share_link_id=256739994187)