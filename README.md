# Ethereum DApp Collection
### Truffle · Ganache · MetaMask · Web3.js
**Prepared by: RCA | Teacher: Awet Fesseha**

---

## Projects

| # | Folder | DApp | Contract |
|---|--------|------|----------|
| 1 | `donation/` | Decentralized Scholarship Funding | `Scholarship.sol` |
| 2 | `voting/` | On-Chain Voting System | `Voting.sol` |
| 3 | `token/` | ERC-20-Style Token | `SimpleToken.sol` |
| 4 | `auction/` | Decentralized Auction | `Auction.sol` |
| 5 | `todo/` | On-Chain Todo List | `TodoList.sol` |

---

## Prerequisites

Install these once — they apply to all five projects.

| Tool | Version | Install |
|------|---------|---------|
| Node.js | v16+ | https://nodejs.org |
| Truffle | latest | `npm install -g truffle` |
| Ganache GUI | latest | https://archive.trufflesuite.com/ganache/ |
| MetaMask | latest | Chrome/Firefox extension — https://metamask.io |
| http-server | latest | `npm install -g http-server` |

Verify your installs:
```bash
node -v
truffle version
```

---

## One-Time Setup — MetaMask + Ganache

Do this once before running any project.

**1. Start Ganache**
- Open the Ganache GUI and click **Quickstart Ethereum**.
- It starts a local blockchain on `http://127.0.0.1:7545`.
- Note the **mnemonic phrase** shown at the top.

**2. Import the mnemonic into MetaMask**
- Open MetaMask → click the account icon → **Import Wallet**.
- Paste the Ganache mnemonic phrase and set a password.

**3. Add the Ganache network to MetaMask**
- MetaMask → Settings → Networks → **Add a network manually**:
  - **Network Name:** Ganache Local
  - **RPC URL:** `http://127.0.0.1:7545`
  - **Chain ID:** `1337`
  - **Currency Symbol:** ETH
- Click **Save** and switch to this network.

> If you use `ganache-cli` instead of the GUI, change the port in `truffle-config.js` from `7545` to `8545`.

---

## Running Each Project

Every project follows the same four steps. Replace `<folder>` with the project name.

```
Step 1 — open the project folder
Step 2 — compile the smart contract
Step 3 — deploy to Ganache
Step 4 — serve the frontend and open in browser
```

---

### Project 1 — Donation (Scholarship Funding)

```bash
cd donation
truffle compile
truffle migrate
```

Copy the contract address printed after `truffle migrate`, then open:

```
donation/client/web3.js  →  line 1
```

Replace `"YOUR_CONTRACT_ADDRESS"` with the copied address.

```bash
cd client
http-server
```

Open **http://localhost:8080** in your browser.

**What you can do:**
- Connect Wallet
- Donate ETH to the scholarship pool
- Apply for a scholarship
- Release funds to a recipient *(admin/deployer only)*

---

### Project 2 — Voting

```bash
cd voting
truffle compile
truffle migrate
```

Copy the contract address, then open:

```
voting/client/app.js  →  line 2
```

Replace `"YOUR_CONTRACT_ADDRESS"` with the copied address.

> **Important:** For the Voting DApp the server **must be started from the `voting/` root**, not from inside `client/`. This is because `app.js` loads the contract ABI from `/build/contracts/Voting.json`, which is relative to the server root.

```bash
# Run from inside the voting/ folder, NOT from voting/client/
http-server
```

Open **http://localhost:8080/client/** in your browser.

**What you can do:**
- Connect Wallet
- View candidates (Rama, Nick, Jose pre-loaded)
- Vote for a candidate *(one vote per address)*
- Add a new candidate *(deployer/owner only)*

---

### Project 3 — Token (SimpleToken)

```bash
cd token
truffle compile
truffle migrate
```

Copy the contract address, then open:

```
token/client/app.js  →  line 2
```

Replace `"YOUR_CONTRACT_ADDRESS"` with the copied address.

```bash
cd client
http-server
```

Open **http://localhost:8080** in your browser.

**What you can do:**
- Connect Wallet
- View token name, symbol, total supply, and your balance
- Mint new tokens to any address *(owner only)*
- Transfer tokens to another address
- Approve a spender with an allowance
- Burn tokens from your own balance

---

### Project 4 — Auction

```bash
cd auction
truffle compile
truffle migrate
```

> The auction is deployed with a **5-minute timer** for local testing. To change the duration, edit `migrations/2_deploy_auction.js` and update the second argument (seconds).

Copy the contract address, then open:

```
auction/client/app.js  →  line 2
```

Replace `"YOUR_CONTRACT_ADDRESS"` with the copied address.

```bash
cd client
http-server
```

Open **http://localhost:8080** in your browser.

**What you can do:**
- Connect Wallet
- View item name, highest bid, countdown timer
- Place a bid in ETH *(must exceed current highest)*
- Withdraw your ETH if you were outbid
- End the auction after the timer expires *(owner only)*

---

### Project 5 — Todo List

```bash
cd todo
truffle compile
truffle migrate
```

Copy the contract address, then open:

```
todo/client/app.js  →  line 2
```

Replace `"YOUR_CONTRACT_ADDRESS"` with the copied address.

```bash
cd client
http-server
```

Open **http://localhost:8080** in your browser.

**What you can do:**
- Connect Wallet
- Create a new task (stored permanently on-chain)
- View all tasks with status (Pending / Done) and creator
- Toggle your own tasks between complete and incomplete
- Refresh the task list at any time

---

## Quick-Reference Cheat Sheet

```bash
# ── Terminal commands (run inside each project folder) ──────────────────

truffle compile          # Compile Solidity contracts → generates build/
truffle migrate          # Deploy to Ganache (note the contract address)
truffle migrate --reset  # Redeploy from scratch (new address each time)

http-server              # Serve the client/ folder on http://localhost:8080
http-server -p 3000      # Use a different port if 8080 is taken
```

---

## Troubleshooting

**`truffle migrate` fails with "connection refused"**
→ Make sure Ganache is running and the port in `truffle-config.js` matches (`7545` for GUI, `8545` for CLI).

**MetaMask shows wrong network**
→ Switch MetaMask to the **Ganache Local** network you added during setup.

**"Please install MetaMask" alert in browser**
→ Install the MetaMask browser extension and refresh the page.

**Transaction fails with "revert"**
→ Open the browser console (F12) for the full error message. Common causes: already voted, already applied, not the owner, insufficient balance.

**Contract address is wrong / functions fail silently**
→ Re-run `truffle migrate --reset`, copy the new address, and update `app.js` (or `web3.js` for the donation project).

**`http-server` port already in use**
→ Run `http-server -p 3001` (or any free port) and open that port in your browser.

**Voting DApp shows "Could not load contract ABI"**
→ Make sure you started `http-server` from the `voting/` folder (not `voting/client/`), and that you ran `truffle compile` first so `build/contracts/Voting.json` exists.

---

## Project Structure Reference

```
Web3/
├── README.md                  ← this file
│
├── donation/
│   ├── contracts/
│   │   ├── Migrations.sol
│   │   └── Scholarship.sol
│   ├── migrations/
│   │   ├── 1_initial_migration.js
│   │   └── 2_deploy_scholarship.js
│   ├── client/
│   │   ├── index.html
│   │   └── web3.js            ← update contractAddress here
│   ├── truffle-config.js
│   └── .gitignore
│
├── voting/
│   ├── contracts/
│   │   ├── Migrations.sol
│   │   └── Voting.sol
│   ├── migrations/
│   │   ├── 1_initial_migration.js
│   │   └── 2_deploy_voting.js
│   ├── client/
│   │   ├── index.html
│   │   └── app.js             ← update contractAddress here
│   ├── truffle-config.js
│   └── .gitignore
│
├── token/
│   ├── contracts/
│   │   ├── Migrations.sol
│   │   └── SimpleToken.sol
│   ├── migrations/
│   │   ├── 1_initial_migration.js
│   │   └── 2_deploy_token.js
│   ├── client/
│   │   ├── index.html
│   │   └── app.js             ← update contractAddress here
│   ├── truffle-config.js
│   ├── .gitignore
│   └── QUESTIONS.md
│
├── auction/
│   ├── contracts/
│   │   ├── Migrations.sol
│   │   └── Auction.sol
│   ├── migrations/
│   │   ├── 1_initial_migration.js
│   │   └── 2_deploy_auction.js
│   ├── client/
│   │   ├── index.html
│   │   └── app.js             ← update contractAddress here
│   ├── truffle-config.js
│   ├── .gitignore
│   └── QUESTIONS.md
│
└── todo/
    ├── contracts/
    │   ├── Migrations.sol
    │   └── TodoList.sol
    ├── migrations/
    │   ├── 1_initial_migration.js
    │   └── 2_deploy_todo.js
    ├── client/
    │   ├── index.html
    │   └── app.js             ← update contractAddress here
    ├── truffle-config.js
    ├── .gitignore
    └── QUESTIONS.md
```
