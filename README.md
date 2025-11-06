# Saga Pattern

## Problem

In a **distributed system or microservices architecture**, a single business process often spans multiple services. If one part of the process fails, rolling back the entire transaction becomes difficult because:

* Traditional **ACID transactions** (like in monolithic systems) are **not feasible** across microservices.
* There's no global transaction manager.
* Partial failures can lead to **data inconsistencies**.

> 💡 Example of the problem: In an e-commerce system, placing an order involves reserving inventory, charging the customer, and creating a shipment. If charging fails, how do you undo the inventory reservation?

## Approach

Use the **Saga Pattern** to manage distributed transactions by splitting them into a sequence of **local transactions**, where:

* Each service performs its own transaction and publishes an event.
* If a step fails, previously completed steps are **compensated** (i.e., undone) using **compensation actions**.

There are **two common saga coordination styles**:

1. **Choreography (Event-driven):** Each service listens and reacts to events from others.
2. **Orchestration (Centralized coordinator):** A central service tells others what to do next.

## Examples

**Context:**

A food delivery service with these steps:

1. Create Order
2. Reserve Inventory
3. Charge Payment

**✅ Successful Saga Flow:**

1. Order Service creates the order → publishes `OrderCreated`.
2. Inventory Service reserves items → publishes `InventoryReserved`.
3. Payment Service charges the card → publishes `PaymentCharged`.

**❌ Failure Flow (e.g., Payment Fails):**

1. Order Service creates the order → `OrderCreated`.
2. Inventory Service reserves items → `InventoryReserved`.
3. Payment Service fails → publishes `PaymentFailed`.

Now, the saga triggers **compensation**:

* Inventory Service receives `PaymentFailed` → releases stock (`ReleaseInventory`).
* Order Service receives `PaymentFailed` → marks order as `Cancelled`.
