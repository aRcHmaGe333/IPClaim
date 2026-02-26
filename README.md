# IPClaim

**Intellectual Property Claim for the Post-Patent World**

In today's world, where creative ownership is vital, IPClaim is transforming how authorship is established. By emphasizing evidence, time, and transparency, it sets a new standard for creative ownership.

## A New Way to Claim What's Yours

IPClaim replaces traditional paperwork and gatekeeping with cryptographic certainty. Your work stands as its own testament — timestamped, verifiable, and enduring.

- Proof embedded in your code
- Authorship verifiable by anyone, anywhere
- Ownership based on evidence

One system is based on evidence. The other is based on permission.

---

## Stamp a Repo (Automated)

The fastest way to claim a repository. From anywhere:

```bash
/path/to/IPClaim/stamp.sh /path/to/your-repo "Your Name" [apc|apc-vf]
```

Or run it with prompts:

```bash
/path/to/IPClaim/stamp.sh /path/to/your-repo
```

This single command:

1. Copies the timestamp workflow, verification instructions, and your chosen license into the target repo
2. Fills in your author name, tree hash, and date automatically
3. Commits and (optionally) pushes — triggering the timestamp workflow on GitHub

Prerequisites: the target must be a git repo with at least one commit and a GitHub remote. If you can `git push`, the rest works.

---

## Manual Setup (Fallback)

If `stamp.sh` doesn't work for your setup, you can perform the same steps by hand.

### 1. Add a License

Select from two license options:

- `LICENSE-APC.md` — All rights reserved, with authorship and patent claim
- `LICENSE-APC-VF.md` — Same, with ValueFlow profit sharing included

Copy your chosen license into your repository and fill in the placeholders: author name, tree hash (`git rev-parse HEAD^{tree}`), and today's date.

### 2. Enable Timestamping

Copy `.github/workflows/timestamp.yml` into your repository. Each push to main or master will:

- Compute your repo's tree hash (SHA-512 fingerprint of every file)
- Timestamp it via FreeTSA.org (RFC 3161)
- Save the `.tsr` proof in `.timestamps/`
- Commit and push the evidence

### 3. Add Verification Instructions

Copy `VERIFY.md` into your repository. It contains step-by-step instructions for anyone to independently verify your claim using OpenSSL — no accounts, no fees, no intermediaries.

---

## How It Works

The process is straightforward and open:

1. **Commit your work** — push your creation to GitHub.
2. **Fingerprint it** — Git generates a unique hash of your project.
3. **Seal it in time** — the hash is timestamped via FreeTSA.org.
4. **Preserve the proof** — the `.tsr` token is stored in `.timestamps/` and committed.
5. **Verify anytime** — confirm the timestamp and integrity with free tools.

Everything happens within your repository. No external service holds your proof.

---

## Contents

| File | Purpose |
|---|---|
| `stamp.sh` | Automate the full IPClaim setup on any repo |
| `LICENSE-APC.md` | All-rights-reserved license template |
| `LICENSE-APC-VF.md` | License with ValueFlow profit sharing |
| `VERIFY.md` | Independent verification instructions |
| `.github/workflows/timestamp.yml` | Drop-in workflow for automatic timestamping |
| `STAMPED-whitepaper.md` | The full case for why this works |

---

## Why It Matters

IPClaim empowers creators by prioritizing evidence over authority. It establishes a public, unalterable record. Each commit is a declaration of authorship, supported by cryptography and time. For creators, this means gaining the recognition and protection they deserve — without asking permission from anyone.
