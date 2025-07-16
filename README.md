# 🏦 Aave V2 DeFi Wallet Credit Scoring

This project builds a credit scoring system (range: 0 to 1000) for wallets interacting with the Aave V2 DeFi protocol, using raw transaction-level data. The goal is to assign scores that reflect the financial behavior and trustworthiness of wallets based on their on-chain actions.

---

## 📌 Objective

Given historical transaction data (`deposit`, `borrow`, `repay`, `redeemunderlying`, `liquidationcall`) for 100,000+ wallets, we:

- Engineer wallet-level behavioral and financial features
- Analyze risk and usage patterns
- Assign each wallet a **credit score between 0 (risky) and 1000 (reliable)**

---

## 📂 Dataset

Raw transaction dataset provided in JSON format.

Each record contains:
- `userWallet`: Wallet address
- `action`: Aave protocol action
- `actionData`: Nested data like `amount`, `asset`
- `timestamp`: Time of transaction
- Other metadata (blockNumber, txHash, etc.)

---

## ⚙️ Feature Engineering

We aggregated transactions per wallet to derive:

| Feature Name            | Description |
|-------------------------|-------------|
| total_transactions      | Total number of interactions |
| txn_per_day             | Average transactions per active day |
| deposit, borrow, repay  | Total amounts for each action |
| repay_to_borrow         | How much was repaid compared to borrowed |
| redeem_to_deposit       | Ratio of funds redeemed vs. deposited |
| liquidationcall         | Number and value of liquidations |
| net_balance             | deposit - borrow - liquidations |

---

## 🧠 Scoring Logic

We applied a **rule-based heuristic scoring model**, where:

- ✅ **Higher score for:**
  - High repay-to-borrow ratio
  - High deposit volume
  - Regular and consistent activity

- ❌ **Penalty for:**
  - Liquidations (more = lower trust)
  - Very low or bot-like activity

Each wallet starts with a base score of 500. Features are scaled and added/subtracted. Final score is **clipped between 0 and 1000**.

---

## 🏁 How to Run

### 📌 Prerequisites

- Python 3.7+
- pandas, numpy, matplotlib

Install dependencies:
```bash
pip install pandas numpy matplotlib
