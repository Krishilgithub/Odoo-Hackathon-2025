# 🔧 ReWear – Backend & Database (Hackathon Project)

This backend powers the ReWear platform — a community-driven clothing exchange system using direct swaps or a point-based model. It supports user authentication, item management, swapping logic, admin moderation, and Cloudinary image uploads.

---

## 📦 Tech Stack

- **Backend:** Node.js + Express
- **Database:** PostgreSQL with Prisma ORM
- **Image Uploads:** Cloudinary + Multer
- **Real-Time Communication:** Socket.IO
- **Authentication:** JWT (JSON Web Tokens)

---

## 🧱 Database Design (Prisma Schema)
```prisma

generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}

model User {
  id             String         @id @default(uuid())
  name           String
  email          String         @unique
  passwordHash   String
  points         Int            @default(0)
  createdAt      DateTime       @default(now())
  updatedAt      DateTime       @updatedAt

  items          Item[]         @relation("UserItems")
  swapRequests   SwapRequest[]  @relation("Requester")
  swapsAsOwner   Swap[]         @relation("SwapOwner")
  swapsAsSwapper Swap[]         @relation("SwapSwapper")
}

model Item {
  id           String           @id @default(uuid())
  title        String
  description  String
  category     String
  type         String
  size         String
  condition    String
  tags         String[]
  status       ItemStatus       @default(AVAILABLE)
  createdAt    DateTime         @default(now())

  userId       String
  user         User             @relation("UserItems", fields: [userId], references: [id])

  images       ItemImage[]
  swapRequests SwapRequest[]
  swaps        Swap[]
}

model ItemImage {
  id        String   @id @default(uuid())
  imageUrl  String
  publicId  String
  itemId    String
  item      Item     @relation(fields: [itemId], references: [id])
}

model SwapRequest {
  id           String              @id @default(uuid())
  status       SwapRequestStatus   @default(PENDING)
  createdAt    DateTime            @default(now())

  itemId       String
  item         Item                @relation(fields: [itemId], references: [id])

  requesterId  String
  requester    User                @relation("Requester", fields: [requesterId], references: [id])
}

model Swap {
  id          String       @id @default(uuid())
  method      SwapMethod   // SWAP or POINTS
  completedAt DateTime     @default(now())

  itemId      String
  item        Item         @relation(fields: [itemId], references: [id])

  ownerId     String
  owner       User         @relation("SwapOwner", fields: [ownerId], references: [id])

  swapperId   String
  swapper     User         @relation("SwapSwapper", fields: [swapperId], references: [id])
}

model Admin {
  id            String   @id @default(uuid())
  email         String   @unique
  passwordHash  String
}

enum ItemStatus {
  AVAILABLE
  PENDING
  SWAPPED
  REJECTED
}

enum SwapRequestStatus {
  PENDING
  ACCEPTED
  REJECTED
}

enum SwapMethod {
  SWAP
  POINTS
}

```

### ✅ Entities

- **User**: Stores user info and point balance
- **Item**: Represents a clothing item
- **ItemImage**: Linked to items, stores Cloudinary image URLs
- **SwapRequest**: Sent by users to request an item
- **Swap**: Completed swap record
- **Admin**: For item moderation

### ✅ Relationships

- A user can upload many items
- An item has many images
- An item can have multiple swap requests
- A swap record connects item, owner, and swapper

### ✅ Enums
- `ItemStatus`: AVAILABLE, PENDING, SWAPPED, REJECTED
- `SwapMethod`: SWAP, POINTS
- `SwapRequestStatus`: PENDING, ACCEPTED, REJECTED

---

## 🗄️ Schema Overview

```bash
npx prisma migrate dev --name init
npx prisma generate
