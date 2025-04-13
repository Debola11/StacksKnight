# Stacks Knight Gaming Platform Smart Contract

A Clarity smart contract enabling ownership, transfer, and marketplace trading of in-game digital assets. Built for the **Stacks blockchain**, this contract powers asset minting, player progression, and decentralized asset commerce with support for batch operations.

---

## 📄 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Contract Architecture](#contract-architecture)
- [Constants & Error Codes](#constants--error-codes)
- [Data Structures](#data-structures)
- [Public Functions](#public-functions)
  - [Minting](#minting)
  - [Transfers](#transfers)
  - [Marketplace](#marketplace)
  - [Player Stats](#player-stats)
- [Read-only Functions](#read-only-functions)
- [Error Handling](#error-handling)
- [Security Considerations](#security-considerations)
- [Deployment Notes](#deployment-notes)
- [License](#license)

---

## 🧠 Overview

This smart contract supports a gaming ecosystem where:

- Developers (contract owner) can mint and distribute in-game assets.
- Players can transfer assets securely and trade them on a decentralized marketplace.
- Player stats (level, experience) can be stored and updated on-chain.

---

## ✨ Features

- ✅ Asset minting (single and batch)
- 🔁 Secure asset transfers (single and batch)
- 💰 Decentralized marketplace for asset trading
- 📊 Player stats management
- 🔒 Access control for minting
- ⚙️ Batch limits for gas optimization
- 📜 Read-only queries for client-side integration

---

## 🏗️ Contract Architecture

This contract is built using [Clarity](https://docs.stacks.co/write-smart-contracts/clarity) and is intended to run on the Stacks blockchain. It defines maps and functions to handle:

- Asset data (ownership, metadata, transferability)
- Asset pricing and listings
- Player experience and level
- Marketplace listings and STX-based purchases

---

## 📌 Constants & Error Codes

| Constant             | Description                                   |
|----------------------|-----------------------------------------------|
| `contract-owner`     | The deployer of the contract                  |
| `max-level`          | Maximum player level (100)                   |
| `max-experience`     | Maximum experience points (10,000)           |
| `max-metadata-length`| Max allowed metadata URI length (256)        |
| `max-batch-size`     | Max allowed items per batch operation (10)    |

### Error Codes

| Code        | Meaning                       |
|-------------|-------------------------------|
| `err u100`  | Only contract owner allowed   |
| `err u101`  | Asset not found               |
| `err u102`  | Unauthorized operation        |
| `err u103`  | Invalid input                 |
| `err u104`  | Invalid price                 |

---

## 🗂️ Data Structures

### `assets`
Tracks each asset's:
- `owner` (principal)
- `metadata-uri` (string-utf8, 256)
- `transferable` (bool)

### `asset-prices`
Stores prices for assets (uint)

### `player-stats`
Tracks players’:
- `experience` (uint)
- `level` (uint)

### `marketplace-listings`
Marketplace listings with:
- `seller` (principal)
- `price` (uint)
- `listed-at` (block height)

### `asset-counter`
Keeps a running count of all minted assets.

---

## 🔓 Public Functions

### 🛠️ Minting

#### `mint-asset(metadata-uri, transferable)`
- Mints a single asset.
- Restricted to contract owner.

#### `batch-mint-assets(metadata-uris, transferable-list)`
- Mints multiple assets in one call.
- Max 10 assets per call.

### 🔁 Transfers

#### `transfer-asset(asset-id, recipient)`
- Transfers a single asset.
- Sender must be owner and asset must be transferable.

#### `batch-transfer-assets(asset-ids, recipients)`
- Transfers multiple assets in one call.
- Ownership and transferability checked for each asset.

### 🛒 Marketplace

#### `list-asset-for-sale(asset-id, price)`
- Lists an asset for sale.
- Only owner can list, asset must be transferable.

#### `purchase-asset(asset-id)`
- Purchases a listed asset using STX.
- Transfers asset to buyer and payment to seller.

#### `delist-asset(asset-id)`
- Removes an asset from the marketplace.
- Only the listing seller can delist.

### 🧑‍🎮 Player Stats

#### `update-player-stats(experience, level)`
- Allows a player to set/update their experience and level.
- Values must be within the allowed max thresholds.

---

## 🧐 Read-only Functions

### `get-asset-details(asset-id)`
Returns metadata and owner of an asset.

### `get-marketplace-listing(asset-id)`
Returns listing info (price, seller, listed-at).

### `get-player-stats(player)`
Returns a player’s experience and level.

### `get-total-assets()`
Returns total number of assets minted so far.

---

## ⚠️ Error Handling

All functions use `asserts!` and `unwrap!` to prevent invalid operations and ensure consistent state. Specific error codes are returned on failure, facilitating integration with frontends or game engines.

---

## 🔐 Security Considerations

- Only the `contract-owner` can mint assets.
- Asset transfers check for valid ownership and transferability.
- Self-transfers are disallowed.
- Listings must be removed by the seller or fulfilled via purchase.
- Batch operations are capped to avoid gas exhaustion.

---

## 🚀 Deployment Notes

- Set `contract-owner` at deployment (default is `tx-sender`).
- Ensure front-end clients respect read-only queries for gas optimization.
- Price fields are denominated in micro-STX.

---
