# 🛡 WalletGuard Inspector

**See what you're really signing before a drainer does.**

![License](https://img.shields.io/badge/license-MIT-green)
![Price](https://img.shields.io/badge/price-free-brightgreen)
![Privacy](https://img.shields.io/badge/runs-100%25%20offline-blue)
![Dependencies](https://img.shields.io/badge/dependencies-none-lightgrey)

**[▶ Try it live](https://astapyth.github.io/walletguard-inspector/)** · [How it works](#%EF%B8%8F-how-it-works) · [What it detects](#-what-it-detects) · [Usage](#-usage) · [FAQ](#-faq)

---

## 📖 Why this tool exists

Most wallet thefts today don't involve hacking. They involve a signature.

A victim visits a convincing fake website, clicks "Claim", "Verify", or "Connect", and their wallet shows a request full of hex data they can't read. One click later, a drainer contract has permission to move their tokens or NFTs, often without the victim sending a single transaction or paying any gas.

WalletGuard Inspector closes that gap. Paste the transaction or signature request your wallet is showing you, and it tells you in plain English what will actually happen if you approve it.

## ✨ Key features

- **Plain-language decoding** of the most common drainer techniques
- **Clear risk verdict** (High Risk / Caution / No known pattern) at a glance
- **100% client-side**: no server, no tracking, no network requests
- **Zero dependencies**: one HTML file you can read, audit, and run offline
- **Never asks for a seed phrase or private key**, ever
- **Built-in examples** that show real drainer patterns safely

## 🔍 What it detects

| Pattern | Why it's dangerous | Risk |
|---|---|---|
| Unlimited `approve` / `increaseAllowance` | Lets a contract spend all of a token, now and forever | ⛔ High |
| `setApprovalForAll(true)` | Hands over control of an entire NFT collection | ⛔ High |
| Permit2 signatures (`PermitSingle`, `PermitBatch`, `PermitTransferFrom`…) | Moves tokens with no transaction and no gas, disguised as a "sign in" | ⛔ High |
| EIP-2612 and DAI-style `Permit` | Gasless approvals with no further confirmation | ⛔ High |
| Seaport `OrderComponents` | Fake listings that sell NFTs for almost nothing | ⛔ High |
| `eth_sign` on a raw hash | Blind signature that can authorize anything | ⛔ High |
| `multicall` bundles | Can hide an approval among harmless-looking steps | ⚠️ Caution |
| `transfer` / `transferFrom` / `safeTransferFrom` | Direct asset movement to a third party | ⚠️ Caution |
| Unknown function selectors | Can't be verified, so they need manual review | ⚠️ Caution |
| Very long expiry / deadline | Permission stays usable far into the future | ⚠️ Caution |
| Approvals set to zero, `setApprovalForAll(false)` | Revokes: these are safe | ✅ Info |

## ⚙️ How it works

WalletGuard Inspector reads the function selector (the first four bytes of the calldata) and decodes the arguments that follow: spender addresses, amounts, operators, and deadlines. For EIP-712 typed data it inspects the `primaryType`, `domain`, and `message` fields, and it checks that Permit2 requests point to the official Permit2 contract.

Any amount at or above 2¹²⁸ is treated as effectively unlimited, since no legitimate dApp needs an allowance that large.

Everything happens inside your browser with standard JavaScript. Nothing is uploaded, logged, or stored.

## 🚀 Usage

**Online:** open the [live version](https://astapyth.github.io/walletguard-inspector/index.html).

**Offline:** download `index.html` and open it in any modern browser. No installation needed.

**Accepted inputs:**

- Raw calldata: `0x095ea7b3...`
- A transaction object: `{"to": "0x...", "value": "0x0", "data": "0x..."}`
- EIP-712 typed data: `{"types": {...}, "primaryType": "...", "domain": {...}, "message": {...}}`
- A wallet request: `{"method": "eth_signTypedData_v4", "params": [...]}`
- A 32-byte hash (flagged as a blind `eth_sign`)
- Plain text (treated as a message signature)

Click **Load example** to cycle through three realistic drainer scenarios.

## 🔐 Stay safe

Even the best tool is a second opinion, not a guarantee. A few habits protect you more than any software:

1. Check the website URL letter by letter before connecting your wallet.
2. Never approve unlimited allowances to addresses you don't recognize.
3. Look up unknown spender addresses on a block explorer.
4. Review and revoke old approvals regularly.
5. No legitimate team or support agent will ever ask for your seed phrase.

## ❓ FAQ

**Does it connect to my wallet?**
No. You copy the request in, and nothing is ever connected or signed.

**Can it guarantee a transaction is safe?**
No. It recognizes known drainer patterns. A "no known pattern" result means nothing dangerous was matched, not that the contract is trustworthy. Always verify the site and the recipient.

**Which chains are supported?**
Any EVM-compatible chain (Ethereum, BNB Chain, Polygon, Arbitrum, Base, and others), since they share the same calldata and EIP-712 formats.

**Why is it a single file?**
So anyone can audit it in a few minutes. In wallet security, code you can read is code you can trust.

## 🤝 Contributing

Issues and pull requests are welcome, especially new drainer patterns, function selectors, and test cases. Please include a sample payload with every new detection rule.

## ⚠️ Disclaimer

This tool is provided "as is" for educational and defensive purposes, without any warranty. It does not replace your own judgment. The authors are not responsible for any loss of funds.

## 📄 License

MIT. Free to use, modify, and share.

---

## 🧰 Want more protection?

WalletGuard Inspector is the free, open-source part of the project.

💬 **For more, visit:** [Telegram](https://t.me/C_ryptotoolsbot)

For the full multi-chain toolkit, see:
👉 **[WalletGuard Anti-Drainer Protection Toolkit](https://github.com/Astapyth/WalletGuard---Anti-Drainer-Protection-Toolkit)**

⭐ If this tool helped you, star the repo so more people can find it.
