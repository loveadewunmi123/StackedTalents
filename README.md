
# StackedTalents - Talent Marketplace Smart Contract

*A decentralized auction marketplace for talent services, built on Stacks using Clarity.*

## 🌟 Overview

This project implements a robust, secure, and fully on-chain **Talent Service Auction Marketplace** on the Stacks blockchain.
Talents can register, create auctions for their services, and receive bids from clients. Auctions are time-bound, enforce minimum bid increments, and follow strict validation rules to ensure security and fairness.

The contract includes:

* Talent registration
* Auction creation
* Bidding with dynamic minimum increments
* Secure STX transfers
* Fee collection
* Auction completion with earnings attribution
* Read-only helper functions

It incorporates **comprehensive error handling**, **input validation**, and **state verification** to ensure consistent logic and safe token transfers.

---

# 🚀 Features

### 👤 Talent Registration

* Each user can register once.
* Stores metadata including earnings, ratings, and completion count.

### 🔨 Auction Creation

* Talents can create service auctions with:

  * Title, description, category
  * Minimum price constraints
  * Configurable duration (1–30 days)
* Auctions automatically track:

  * Creation height
  * End height
  * Initial price (also minimum bid)

### 💰 Bidding System

* Enforces:

  * Auction must be active
  * Auction must not be expired
  * No self-bidding
  * Minimum bid increment (5% above current highest)
* Bidders pay immediately; previous bidders are refunded.
* A fee (default **5%**) is automatically taken on every bid.

### 🏁 Auction Completion

* Only the talent who created the auction can finalize it.
* Only after the auction end block is reached.
* Payment is released to the talent.
* Talent stats and global stats are updated.

### 📊 Read-Only Functions

* Fetch auctions
* Fetch talent data
* Get global statistics
* Check registration and completion eligibility

---

# ⚙️ Constants & Limits

| Constant               | Description                          |
| ---------------------- | ------------------------------------ |
| `FEE-RATE`             | 5% marketplace fee (50 basis points) |
| `MIN-AUCTION-DURATION` | 1 day (144 blocks)                   |
| `MAX-AUCTION-DURATION` | 30 days (4320 blocks)                |
| `MIN-PRICE`            | 1 STX                                |
| `MAX-PRICE`            | 1,000,000 STX                        |
| `MIN-BID-INCREMENT`    | 5% of current highest bid            |

---

# 🧩 Data Structures

### Talent Profile

```clarity
{
  verified: bool,
  rating: uint,
  total-earnings: uint,
  auctions-completed: uint,
  registration-height: uint
}
```

### Auction Object

```clarity
{
  talent: principal,
  title: string-ascii,
  description: string-ascii,
  price: uint,
  end-height: uint,
  highest-bid: uint,
  highest-bidder: (optional principal),
  status: "active" | "completed",
  category: string-ascii,
  creation-height: uint
}
```

---

# 🧪 Error Codes

| Error | Meaning                        |
| ----- | ------------------------------ |
| `u1`  | Not authorized                 |
| `u2`  | Invalid state                  |
| `u3`  | Not found                      |
| `u4`  | Invalid duration               |
| `u5`  | Insufficient funds             |
| `u6`  | Already registered             |
| `u7`  | Invalid price                  |
| `u8`  | Auction expired                |
| `u9`  | Auction not ended              |
| `u10` | Cannot bid on your own auction |
| `u11` | Invalid bid amount             |
| `u12` | Empty title                    |
| `u13` | Empty description              |
| `u14` | Empty category                 |
| `u15` | Auction not active             |

---

# 🛠️ Public Functions

### `register-talent`

Registers a user as a talent.

### `create-auction (title description category price duration)`

Creates a new talent service auction.

### `place-bid (auction-id amount)`

Places a bid, enforcing minimum increments and handling refunds.

### `complete-auction (auction-id)`

Finalizes the auction, transfers earnings, updates stats.

---

# 🔍 Read-Only Functions

| Function               | Purpose                                 |
| ---------------------- | --------------------------------------- |
| `get-auction`          | Fetch auction details                   |
| `get-talent-info`      | Get talent profile                      |
| `get-contract-stats`   | Total fees + completed auctions         |
| `is-registered`        | Check if a talent is registered         |
| `can-complete-auction` | Returns true if auction can be finished |

---

# 📁 Project Structure (suggested)

```
contracts/
  talent-marketplace.clar
readme.md
tests/
  auction-tests.clar
  talent-tests.clar
```

---

