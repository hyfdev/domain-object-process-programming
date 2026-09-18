# Domain–Object–Process Programming

**DOP Programming** organizes programs around Domains, Objects, Processes, and conceptual ownership. Its current expression is a source layout convention that keeps the program's important concepts visible in the filesystem.

The aim is to help people understand a program as its implementation grows, including when AI writes much of the code. A reader should be able to find the program's conceptual areas, important stateful concepts, and significant activities through its source structure.

## Adopt DOP

Copy the contents of [DOP.md](./DOP.md) into your project's `AGENTS.md`, preserving the project's existing instructions. The rules apply to every package or crate, including each package or crate in a monorepo.

[DOP.md](./DOP.md) is the complete, canonical rule block for agents. This README provides an introduction and an example. To update an adopted project, replace its DOP section with the current rule block while preserving other project instructions.

## Concepts

| Concept | Meaning | Examples |
| --- | --- | --- |
| Domain | A coherent conceptual area with its own vocabulary of Objects and Processes | Catalog, Checkout, Delivery |
| Object | A meaningful stateful concept with naturally owned behavior | Product, Cart, Order |
| Process | An architecture-significant activity, transformation, or orchestration | PlaceOrder, ApplyDiscount, ShipOrder |

Each package or crate represents a root Domain. Within a Domain, `objects/`, `processes/`, and `domains/` contain its Objects, Processes, and Subdomains. Apply the same convention recursively within Subdomains, creating directories as needed.

An Object can have methods, and a Process can use many functions, internal types, and execution state. The classification describes a concept's responsibility in the program. Implementation details stay with the concept they support.

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

## Principles

- **The filesystem represents conceptual ownership.** Give important concepts a clear home, and keep their implementation details there.
- **Ownership is not dependency.** Domain nesting expresses where concepts belong. Determine dependencies from program responsibilities and actual language or project constraints.
- **Expose concepts, hide implementation details.** Keep the architectural vocabulary focused on what a reader needs to understand the program.

## License

[MIT](./LICENSE) — Copyright (c) 2026 Yunfei He.
