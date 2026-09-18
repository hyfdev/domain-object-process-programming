## Domain–Object–Process Programming

DOP Programming organizes programs around Domains, Objects, Processes, and conceptual ownership. Its current expression is a source layout convention. Use the source tree as the primary index of the program's important concepts, and keep implementation details with their conceptual owners.

### Scope

- Each package or crate represents a root Domain within its own source structure. Organize every package or crate using DOP, including each package or crate in a monorepo.
- Workspace directories group packages and crates without automatically establishing conceptual ownership between them. Do not infer Domain relationships from workspace nesting.

### Concepts

- A Domain is a coherent conceptual area with its own vocabulary of Objects and Processes, and may recursively contain Subdomains. Introduce a Subdomain when a distinct conceptual area benefits from owning its own vocabulary; file count and grouping convenience are not sufficient reasons. A Domain need not contain every category.
- An Object is a concept whose state matters when explaining its Domain; that state may be immutable. Keep behavior with an Object when it naturally expresses the Object's responsibilities or maintains its invariants. Do not classify every class or struct as an Object.
- A Process is an architecture-significant activity, transformation, or orchestration. Represent an activity as a Process when understanding the Domain benefits from naming it independently. Do not classify every function as a Process or assign an activity to an Object solely because it receives or modifies that Object.
- Implementation details support an existing concept without needing an independent place in the Domain's vocabulary. Keep helpers, internal types, and execution state with their owner. Decide whether to expose a concept by its responsibility and importance to understanding the Domain, not by syntax, visibility, file size, or number of callers.

### Source layout

- At each Domain root, `objects/`, `processes/`, and `domains/` are DOP's semantic spaces. Place independently meaningful Objects under `objects/`, Processes under `processes/`, and immediate Subdomains under `domains/<name>/`. Apply these rules recursively inside each Subdomain. Create the three directories only when needed.
- Each independent Object or Process has a named home in its Domain. Give it a primary file or directory named after the concept, and keep its implementation there. Follow the language's naming conventions.
- Grouping directories inside `objects/` and `processes/` organize concepts of the same role. Use them freely when useful, and keep the individual concepts identifiable through their names and module entry points. Do not treat a grouping directory as a Subdomain; represent a Subdomain under `domains/`.
- An Object or Process may contain many implementation files and directories. Organize them freely within the owner. Interpret DOP's three semantic spaces at Domain roots; directory names inside a concept's implementation do not establish new DOP concepts by themselves.
- Other files and directories represent ordinary source organization and have no automatic DOP role. Use them for entry points, tests, fixtures, generated code, and implementation support. Do not infer a Domain, Object, or Process solely from their existence or name. Place independently meaningful architecture concepts in their corresponding DOP spaces.
- Language and tooling constraints may require particular physical locations. Preserve required entry points and module behavior when applying DOP. If a concrete technical constraint prevents the usual layout, document the actual ownership mapping in the project's instructions.

### Ownership and dependencies

- The Domain tree expresses conceptual ownership. Determine dependencies from program responsibilities and actual language or project constraints. Do not infer dependency direction from Domain nesting or introduce wrappers solely to route calls through the Domain tree.
- Shared code retains a conceptual owner. Choose its home according to what it means and which responsibility owns it; reuse alone does not require moving it to a parent Domain or creating a shared Domain. Keep support code that belongs to a Domain as ordinary implementation support when it does not warrant an independent concept.

### Working on changes

- A change may extend an existing concept or introduce a new one. Inspect the affected Domain and its existing vocabulary before choosing a location. Extend an existing owner when the responsibility belongs to it; give an independently meaningful new concept a home in the appropriate DOP space.
- Concept boundaries evolve with responsibilities. When an implementation detail becomes independently meaningful, expose it in the appropriate DOP space. When a concept loses its independent responsibility, fold it into its owner or remove it. Keep structural changes within the task's scope.
- The source tree should remain an accurate guide to the program. Before finishing, check that the affected concepts' names, locations, and implementation agree with their responsibilities, and that internal details remain with their owners.
- Changes to the conceptual structure affect how people understand the program. Explain added, moved, split, merged, or removed concepts and their reasons in the normal change summary. Pure implementation changes need no separate DOP report.
