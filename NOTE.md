### Purpose

Use **GitHub** to manage articles you want to read, from quick capture to structured reflection, with **automation but explicit control**.

---

### Entities & structure

#### Issue = inbox / task

* **Required**

  * Title
  * URL
* **Optional**

  * Category via **GitHub labels**

    * Format: `topic:pl/compilers`, `topic:math/category-theory`, etc.
  * Key point (free text)
* **Scheduling**

  * Issue body may contain:

    ```
    due: YYYY-MM-DD
    ```

#### Markdown file = curated knowledge

* Created only via **manual GitHub Actions dispatch**
* Input: **issue number**
* Must include:

  * Summary
  * Interesting points
  * Unfamiliar points

---

### Automation behavior

#### Periodic GitHub Action (daily)

* Scan issues
* Select issues whose body contains `due: YYYY-MM-DD`
* If **no “Today’s reading (YYYY-MM-DD)” issue exists**:

  * Create one
  * List all due issues (links, titles)
* If one already exists:

  * **Do nothing**

#### Manual GitHub Action

* Triggered manually
* Input: issue number
* Promotes the issue into a Markdown file

---

### Key design principles (implicit but clear)

* Low-friction capture, high-quality reflection
* Explicit promotion (no accidental knowledge inflation)
* Labels = taxonomy, Actions = behavior, Issues = state

---
