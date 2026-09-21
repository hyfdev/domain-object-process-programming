<!-- DOP:START -->
## Domain–Object–Process Programming

This project follows [DOP Programming](https://github.com/hyfdev/domain-object-process-programming), expressed as a source layout convention. The source tree represents conceptual ownership.

### Definitions

- **Domain:** a coherent conceptual area that owns a vocabulary of Objects and Processes, and may contain Subdomains.
- **Object:** a stateful concept together with its naturally owned behavior. Its state may be immutable.
- **Process:** an independently meaningful activity, transformation, or orchestration, rather than an Object's intrinsic operation.
- **Implementation detail:** code that supports an existing concept without an independent responsibility in its Domain.

### Rules

1. **Roots.** Organize every package or crate as a root Domain within its source structure. Workspace nesting does not establish Domain ownership.
2. **Concept locations.** At each Domain root, place Objects in `objects/`, Processes in `processes/`, and immediate Subdomains in `domains/<name>/`. Apply the same rules recursively within Subdomains. Create directories only when needed.
3. **Concept boundaries.** Give each Object or Process a primary file or directory named after it. Keep its implementation details there, including helpers, internal types, and execution state. Classify by conceptual responsibility, not by language construct, file count, or number of callers.
4. **Groups.** Directories within `objects/` or `processes/` may group peer concepts or implement one concept. Make the distinction clear through names and module entry points. A group does not introduce a Domain; the three DOP directory roles apply at Domain roots.
5. **Ordinary structure.** Other files and directories have no automatic DOP role. Use them for entry points, tests, fixtures, generated code, and Domain-owned implementation support. Preserve language-required locations; document any necessary departure from the DOP layout in project instructions.
6. **Ownership.** Keep an Object's intrinsic operations with that Object. Give shared code an owner according to its responsibility; reuse alone does not move it to a parent or shared Domain. Domain containment imposes no dependency direction. Follow actual language and project dependency constraints.
7. **Changes.** Place a change with its existing conceptual owner. Introduce a new Object, Process, or Subdomain when it has a distinct responsibility needed to explain the Domain. Update names and locations when responsibilities change; keep unrelated restructuring outside the task.
<!-- DOP:END -->
