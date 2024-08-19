# micro-frontends-complete-guide

## What are micro-frontends?

1. Divide a monolithic app into multiple, smaller apps.
2. Each smaller app is responsible for a distinct feature of a product.

## Why micro-frontends?

1. Multiple engineering teams can work in isolation.
2. Each smaller app is easier to understand and make changes to.


## Example scenario

Imagine we have `product list` and `cart list` components and we have decided to make each module as a single MFE of a micro-frontend application, like image below: But as you can see from image below, for haveing different MFEs in one monorepo we need to have one additional MFE called `container` or `shell` to combine and integrate different MFEs.
 
<img width="1720" alt="Screenshot 2024-08-17 at 14 11 08" src="https://github.com/user-attachments/assets/8306a027-b5a6-4bfb-a4de-aec71bd56a58">


This process of combining MFEs is called `integration`. 


## What is Integration?

How and when does the container get access to the source code in MFEs.

<img width="1720" alt="Screenshot 2024-08-17 at 16 34 22" src="https://github.com/user-attachments/assets/d0eedc8a-116e-457a-81ae-5795bc827c60">

<img width="1720" alt="Screenshot 2024-08-17 at 16 37 00" src="https://github.com/user-attachments/assets/86dbf01b-8980-4401-a6d3-2478cd898442">


## Build time integration

<img width="1720" alt="Screenshot 2024-08-17 at 16 47 21" src="https://github.com/user-attachments/assets/b814a6ce-32d8-4e6c-aa1a-3e04fd776a4d">

<img width="1720" alt="Screenshot 2024-08-17 at 16 53 54" src="https://github.com/user-attachments/assets/f438d7b8-8433-45f5-b438-818873500bf9">
