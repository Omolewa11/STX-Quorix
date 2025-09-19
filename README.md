# STX-Quorix - DAO Governance Smart Contract

This Clarity smart contract implements a decentralized autonomous organization (DAO) on the Stacks blockchain. It enables member-based proposal submission, voting, reputation tracking, and fund management through a transparent and democratic process.

---

##  Features

### ✅ DAO Membership

* Members can join by staking a minimum amount of STX.
* Membership grants permission to submit proposals, vote, and delegate votes.
* Reputation points track member participation and can be increased via contract call.

### 🗳️ Proposal Lifecycle

* **Submit Proposals**: DAO members can submit proposals with a title, description, and funding amount.
* **Voting**: Members vote *for* or *against* proposals within the voting period (\~10 days).
* **Delegation**: Vote delegation is supported to another DAO member (not self).
* **Execution**: Approved proposals (meeting approval threshold) can be executed and funds disbursed.

### 💰 Treasury Management

* The DAO maintains a treasury, funded by member contributions or direct funding.
* Proposals that pass and have sufficient treasury balance will trigger fund transfers to the proposer.

### 📊 Reputation System

* Members earn reputation through joining and participation.
* Reputation does not directly affect voting weight but can be used for future feature expansions.

---

## 📚 Data Structures

### `proposals`

Holds all DAO proposals.

```clojure
{
  title: string,
  description: string,
  amount: uint,
  proposer: principal,
  votes-for: uint,
  votes-against: uint,
  status: "active" | "done",
  end-block: uint
}
```

### `votes`

Tracks whether a user has voted on a specific proposal.

### `member-details`

Stores each member’s reputation score.

---

## 📜 Constants

* `VOTING_PERIOD`: \~10 days in blocks (1440)
* `MIN_PROPOSAL_AMOUNT`: Minimum proposal size (`1 STX` = `1,000,000 microSTX`)
* `REQUIRED_APPROVAL_PERCENTAGE`: `70%` vote-for required for approval

---

## 🔐 Errors

Handled using custom error codes:

* `u100`: Not authorized
* `u101`: Invalid proposal
* `u102`: Already voted
* `u103`: Proposal expired
* `u104`: Insufficient funds
* `u105`: Zero amount
* `u106`: Invalid status
* `u107`: Self-delegation not allowed
* `u108`: Invalid title length
* `u109`: Invalid description length

---

## 🧪 Public Functions

| Function                                      | Description                                 |
| --------------------------------------------- | ------------------------------------------- |
| `submit-proposal(title, description, amount)` | Create a new funding proposal               |
| `cast-vote(proposal-id, vote-for)`            | Vote for or against a proposal              |
| `execute-proposal(proposal-id)`               | Execute a successful proposal               |
| `join-dao(stake)`                             | Stake STX and join the DAO                  |
| `delegate-vote(delegate-to, proposal-id)`     | Delegate vote to another member             |
| `fund-dao()`                                  | Contribute STX directly to the DAO treasury |
| `increase-reputation(member)`                 | Manually increase a member's reputation     |

---

## 🧩 Private & Read-only Functions

### Private:

* `is-dao-member(user)`: Checks if a user is a DAO member
* `is-proposal-approved(proposal)`: Calculates if a proposal has reached 70% approval

### Read-only:

* `get-proposal(proposal-id)`: Retrieve proposal details
* `get-treasury-balance()`: Check DAO treasury balance
* `get-member-reputation(member)`: Get a member's reputation score

---

## 🔐 Security & Access Control

* Only DAO members can submit or vote on proposals.
* Voting and delegation is restricted to once per proposal per member.
* Proposal execution requires:

  * Voting period to be over
  * Approval threshold met
  * DAO treasury has enough funds

---

## 📦 Deployment Notes

* Requires Clarity-compatible blockchain (Stacks)
* Ensure `MIN_PROPOSAL_AMOUNT` aligns with your economic model
* Reputation system is basic — consider integrating quadratic or weighted voting in future versions

---

## 📈 Future Improvements

* Weighted voting based on reputation
* Multi-signature treasury control
* Proposal types: signaling vs funding
* Member slashing or rewards
* DAO parameter upgrades

---

## ✅ Example Workflow

1. User joins DAO by staking `1 STX`
2. Member submits proposal requesting `2 STX`
3. Members vote within 10-day period
4. If approved (70%+ FOR), the DAO transfers funds to proposer

---
