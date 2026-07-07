# 🎯 Tricentis Tosca — Complete Learning Guide (From Scratch to Advanced)

> A single-file crash course to learn Tosca **fast**. Every concept has a **plain definition**, **why it matters**, and a **practical example**. Read top-to-bottom the first time; use it as a reference after.

---

## 📑 Table of Contents

1. [What is Tosca?](#1-what-is-tosca)
2. [Key Terminology (Glossary First!)](#2-key-terminology-glossary-first)
3. [Model-Based Test Automation (MBTA)](#3-model-based-test-automation-mbta)
4. [Getting Started — Account, Download & Installation](#4-getting-started--account-download--installation)
5. [Tosca Commander — The Workspace](#5-tosca-commander--the-workspace)
6. [The 5 Sections (Explorer Structure)](#6-the-5-sections-explorer-structure)
7. [Modules & Scanning](#7-modules--scanning)
8. [TestCases & TestSteps](#8-testcases--teststeps)
9. [Value Types & The ActionMode](#9-value-types--the-actionmode)
10. [Verifications & Constraints](#10-verifications--constraints)
11. [Buffers & Dynamic Data](#11-buffers--dynamic-data)
12. [Modularization — TestStepBlocks & Reusable Modules](#12-modularization--teststepblocks--reusable-modules)
13. [Libraries & Reusability](#13-libraries--reusability)
14. [Parameters & The TCP (Test Configuration Parameters)](#14-parameters--the-tcp)
15. [Data-Driven Testing (DDT)](#15-data-driven-testing-ddt)
16. [Test Case Design (TCD)](#16-test-case-design-tcd)
17. [Requirements & Risk Management](#17-requirements--risk-management)
18. [ExecutionLists & Running Tests](#18-executionlists--running-tests)
19. [Reporting & Results](#19-reporting--results)
20. [Dynamic Expressions (TBox Expressions)](#20-dynamic-expressions-tbox-expressions)
21. [Recovery Scenarios & Error Handling](#21-recovery-scenarios--error-handling)
22. [API Testing](#22-api-testing)
23. [Mobile Testing](#23-mobile-testing)
24. [Test Data Management (TDM) & Service (TDS)](#24-test-data-management-tdm--service-tds)
25. [Distributed Execution — DEX & Agents](#25-distributed-execution--dex--agents)
26. [Version Control & Multi-User (Workspaces)](#26-version-control--multi-user-workspaces)
27. [CI/CD Integration](#27-cicd-integration)
28. [Tosca Query Language (TQL)](#28-tosca-query-language-tql)
29. [Best Practices](#29-best-practices)
30. [Common Shortcuts & Tips](#30-common-shortcuts--tips)
31. [Sample End-to-End Walkthrough](#31-sample-end-to-end-walkthrough)

---

## 1. What is Tosca?

**Definition:** Tricentis Tosca is a **continuous testing platform** for automated software testing. It automates **functional** and **regression** testing of web, desktop, mobile, and API applications using a **scriptless, model-based** approach.

**Why it matters:** Unlike Selenium or other code-based tools, Tosca lets you build robust automated tests **without writing code**. You "model" the application once, then reuse those models across hundreds of test cases. This makes maintenance dramatically easier.

**Key characteristics:**
- **Scriptless** — drag-and-drop, no programming required (but scripting is available when needed).
- **Model-Based** — you create a model of the UI/API once and reuse it.
- **Technology-agnostic** — supports Web, SAP, Salesforce, APIs, mobile, mainframe, and more.
- **Risk-based** — focuses testing effort on the highest-risk areas.

**Example:** For a login page, you scan the page once to create a "Login Module" containing the Username field, Password field, and Login button. Then any number of test cases (valid login, invalid password, empty fields) reuse that single module.

---

## 2. Key Terminology (Glossary First!)

Learn these words *now* — everything else builds on them.

| Term | Definition |
|------|-----------|
| **Module** | A reusable model of a screen/API that contains the technical objects (controls) you interact with. |
| **ModuleAttribute** | A single control inside a Module (e.g., a textbox, button, link). |
| **TestCase** | A sequence of test steps that validates one scenario. |
| **TestStep** | One instance of a Module used inside a TestCase. |
| **TestStepValue** | The actual value/action assigned to a ModuleAttribute in a TestStep. |
| **ActionMode** | Defines *what* Tosca does with a value: Input, Verify, Buffer, WaitOn, Constraint, Select. |
| **Buffer** | A named variable that stores a value at runtime for later reuse. |
| **ExecutionList** | A collection of TestCases scheduled to run together. |
| **Requirement** | A feature/functionality to be tested, weighted by risk. |
| **TestSheet / TestCaseDesign** | A table-based way to design test data combinations. |
| **Workspace** | Your local working copy connected to a repository. |
| **Repository** | Central storage (single-user file or multi-user DB) for all test assets. |
| **XScan** | The scanning wizard used to capture UI controls into Modules. |
| **Steering** | The act of Tosca controlling an application object (clicking, typing, etc.). |

---

## 3. Model-Based Test Automation (MBTA)

**Definition:** MBTA is Tosca's core methodology where you create an abstract **model** of the system under test, separate from the test logic and the test data. The three layers are:

1. **Modules (the "What")** — technical model of the objects.
2. **TestCases (the "How/Logic")** — the steps and flow.
3. **Test Data (the "With What")** — the input/expected values.

**Why it matters:** Because these three are separated, a UI change only requires updating the **Module** — every test case that uses it is automatically fixed. This is the single biggest reason Tosca scales better than script-based tools.

**Example:** If the "Login" button's ID changes in the app:
- **Selenium:** You'd fix the locator in every script that clicks it.
- **Tosca:** You re-scan/update the button in the Login Module *once*, and all 50 test cases keep working.

---

## 4. Getting Started — Account, Download & Installation

### 4.1 Creating a Tricentis Account

**Step-by-step:**

1. Go to **https://www.tricentis.com** or the **Tricentis Support Portal** at `https://support.tricentis.com`.
2. Click **"Sign Up"** / **"Register"**.
3. Fill in your details — name, **corporate email** (Tosca is enterprise software; personal email may be rejected), company, country.
4. Verify your email through the confirmation link.
5. Log in to the portal. Access to downloads and licenses is usually granted via your organization's Tricentis account/administrator.

> 💡 **Note:** Tosca is licensed commercial software. Most learners get access through their **company's license** or a **trial** requested via a Tricentis sales rep. There is also **Tosca Trial / Tricentis Academy** for learning.

### 4.2 Tricentis Academy (Free Learning + Certification)

- Visit **`https://academy.tricentis.com`**.
- Register with your email.
- Free courses: **Automation Specialist Level 1**, **Level 2**, **API**, **Mobile**, etc.
- Certifications: **AS1 (Automation Specialist Level 1)** is the standard entry certification.

### 4.3 Downloading Tosca

1. Log into the **Support Portal** → **Downloads** section.
2. Download the **Tosca Installer** (e.g., `Tricentis Tosca <version>.exe`) — versions are named like `Tosca 16.x`, `2023.x`, etc.
3. Download your **License** (subscription or offline license file).

### 4.4 System Requirements (typical)

- **OS:** Windows 10/11 (64-bit) or Windows Server.
- **RAM:** 8 GB minimum, 16 GB recommended.
- **.NET Framework** (bundled/required).
- A supported database (SQL Server, Oracle, etc.) **for multi-user** repositories — not needed for single-user.

### 4.5 Installation

1. Run the installer **as Administrator**.
2. Accept the license agreement.
3. Choose components:
   - **Tosca Commander** (the main IDE) ✅
   - **Tosca Server** (optional, for enterprise features like DEX, TDS, reporting).
   - **Tosca Automation Extensions** (browser plugins for Chrome/Edge/Firefox).
4. Complete installation and **restart** if prompted.
5. **Activate the license:** Open Tosca Commander → License Configuration → point to your license server or offline file.
6. **Install browser extensions:** Enable the "Tricentis Tosca" extension in Chrome/Edge (from the settings or Web Store) so Tosca can steer the browser.

### 4.6 First Launch — Create a Workspace

1. Open **Tosca Commander**.
2. On first launch it asks you to **create a Workspace**.
3. Choose repository type:
   - **Single-user (recommended for learning):** creates a local `.tws` file — no database needed.
   - **Multi-user:** connects to a shared database (SQL Server/Oracle) for team collaboration.
4. Name your workspace, choose a location, click **Create**.
5. You're in! You'll see the empty **Explorer** tree.

---

## 5. Tosca Commander — The Workspace

**Definition:** **Tosca Commander** is the main graphical IDE where you build, manage, and execute tests.

**Key UI areas:**
- **Explorer (left):** the tree of all your objects (Modules, TestCases, etc.).
- **Details view / Properties (center-right):** editing area for the selected object.
- **Toolbar / Ribbon (top):** actions like Scan, Run, Verify.
- **Search bar:** quickly find any object.
- **Output / Messages (bottom):** logs, errors, execution feedback.

**Example:** Selecting a TestCase in the Explorer shows its TestSteps in the center pane where you assign values.

---

## 6. The 5 Sections (Explorer Structure)

**Definition:** The Tosca Explorer is organized into standard top-level folders, each with a specific purpose. Understanding these five is understanding Tosca's architecture.

| # | Section | Purpose | Icon Color (mnemonic) |
|---|---------|---------|-----------------------|
| 1 | **Modules** | Store the models of your application (scanned controls). | Blue |
| 2 | **TestCases** | Store the test logic (steps, flow, data). | Green |
| 3 | **ExecutionLists** | Organize and schedule which tests to run. | Orange/Yellow |
| 4 | **Requirements** | Manage requirements, risk, and coverage. | Purple |
| 5 | **TestCaseDesign** | Design test data and combinations (tables/sheets). | Teal |

> 🧠 **Memory aid:** **M**odules → **T**estCases → **E**xecutionLists → **R**equirements → **T**estCaseDesign.

**Example workflow across sections:** You **scan** a page (Modules) → **build** a test (TestCases) → **design data** for it (TestCaseDesign) → **run it** (ExecutionLists) → **track coverage** (Requirements).

---

## 7. Modules & Scanning

### 7.1 Module

**Definition:** A **Module** is the technical model of a part of your application — a web page, a dialog, or an API endpoint. It holds the **controls (ModuleAttributes)** you want to interact with.

**Why it matters:** Modules are the **reusable foundation**. Create once, use in unlimited test cases.

### 7.2 ModuleAttribute

**Definition:** A **ModuleAttribute** represents a single UI control (textbox, button, checkbox, dropdown, link) within a Module. It stores the **identification properties** (locators) Tosca uses to find that control.

### 7.3 Scanning (XScan)

**Definition:** **Scanning** is the process of capturing application controls into a Module using the **XScan** wizard.

**How to scan a web page:**

1. Open the target web page in a supported browser (extension enabled).
2. In Tosca, right-click the **Modules** folder → **Scan** → **Application** (or press the scan shortcut).
3. Choose **XScan** and select the browser window.
4. Hover over controls — Tosca highlights them. Click the controls you want.
5. For each control, pick the best **identification properties** (e.g., ID, Name, Class, XPath) — prefer stable ones like `id`.
6. Rename controls to friendly names (e.g., "txtUsername").
7. Click **Save** — a new Module is created.

**Example:** Scanning a login page yields a **"Login" Module** with:
- `txtUsername` (textbox)
- `txtPassword` (textbox)
- `btnLogin` (button)

> 💡 **Tip:** Choose identification properties that are **unique and stable**. Avoid dynamic IDs; if needed, use a combination or a relative anchor.

---

## 8. TestCases & TestSteps

### 8.1 TestCase

**Definition:** A **TestCase** is a self-contained test scenario made up of ordered **TestSteps** that verify a specific behavior.

**How to create:** Right-click **TestCases** folder → **Create TestCase**.

### 8.2 TestStep

**Definition:** A **TestStep** is created by **dragging a Module** into a TestCase. It represents "use this screen/API at this point in the flow."

### 8.3 TestStepValue

**Definition:** A **TestStepValue** is the concrete value or action you assign to a ModuleAttribute inside a TestStep (e.g., type "admin" into Username).

**How to build a TestCase (example — Login):**

1. Create TestCase → name it "TC_ValidLogin".
2. Drag the **Login Module** onto the TestCase → this creates a TestStep.
3. In the TestStep, fill in values:
   - `txtUsername` → `admin` (ActionMode: **Input**)
   - `txtPassword` → `Pass@123` (ActionMode: **Input**)
   - `btnLogin` → `{CLICK}` (ActionMode: **Input**, action = click)
4. Drag a **Home/Dashboard Module** → add a **Verify** on the welcome message.
5. **Run** it (press **F6** on the TestCase / TestStep).

**Result:** Tosca opens the browser, types credentials, clicks login, and verifies the dashboard.

---

## 9. Value Types & The ActionMode

**Definition:** The **ActionMode** tells Tosca *what to do* with a TestStepValue. This is one of the **most important** concepts in Tosca.

| ActionMode | Symbol | Meaning | Example |
|-----------|--------|---------|---------|
| **Input** | *(default)* | Enter/type a value or perform an action. | Type `admin`, click `{CLICK}`. |
| **Verify** | ✓ | Check that the actual value equals the expected. | Verify total = `100`. |
| **Buffer** | 📥 | Store the actual runtime value into a named buffer. | Save order ID into `OrderNo`. |
| **WaitOn** | ⏳ | Wait until the control reaches a specific state/value. | Wait until spinner disappears. |
| **Constraint** | 🔒 | Narrow down *which* row/instance to act on (used with tables/grids). | Act on the row where Name = `John`. |
| **Select** | (dropdown) | Select an item from a list/dropdown. | Select `India` from country dropdown. |

**How to change ActionMode:** Click the ActionMode column of the TestStepValue and pick from the dropdown (or use keyboard toggles).

**Example combining modes (edit a specific grid row):**
- Column `Name` → ActionMode: **Constraint**, value `John` (find John's row)
- Column `Status` → ActionMode: **Input**, value `Active` (update his status)
- Column `Status` → ActionMode: **Verify**, value `Active` (confirm the update)

---

## 10. Verifications & Constraints

### 10.1 Verification

**Definition:** A **Verification** (ActionMode = Verify) asserts that an actual value in the application matches an expected value. It's how Tosca decides **pass/fail**.

**Example:** After a purchase, verify the confirmation text equals `"Order Placed Successfully"`.

**Operators in verifications** (use with `{...}` expressions):
- `=` equals (default)
- `>`, `<`, `>=`, `<=` numeric comparisons
- `{REGEX[...]}` regular expression match
- `{SUB[...]}` substring/partial match

**Example:** Verify price `{>[100]}` (greater than 100) or verify a code matches `{REGEX[ORD-\d{5}]}`.

### 10.2 Constraint

**Definition:** A **Constraint** (ActionMode = Constraint) restricts the scope to a specific element among many — essential for tables, grids, and repeating structures.

**Example:** In a product table, to click "Buy" for the row where `Product = "Laptop"`:
- `Product` → Constraint → `Laptop`
- `BuyButton` → Input → `{CLICK}`

---

## 11. Buffers & Dynamic Data

**Definition:** A **Buffer** is a named runtime variable that stores a value captured during execution so it can be reused later in the same run (or persisted across runs).

**Why it matters:** Applications generate dynamic values (order numbers, session tokens, generated usernames). Buffers let you capture and reuse them.

**How to create a buffer (capture):**
- Set ActionMode = **Buffer** on a TestStepValue, name it (e.g., `OrderNo`).

**How to use a buffer (retrieve):**
- Reference it with `{B[OrderNo]}` in any later value field.

**Example:**
1. After placing an order, the app shows Order #`ORD-45231`.
2. On that field: ActionMode = **Buffer**, buffer name = `OrderNo`.
3. Later, on the "Track Order" page: type `{B[OrderNo]}` into the search box.
4. Verify the order status page shows the same `{B[OrderNo]}`.

> 💡 You can also **set** a buffer manually with the **Set Buffer** special step (TBox `TBox Set Buffer`).

---

## 12. Modularization — TestStepBlocks & Reusable Modules

### 12.1 TestStepBlock

**Definition:** A **TestStepBlock** is a grouping of TestSteps within a TestCase — used to organize logic (like a folder inside a test) or to loop/repeat a set of steps.

**Example:** Group all login steps into a "Login" block, all checkout steps into a "Checkout" block for readability.

### 12.2 Reusable TestStepBlock (Reusability!)

**Definition:** A **Reusable TestStepBlock** is a block placed under a **Library** that can be referenced by many TestCases. Change it once → all references update.

**Example:** Create a reusable "Login" block. Reference it in 30 test cases. When login changes, edit the one reusable block.

**How to make reusable:**
1. Create/organize TestSteps into a block.
2. Move it under a **Library** folder (see next section).
3. In any TestCase, drag/reference it — Tosca inserts a **reference** (shown with a link icon), not a copy.

---

## 13. Libraries & Reusability

**Definition:** A **Library** is a special folder under TestCases that holds **reusable TestStepBlocks** (and reusable components) shared across the project.

**Why it matters:** Libraries are the backbone of maintainability — encapsulate common flows (login, search, logout) once.

**Example structure:**
```
TestCases
├── Library
│   ├── Login (reusable)
│   ├── Logout (reusable)
│   └── SearchProduct (reusable)
├── TC_ValidLogin       → references Library/Login
├── TC_Checkout         → references Library/Login + Library/SearchProduct
```

---

## 14. Parameters & The TCP

**Definition:** **Parameters** make reusable blocks flexible by passing values in from the calling TestCase. In a reusable block, you define a **parameter** (placeholder); the caller supplies the actual value.

**Syntax:** Reference a parameter with `{X[ParamName]}` (X = "external"/exchange parameter) inside the reusable block.

**Example (parameterized Login):**
- In reusable "Login" block:
  - `txtUsername` → `{X[User]}`
  - `txtPassword` → `{X[Pwd]}`
- In TC_ValidLogin caller: set `User = admin`, `Pwd = Pass@123`.
- In TC_InvalidLogin caller: set `User = admin`, `Pwd = wrong`.

**TCP — Test Configuration Parameters:**

**Definition:** **TCPs** are configuration values (like environment URL, browser type, credentials) defined at a folder/TestCase level and inherited by children. Great for switching environments.

**Example TCP:** Define `BaseURL = https://qa.myapp.com` at the project folder. All tests navigate to `{TCP[BaseURL]}`. To test on staging, change one TCP value.

**Common built-in-style TCPs:** `Browser` (Chrome/Edge/Firefox), navigation URL, timeouts.

---

## 15. Data-Driven Testing (DDT)

**Definition:** **Data-Driven Testing** runs the *same* TestCase logic multiple times with *different* data sets sourced from an external file or Tosca structure.

**Why it matters:** Test many combinations (10 login pairs) without duplicating the test.

**Data sources:**
- **Excel / CSV** files
- **Databases**
- Tosca **TestSheets** (TestCaseDesign)

**How to set up DDT (TestSheet-based):**
1. In **TestCaseDesign**, create a **TestSheet** with columns `User`, `Pwd`, `ExpectedResult`.
2. Add **Instances** (rows) — each row is one data set.
3. Create a **Template TestCase** and link its values to the sheet columns (`{XL[...]}` or attribute links).
4. **Instantiate** — Tosca generates one runnable TestCase per row.

**Excel example:** Use the **TBox** step `Excel` or reference cells with `{XL[SheetName#Column]}` to read data at runtime.

---

## 16. Test Case Design (TCD)

**Definition:** **Test Case Design** is Tosca's structured, table-based method to systematically derive test data and combinations — reducing redundant tests while maximizing coverage.

**Key concepts:**
- **TestSheet:** a table representing a business object/scenario with **Attributes (columns)**.
- **Attribute:** a column (e.g., Age, Country, PaymentType).
- **Equivalence Class Partition:** grouping input values into classes that should behave the same (e.g., Age: `<18`, `18-60`, `>60`).
- **Instance:** a concrete combination (one row) = one test data record.
- **Combinatorics:** automatically generate combinations (e.g., linear, pairwise) across attributes.

**Example — Age eligibility:**
| Attribute | Classes |
|-----------|---------|
| Age | `<18` (invalid), `18–60` (valid), `>60` (senior) |
| Country | India, USA |

Tosca can generate instances covering these classes so you test the meaningful combinations, not every permutation.

**Why it matters:** Design-driven testing ensures **coverage** is intentional and traceable, not random.

---

## 17. Requirements & Risk Management

**Definition:** The **Requirements** section lets you capture what the application must do, assign **risk/weighting**, and link TestCases to measure **coverage** and **risk mitigation**.

**Key terms:**
- **Requirement:** a feature/functionality to test.
- **Weighting:** relative importance/risk (higher weight = more critical).
- **Risk Coverage:** % of weighted risk covered by (passed) tests.

**Why it matters:** Risk-based testing focuses effort where failure hurts most, and produces management-friendly dashboards ("we've mitigated 85% of business risk").

**Example:**
- Requirement "Payment Processing" → weight 10 (critical).
- Requirement "Footer links" → weight 1 (trivial).
- Link test cases → Tosca reports risk coverage. If payment tests pass, the bulk of risk is mitigated.

**How to link:** In a TestCase's properties, associate it with a Requirement; execution results roll up into requirement coverage.

---

## 18. ExecutionLists & Running Tests

### 18.1 ExecutionList

**Definition:** An **ExecutionList** is a container that groups TestCases you want to run together, with **ExecutionEntries** referencing the actual TestCases.

**Why separate from TestCases?** It lets you build different suites (Smoke, Regression, Nightly) from the same test cases without duplicating them.

**How to create & run:**
1. Right-click **ExecutionLists** → **Create ExecutionList** (e.g., "SmokeSuite").
2. **Drag TestCases** into it → creates **ExecutionListEntries**.
3. Select the ExecutionList → press **F6** (Run) — or right-click → **Run**.
4. Watch results (green = pass, red = fail).

### 18.2 Run modes

- **Run (F6):** normal execution.
- **ScratchBook run (F5):** trial/experimental run that does **not** persist results — great for debugging.

> 🧠 **Remember:** **F5 = ScratchBook (no saved log)**, **F6 = Run (saves results/log)**.

**Example:** Build "SmokeSuite" with your 5 critical TestCases; run nightly to catch breakages fast.

---

## 19. Reporting & Results

**Definition:** After execution, Tosca stores **ExecutionLogs** and can generate **reports** showing pass/fail, duration, screenshots, and requirement coverage.

**Where to see results:**
- **ExecutionList → Log** node: each run creates a log with per-step details.
- **Right-click → Create Report** for formatted reports.

**Report types & tools:**
- **Standard Tosca reports** (built-in templates).
- **Tricentis Reporting** / **qTest** integration for enterprise dashboards.
- **Screenshots on failure** — configurable so failed steps capture the screen.

**Example:** After a regression run, open the log to see step 12 failed, view the screenshot, and read the actual vs. expected values.

---

## 20. Dynamic Expressions (TBox Expressions)

**Definition:** Tosca **expressions** (wrapped in `{...}`) let you insert dynamic/calculated values instead of static text. These are extremely powerful.

**Common expressions:**

| Expression | Purpose | Example |
|-----------|---------|---------|
| `{B[name]}` | Read a buffer | `{B[OrderNo]}` |
| `{X[name]}` | Read a parameter | `{X[Username]}` |
| `{TCP[name]}` | Read a Test Config Parameter | `{TCP[BaseURL]}` |
| `{DATE[...]}` | Current/relative date | `{DATE[+1;dd-MM-yyyy]}` (tomorrow) |
| `{TIME[...]}` | Current time | `{TIME[HH:mm:ss]}` |
| `{RANDOM[...]}` | Random value | `{RANDOM[8;ABC123]}` |
| `{RGID[...]}` | Random GUID/unique ID | for unique test data |
| `{SUB[start;len]}` | Substring | extract part of a value |
| `{REGEX[...]}` | Regex match/extract | validate patterns |
| `{CALC[...]}` | Arithmetic calculation | `{CALC[5*3]}` → 15 |
| `{CONCAT[a;b]}` | Concatenate | build strings |
| `{ENVIRONMENT[VAR]}` | Read env variable | machine/user info |

**Example (unique email each run):**
`test{RGID[]}@mail.com` → creates a unique registration email every execution, avoiding "email already exists" errors.

**Example (future date):**
Book a flight for `{DATE[+7;dd/MM/yyyy]}` (7 days from today).

---

## 21. Recovery Scenarios & Error Handling

**Definition:** A **Recovery Scenario** defines automatic actions Tosca takes when a step fails (e.g., close a popup, restart the app, retry) so a whole suite doesn't halt on one error.

**Key error-handling settings (per TestStep or TestStepValue):**
- **IfContinue on failure** — keep running remaining steps even if this one fails.
- **AbortOnError** — stop the test on failure (default for critical steps).
- **Optional / IfExists** — proceed only if a control exists (great for intermittent popups).

**Recovery actions examples:**
- On failure → take screenshot → close browser → re-launch → re-login → continue.
- Handle an unexpected "Session Expired" dialog by dismissing it.

**Why it matters:** Robust suites keep running unattended overnight without a single popup killing 200 tests.

---

## 22. API Testing

**Definition:** Tosca **API Scan** lets you test REST/SOAP APIs — sending requests and verifying responses — using the same model-based approach (no UI needed).

**How to test an API:**
1. Right-click Modules → **API Scan**.
2. Choose protocol: **REST** or **SOAP**.
3. Enter the **endpoint URL**, **method** (GET/POST/PUT/DELETE), **headers**, and **body** (JSON/XML).
4. Send once to capture the response; Tosca models request/response.
5. Save as a Module.
6. In a TestCase, set request values (**Input**) and response checks (**Verify**).

**Example (REST GET):**
- URL: `{TCP[BaseURL]}/api/users/1`
- Method: `GET`
- Verify: response JSON `name = "John"`, status code `200`.

**Example (POST with dynamic body):**
- Body: `{"email":"test{RGID[]}@mail.com","role":"admin"}`
- Buffer the returned `id` → reuse in a follow-up DELETE call.

---

## 23. Mobile Testing

**Definition:** Tosca supports **mobile app testing** (Android/iOS — native, hybrid, web) via the **Tosca Mobile engine** (and device farm/emulator integrations).

**Setup essentials:**
- Install/configure the **Tosca Mobile** components and device connection (real device via USB or emulator/cloud like BrowserStack/Sauce Labs).
- Scan the mobile app screens into Modules (like web scanning).
- Build TestCases with mobile-specific actions (tap, swipe, scroll).

**Example:** Scan the mobile login screen → create "Mobile_Login" TestCase → tap username, enter text, tap login, verify home screen.

---

## 24. Test Data Management (TDM) & Service (TDS)

**Definition:**
- **Test Data Management (TDM)** is the discipline of provisioning, generating, and maintaining the data your tests need.
- **Tosca Test Data Service (TDS)** is a REST-based service that stores and serves test data centrally so tests fetch fresh, valid data at runtime.

**Why it matters:** Tests fail when data is stale, used-up, or duplicated. TDS gives every test run clean, reusable, and unique data.

**Example:** A "create user" test pushes the new user into TDS; a later "login" test pulls that user from TDS — no hardcoding, no collisions.

---

## 25. Distributed Execution — DEX & Agents

**Definition:** **Distributed Execution (DEX)** runs many TestCases in **parallel** across multiple machines/**Agents** coordinated by the **Tosca Server / DEX Monitor**, drastically cutting execution time.

**Key terms:**
- **Agent:** a machine (or service) that executes assigned tests.
- **DEX Monitor:** dashboard to configure and monitor distributed runs.
- **AOS (Automation Object Service):** stores automation objects centrally for agents.

**Example:** 500 regression tests that take 10 hours on one machine finish in ~1 hour across 10 agents running in parallel.

---

## 26. Version Control & Multi-User (Workspaces)

**Definition:** In a **multi-user workspace**, multiple testers connect to a shared repository (database). Tosca uses **check-in / check-out (multi-user versioning)** so changes are merged safely.

**Key concepts:**
- **Check-out:** you take a working copy of objects to edit.
- **Check-in:** you commit changes back to the common repository.
- **Update All / Refresh:** pull the latest changes from others.
- **Conflict resolution:** Tosca flags and helps merge conflicting edits.
- **Branching / Revision history:** track who changed what and when.

**Best practice:** Check in **small, frequent** changes with meaningful comments; update before big edits.

**Example:** Tester A edits the Login Module while Tester B builds a checkout test. Both check in; Tosca merges without overwriting each other's work.

---

## 27. CI/CD Integration

**Definition:** Tosca integrates with CI/CD pipelines so tests run automatically on builds/deployments.

**Mechanisms:**
- **ToscaCI / ToscaCIClient (command-line + remote execution):** trigger ExecutionLists from the pipeline.
- **Plugins:** Jenkins, Azure DevOps, GitHub Actions, Bamboo, TeamCity, etc.
- **REST endpoints** on Tosca Server to start runs and fetch results (JUnit XML for pipeline dashboards).

**Example (Jenkins):**
1. Jenkins job builds the app and deploys to QA.
2. Jenkins calls **ToscaCIClient** to run the "Regression" ExecutionList on a DEX agent.
3. Results return as JUnit XML → Jenkins shows pass/fail and fails the build on regressions.

---

## 28. Tosca Query Language (TQL)

**Definition:** **TQL (Tosca Query Language)** is a query syntax to search and filter objects in the repository (used in search, dynamic ExecutionLists, and API automation of the tool itself).

**Basic examples:**
- `->SUBPARTS` navigate to child objects.
- Filter by name: `=>SUBPARTS:TestCase[Name=="TC_Login*"]`
- Combine conditions to build **dynamic ExecutionLists** that auto-include matching tests.

**Example use:** Create a dynamic ExecutionList that always contains every TestCase tagged "Smoke" — new smoke tests are picked up automatically.

---

## 29. Best Practices

- ✅ **Model once, reuse everywhere** — keep Modules lean and well-named.
- ✅ **Use stable identification properties** — avoid brittle dynamic XPaths.
- ✅ **Modularize** — extract common flows (login/logout) into reusable blocks in a **Library**.
- ✅ **Parameterize** reusable blocks; use **TCPs** for environments.
- ✅ **Separate logic and data** — use TestCaseDesign/DDT rather than hardcoding.
- ✅ **Meaningful names & folders** — clear naming conventions (`TC_`, `M_`, `EL_`).
- ✅ **Add verifications** — a test without a Verify proves nothing.
- ✅ **Handle errors** — recovery scenarios & IfExists for popups.
- ✅ **Check in frequently** with comments (multi-user).
- ✅ **Run in ScratchBook (F5)** while building; **Run (F6)** for real results.
- ✅ **Keep buffers and expressions** for dynamic data — never rely on fixed order IDs.

---

## 30. Common Shortcuts & Tips

| Shortcut | Action |
|----------|--------|
| **F5** | ScratchBook run (temporary, no saved results) |
| **F6** | Run (saves execution log/results) |
| **Ctrl + S** | Save |
| **Ctrl + F** | Search / find objects |
| **F2** | Rename selected object |
| **Del** | Delete selected object |
| **Ctrl + C / V** | Copy / paste objects |
| **Drag Module → TestCase** | Create a TestStep |
| **Drag TestCase → ExecutionList** | Create an ExecutionEntry |

> 💡 **Debug tip:** Use **F5 ScratchBook** to test a single step or partial flow while building — nothing gets logged, so you can experiment freely.

---

## 31. Sample End-to-End Walkthrough

Let's tie it all together with a **complete beginner project: Automating login on a demo site.**

### Step 1 — Create Workspace
- Open Tosca Commander → create **single-user workspace** "MyFirstProject".

### Step 2 — Scan the Application (Modules)
- Open the demo login page in Chrome (extension enabled).
- Right-click **Modules** → **Scan → Application → XScan**.
- Capture `txtUsername`, `txtPassword`, `btnLogin`. Save as Module **"M_Login"**.
- Scan the post-login page for the welcome message → Module **"M_Dashboard"**.

### Step 3 — Build the TestCase (TestCases)
- Create TestCase **"TC_ValidLogin"**.
- Add a step to open the browser/navigate (use TBox "Open URL" or a browser module) → `{TCP[BaseURL]}`.
- Drag **M_Login** → set:
  - `txtUsername` = `admin` (Input)
  - `txtPassword` = `Pass@123` (Input)
  - `btnLogin` = `{CLICK}` (Input)
- Drag **M_Dashboard** → `lblWelcome` = `Welcome, admin` (**Verify**).

### Step 4 — Add a TCP for the environment
- On the project folder, add TCP `BaseURL = https://demo.myapp.com`.

### Step 5 — Make Login Reusable (optional but ideal)
- Move the login steps into a **Reusable TestStepBlock** under **Library**.
- Parameterize: `txtUsername = {X[User]}`, `txtPassword = {X[Pwd]}`.
- In TC_ValidLogin, reference it and pass `User = admin`, `Pwd = Pass@123`.

### Step 6 — Data-Drive it (TestCaseDesign)
- Create a **TestSheet** with columns `User`, `Pwd`, `Expected`.
- Add rows: valid creds, wrong password, empty username.
- Link the template TestCase to the sheet → **instantiate** → 3 TestCases generated.

### Step 7 — Organize an ExecutionList
- Create ExecutionList **"EL_Smoke"** → drag TC_ValidLogin (and the instantiated ones) in.

### Step 8 — Run
- Select **EL_Smoke** → press **F6**.
- Watch the browser drive automatically. Green = pass.

### Step 9 — Review Results & Coverage
- Open the **Log** under the ExecutionList → inspect steps, screenshots.
- Link tests to a **Requirement** "User Login" (weight 10) → view risk coverage.

### Step 10 — (Enterprise) Integrate & Scale
- Configure **DEX** agents for parallel runs.
- Trigger **EL_Smoke** from **Jenkins** via **ToscaCIClient** on each deployment.

🎉 **You've now used all five sections and every core concept — Modules, TestCases, Values, ActionModes, Verifications, Buffers, Reusability, Parameters, TCPs, DDT, TestCaseDesign, ExecutionLists, Requirements, Reporting, DEX, and CI/CD.**

---

## 📚 Quick Revision Cheat Sheet

- **Tosca = scriptless, model-based continuous testing.**
- **5 Sections:** Modules · TestCases · ExecutionLists · Requirements · TestCaseDesign.
- **Flow:** Scan → Build TestCase → Assign Values (ActionModes) → Verify → Run (F6).
- **ActionModes:** Input, Verify, Buffer, WaitOn, Constraint, Select.
- **Reuse:** Modules + Library reusable blocks + Parameters + TCPs.
- **Dynamic data:** `{B[]}` buffer, `{X[]}` param, `{TCP[]}` config, `{DATE[]}`, `{RANDOM[]}`, `{RGID[]}`, `{CALC[]}`.
- **Scale:** DEX (parallel agents), CI/CD (ToscaCIClient), TDS (test data).
- **F5 = ScratchBook (no log), F6 = Run (saved log).**

---

*Happy learning! Build small, reuse everything, verify always. 🚀*
