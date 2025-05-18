# 🟧 Bitcoin Lending Protocol (BLP)

**BLP** is a decentralized, smart contract-based lending protocol that enables Bitcoin holders to unlock liquidity without selling their BTC. Built on the Stacks blockchain using Clarity, BLP brings TradFi-inspired capital efficiency and risk management to the Bitcoin ecosystem.

---

## 📌 Overview

### Purpose

BLP allows users to deposit BTC as collateral and borrow supported assets like STX, all while maintaining exposure to Bitcoin. Loans are issued in a permissionless, trust-minimized environment with protocol-enforced collateralization and liquidation mechanics.

### Key Features

* 🔒 **BTC-Backed Loans**: Unlock liquidity while retaining BTC exposure
* ⚖️ **Collateral Ratio Enforcement**: Prevents undercollateralized borrowing
* 💸 **Interest Accrual**: Per block, at fixed rates
* 🚨 **Auto-Liquidation**: Triggered when collateral ratio < 120%
* 📉 **Dynamic Price Feeds**: Admin-managed or oracle-fed
* 🛡 **Governance Controls**: Protocol parameters managed by contract owner
* 📊 **Transparent Metrics**: On-chain loan and platform stats

## ⚙️ Workflow

1. **Initialize Protocol**

   * Called once by `CONTRACT-OWNER`.
   * Ensures the protocol is initialized only once.

2. **Deposit Collateral**

   * User deposits BTC via an off-chain custody bridge.
   * On-chain records updated via `deposit-collateral`.

3. **Request Loan**

   * Based on BTC collateral and current price feed.
   * Must meet `minimum-collateral-ratio`.

4. **Interest Accrual**

   * Flat rate (default: 5%) calculated on repayment over block height.

5. **Repayment**

   * User repays principal + interest.
   * Collateral released; loan marked "repaid".

6. **Liquidation**

   * Triggered if collateral ratio drops below threshold.
   * Collateral seized and position liquidated.

---

## 🧾 Sample Data Structures

### Loan Entry

```clojure
{
  borrower: 'SP123...',
  collateral-amount: u1000,
  loan-amount: u500,
  interest-rate: u5,
  start-height: u10000,
  last-interest-calc: u10000,
  status: "active"
}
```

### User Loan Mapping

```clojure
{ user: 'SP123...', active-loans: (list u1 u2) }
```

### Price Feed

```clojure
{ asset: "BTC", price: u30000 }
```

---

## 🧠 Smart Contract Snippets (Clarity)

### Key Variables

```clarity
(define-data-var total-btc-locked uint u0)
(define-data-var minimum-collateral-ratio uint u150)
(define-data-var liquidation-threshold uint u120)
(define-map loans {loan-id: uint} { ... })
```

### Main Functions

```clarity
(define-public (deposit-collateral (amount uint)))
(define-public (request-loan (collateral uint) (loan-amount uint)))
(define-public (repay-loan (loan-id uint) (amount uint)))
(define-public (liquidate-position (loan-id uint)))
```

### Governance

```clarity
(define-public (update-collateral-ratio (new-ratio uint)))
(define-public (update-liquidation-threshold (new-threshold uint)))
(define-public (update-price-feed (asset (string-ascii 3)) (new-price uint)))
```

---

## 💻 Usage Examples

**1. Deposit BTC Collateral**

```clarity
(contract-call? .blp deposit-collateral u5000000) ;; 0.05 BTC
```

**2. Request a Loan**

```clarity
(contract-call? .blp request-loan u5000000 u100) ;; $100 loan backed by 0.05 BTC
```

**3. Repay Loan with Interest**

```clarity
(contract-call? .blp repay-loan u1 u105) ;; repaying loan ID 1
```

---

## 🛡 Security Considerations

* **Immutable Access Control**: Only `CONTRACT-OWNER` can update protocol parameters.
* **Collateral Protection**: Strict ratio checks before loan issuance.
* **Liquidation Mechanism**: Auto-triggered via real-time ratio calculation.
* **Validation Functions**: Input checks on all user actions.
* **Static Asset List**: Ensures protocol predictability.

---

## 📈 BLP at a Glance

A trustless protocol enabling Bitcoin holders to access liquidity without giving up their BTC — permissionless, secure, and built on Bitcoin's most programmable layer.
