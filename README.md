# ITS - DevOps & AI Resources

## Introduction

Resources for ITS students learning to build, operate, and improve software with AI. Start with practical projects, then use debugging and experiments to understand the systems underneath them.

## Resources

- [DevOps Resources](./DEVOPS.md): A document on discussed DevOps topics (Git, Actions, Docker, K8S, Deployments).
- [AI & ML Resources](./AI_ML.md): A document on discussed AI & ML topics (Models, LLMs, ML.NET, Neural networks).

## Projects

### 2025
- [JParser](./projects/2025/jparser) uses TypeScript and Deno to explore how a lexer, parser, and formatter handle JSON.
- [PokeBattle](./projects/2025/pokebattle): API and UI in docker with example GitHub actions workflows and ML.NET examples.
- [WalkGame](./projects/2025/walk-game) is a PyTorch game for exploring how an agent learns to navigate an environment.

### 2024

- [ITS SQL](./projects/2024/its-sql): A distributed No-SQL database engine similar to Cosmos DB with SQL-like query language.

### 2023

- [GitMess](./projects/2023/gitmess): A GitHub backend clone running in Docker for managing repositories.

## How to be a good engineer

Use AI from the beginning to explore, implement, and iterate. You remain responsible for understanding the problem, choosing tradeoffs, and checking the result. A project can contain entirely AI-generated code; the percentage generated says little about either its quality or what you learned.

- **Define the problem**: Identify the user, expected behavior, constraints, and examples of success and failure before requesting an implementation.
- **Understand the system**: Trace requests and data through the application. Read the code involved in a change, especially authorization, persistence, concurrency, and error handling. Use AI explanations as a starting hypothesis and check them against execution and source code.
- **Make architecture decisions**: Define component boundaries, data ownership, interfaces, and failure behavior. Compare a simple design with alternatives using expected load, cost, reliability, and team needs. Add services or orchestration when you can explain the benefit.
- **Delegate clearly**: Give humans and agents bounded tasks, relevant context, interface contracts, and acceptance criteria. Integrate and verify their work. Parallelize independent tasks when the coordination cost is worthwhile.
- **Create feedback loops**: Make it easy to build, run, test, inspect logs, and reproduce failures. An agent needs reliable feedback to improve its implementation; see [AI development and harnesses](./AI_ML.md#ai-assisted-software-development).
- **Work iteratively**: Deliver a small working slice, observe it, and adjust. Check unfamiliar assumptions with documentation, experiments, and measurements.
- **Document decisions**: Record why the system works this way, its limitations, and how to operate it. Use comments for intent and surprising constraints.
- **Own the outcome**: Review security, privacy, deployment, and recovery alongside functionality. Measure useful behavior and maintainability rather than lines generated or typing speed.

### Foundations to learn through experiments

Higher-level decisions need a working understanding of what runs underneath them. Start with a project problem, predict what will happen, then use tools to check the prediction.

| Area | What to understand | Small experiment |
| --- | --- | --- |
| Computer architecture | CPU execution, caches, RAM, storage, and the cost of moving data | Compare sequential and random access over a large array; measure time and explain the difference. |
| Operating systems | Processes, threads, virtual memory, files, permissions, and resource limits | Run a service with a memory limit, observe its failure, and inspect the logs. |
| Networks | DNS, TCP, TLS, HTTP, latency, timeouts, and connection reuse | Trace a request and simulate a slow dependency; distinguish a timeout from an application error. |
| Data systems | Indexes, transactions, isolation, consistency, and recovery | Compare query plans before and after an index; test concurrent updates and restore a backup. |
| System architecture | Boundaries, queues, caching, retries, idempotency, and backpressure | Deliver the same event twice and prove that an operation is applied only once. |

### Example projects

Choose a project with a clear behavior to demonstrate. AI can help implement it; your job is to explain decisions, investigate failures, and provide evidence.

| Project | Practice | Evidence to show |
| --- | --- | --- |
| Operate a small API and database | Architecture, data modeling, deployment, and observability | Trace a request, enforce access control, diagnose a slow query, and demonstrate backup restoration. |
| Build a background job system | Queues, timeouts, retries, and idempotency | Restart a worker during a job and verify the intended result after redelivery. |
| Improve an unfamiliar application | Code reading, debugging, and performance | Reproduce a defect, trace its cause, fix it, and show a regression check or a measured improvement. |
| Create an agent development harness | Task specification, repeatable environments, and verification | Give an agent a bounded issue, record its attempts, and demonstrate that the checks reject a deliberately broken solution. |
| Compare two system designs | Tradeoffs and measurement | Test the same workload against a simple design and one alternative; explain latency, cost, and failure behavior. |

For every project, explain one design decision, walk through a relevant code path, reproduce an injected failure, and make a small change while explaining its effect. Keep a short record of what was delegated and how you verified it. A successful demo alone does not establish understanding.

### Optional internals exercises

Writing a compiler, storage engine, HTTP server, or version control system can still teach useful concepts when the scope matches a learning goal. Prefer a small exercise you can inspect and change over a large generated implementation you cannot explain.

- **Language internals.** Start in [JParser](./projects/2025/jparser/main.ts). Predict the tokens and parsed value for `[1,[2]]`, then step through the parser. Try `[1 2]` and compare the result with `JSON.parse`. Find the branch responsible for the difference and add a check that catches it. A full compiler is an optional next project if language implementation interests you.
- **Storage**: Build a tiny key-value log, interrupt a write, and investigate recovery. Compare lookup behavior with and without an index.
- **HTTP**: Inspect raw requests and implement a small parser in a local lab. Explain how malformed input is handled before comparing with a production server.
- **Version control**: Store content by hash and represent parent commits. Explain how to detect changed content and where merge conflicts come from.

AI may generate the starting implementation. Read the relevant code, predict behavior before running it, break an assumption, and verify a change. If these steps are too difficult, reduce the scope or implement a small part manually to close the gap.

### Tools

Choose an environment that helps you inspect, debug, and deliver work. Windows, macOS, and Linux can all support effective development; learn enough Linux to understand the environments where your services run.

- Learn Git diffs, navigation, a debugger, a profiler, and how to inspect logs and network requests.
- Use the terminal for repeatable commands and scripts; use graphical tools when they make investigation easier.
- Learn shortcuts and automate recurring work when it saves time. Vim motions, Neovim, tmux, and tiling window managers are optional preferences.
- Keep setup, build, test, and run instructions reproducible for both teammates and agents.

### Learning resources and YouTube

Start with a question or experiment. Prefer official documentation and structured courses for technical reference, then use creators for demonstrations and additional perspectives.

| Resource | Use it for |
| --- | --- |
| [Nand to Tetris](https://www.nand2tetris.org/course) | Computer architecture and the software stack through scoped construction projects. Select modules that fit your learning goal. |
| [MIT Missing Semester](https://missing.csail.mit.edu/2026/) | Shell tools, Git, debugging, profiling, shipping software, and agentic coding. |
| [Hussein Nasser](https://www.husseinnasser.com/p/software-engineering.html) | Backend engineering, networking, and database behavior. |
| [Salvatore Sanfilippo](https://www.youtube.com/@antirez) | Programming and engineering discussions; choose material relevant to your current question. |
| [MorroLinux](https://www.youtube.com/@morrolinux) | Linux demonstrations in Italian. |
| [ThePrimeagen](https://www.youtube.com/@ThePrimeagen) | Programming and editor-workflow demonstrations, including Vim. Treat broader tooling and career opinions as perspectives to evaluate. |

No creator is a curriculum or a general authority. Distinguish demonstrations from commentary, check the publication date and underlying sources, and reproduce technical claims. Watching more videos is useful when it helps you answer a question or run a better experiment.

### Working sustainably with others

- Use a comfortable workspace, take breaks, and adjust tools to your needs.
- Ask for feedback early and explain uncertainty. A clear handoff should let another person continue the work.
- Evaluate team processes by whether they improve feedback, coordination, and delivery. Adjust meetings and ceremonies when they stop serving those goals.
