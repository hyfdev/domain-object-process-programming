# Domain–Object–Process Programming

**DOP Programming** organizes programs around Domains, Objects, Processes, and conceptual ownership. Its current expression is a source layout convention that keeps the program's important concepts visible in the filesystem.

The aim is to help people understand a program as its implementation grows, including when AI writes much of the code. A reader should be able to find the program's conceptual areas, important stateful concepts, and significant activities through its source structure.

## Adopt DOP

Install the setup skill:

```sh
npx skills add hyfdev/domain-object-process-programming --skill dop-setup -g
```

In your project, ask your agent to use `dop-setup`. The skill installs the complete DOP rules into the root `AGENTS.md`, or an existing `CLAUDE.md` when the project uses that instead. It preserves other instructions and project-specific additions. In a monorepo, run setup once at the workspace root; each package or crate follows DOP within its own source structure.

Agents follow the installed rules during development. Setup changes instructions only; reorganizing an existing codebase is a separate task. To update the rules, update the skill and run `dop-setup` in the project again:

```sh
npx skills update dop-setup -g
```

For manual setup, copy the complete [raw DOP rule block](https://raw.githubusercontent.com/hyfdev/domain-object-process-programming/main/skills/dop-setup/assets/DOP.md), including its markers, into your project's `AGENTS.md`. Keep project-specific decisions outside `<!-- DOP:START -->` and `<!-- DOP:END -->`.

## Concepts

| Concept | Meaning | Examples |
| --- | --- | --- |
| Domain | A conceptual area of the program | Catalog, Checkout, Delivery |
| Object | Something that holds state and has naturally owned behavior | Product, Cart, Order |
| Process | An activity with its own purpose | PlaceOrder, ApplyDiscount, ShipOrder |

Each package or crate represents a root Domain. Within a Domain, `objects/`, `processes/`, and `domains/` contain its Objects, Processes, and Subdomains. Apply the same convention recursively within Subdomains, creating directories as needed.

For example, `Cart.add_item()` belongs to Cart because it updates the cart while maintaining its rules. PlaceOrder is a Process that coordinates the work of placing an order. A helper used only to implement that process stays inside it. [DOP.md](./skills/dop-setup/assets/DOP.md) contains the definitions and placement rules.

## Example

This example shows an online store package. Checkout owns the concepts involved in placing an order. The filenames use TypeScript conventions.

```text
<package-source-root>/
├── index.ts
├── objects/
│   ├── customer.ts
│   └── product.ts
├── domains/
│   └── checkout/
│       ├── objects/
│       │   ├── cart.ts
│       │   └── orders/
│       │       ├── order.ts
│       │       └── order_item.ts
│       └── processes/
│           └── place_order/
│               ├── index.ts
│               ├── calculate_total.ts
│               └── validate_items.ts
└── fixtures/
```

- `domains/checkout/` declares a Subdomain with its own vocabulary: Cart, Order, OrderItem, and PlaceOrder.
- Within Checkout, `objects/orders/` groups Order and OrderItem. It does not introduce a Subdomain.
- `processes/place_order/` implements one Process. Calculating the total and validating items are internal steps in this example.
- The package's `index.ts` and `fixtures/` follow ordinary source organization and have no automatic DOP role.

## License

[MIT](./LICENSE) — Copyright (c) 2026 Yunfei He.
