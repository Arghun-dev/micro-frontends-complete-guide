# micro-frontends-complete-guide

## What are micro-frontends?

1. Divide a monolithic app into multiple, smaller apps.
2. Each smaller app is responsible for a distinct feature of a product.

## Why micro-frontends?

1. Multiple engineering teams can work in isolation.
2. Each smaller app is easier to understand and make changes to.


## Example scenario

Imagine we have `product list` and `cart list` components and we have decided to make each module as a single MFE of a micro-frontend application, like image below: But as you can see from image below, for haveing different MFEs in one monorepo we need to have one additional MFE called `container` or `shell` to combine and integrate different MFEs.
 
<img width="1720" alt="Screenshot 2024-08-17 at 14 06 32" src="https://github.com/user-attachments/assets/29393e35-a462-4a41-ba60-61a2fbab27f9">
