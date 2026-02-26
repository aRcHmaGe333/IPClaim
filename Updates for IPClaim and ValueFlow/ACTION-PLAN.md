# ACTION PLAN — APC-VF System Deployment

**What you're building:** A publicly verifiable authorship + universal-access licensing system, deployed on your existing GitHub presence, starting with the ValueFlow repo.

**What you need:** A computer with Git, OpenSSL, and a browser. All free.

---

## PHASE 1: Harden the ValueFlow Repo (Day 1)

This is your existing repo. It already has prior art disclosure from August 2025. It needs cryptographic proof and the new license.

### Step 1: Clone your repo locally

```bash
git clone https://github.com/archmage333/ValueFlow.git
cd ValueFlow
```

### Step 2: Replace the license

Delete or rename the old implicit all-rights-reserved status. Drop in the new one:

- Copy `LICENSE-APC-VF.md` into the repo root as `LICENSE.md`
- Fill in: `Slavko Stojnić`, your chosen revenue split (e.g., 30/70, 50/50, whatever you want), and the current date
- The hash fields will be filled after the first timestamp run

### Step 3: Add VERIFY.md

Copy `VERIFY.md` into the repo root. No edits needed — it's generic by design.

### Step 4: Add the GitHub Actions workflow

```bash
mkdir -p .github/workflows
cp timestamp.yml .github/workflows/timestamp.yml
```

### Step 5: Commit and push

```bash
git add .
git commit -m "APC-VF License v2.0 + automated cryptographic timestamping"
git push origin main
```

**What happens next:** GitHub Actions fires automatically. Within ~60 seconds, a `.timestamps/` folder appears in your repo with an RFC 3161 `.tsr` file. Your repo is now cryptographically timestamped.

### Step 6: Update the LICENSE with actual hashes

After the Actions run completes, pull the result:

```bash
git pull
```

Now get your tree hash:

```bash
git rev-parse HEAD^{tree}
```

Copy that hash into the LICENSE proof table. Commit again. The next Actions run timestamps this updated state too. The chain is now self-reinforcing.

**Phase 1 outcome:** ValueFlow is hardened with cryptographic proof, a real license, independent verifiability, and automated future timestamping. Every subsequent push is automatically proven.

---

## PHASE 2: Timestamp Your Backlog (Day 1–2)

Your original commits from August 2025 are already in Git history. Their tree hashes exist. You can retroactively timestamp them to create proof anchored to the *current* date that those exact contents were committed at those points.

### For each important historical commit:

```bash
# Check out the old commit's tree hash
TREE_HASH=$(git rev-parse <COMMIT_SHA>^{tree})

# Timestamp it now
echo -n "$TREE_HASH" > /tmp/tree_hash.txt
openssl ts -query -data /tmp/tree_hash.txt -no_nonce -sha512 -cert -out /tmp/request.tsq
curl -s -H "Content-Type: application/timestamp-query" \
     --data-binary '@/tmp/request.tsq' \
     https://freetsa.org/tsr \
     -o .timestamps/${TREE_HASH}.tsr
```

This proves: "the content that existed at commit X has this tree hash, and I timestamped that hash on [today]." It doesn't retroactively prove the August date, but it creates an additional layer of cryptographic evidence linking those file contents to a verified moment.

For the August date itself, your Git history + GitHub's server logs are the evidence. The RFC 3161 timestamp adds mathematical proof of content integrity on top.

**Phase 2 outcome:** Historical commits are cryptographically anchored.

---

## PHASE 3: Create the Stamped Service MVP (Week 1–2)

This is the web service where anyone (including you) can submit files and get back a timestamped, licensed bundle.

### Tech stack (minimal, free, no dependencies on anything fragile):

| Component | Tool | Why |
|---|---|---|
| Backend | Python + Flask | Simple, you can run it locally or deploy it |
| Hashing | Python `hashlib` (SHA-512) | Built-in, no dependencies |
| Merkle tree | ~30 lines of Python | Pair-hash files into a single root |
| RFC 3161 calls | `subprocess` calling `openssl` + `curl` | Uses FreeTSA, same as the manual process |
| License generation | String template | Fills in hashes, dates, author name |
| Output | ZIP file | Contains originals + proofs + license + VERIFY.md |
| Frontend | Single HTML page with a file upload form | No framework needed |

### Build order:

**Step 1:** Write the Merkle tree function.
- Input: list of file paths
- Output: single SHA-512 root hash
- ~30 lines of Python

**Step 2:** Write the timestamp function.
- Input: root hash string
- Output: `.tsr` file path
- Calls `openssl ts` and `curl` via subprocess

**Step 3:** Write the license generator.
- Input: author name, root hash, timestamp date
- Output: populated `LICENSE-APC-VF.md` text

**Step 4:** Write the bundler.
- Input: original files + license + .tsr + VERIFY.md
- Output: ZIP archive

**Step 5:** Write the Flask route.
- `GET /` → upload form with consent text ("By submitting, you consent to timestamping and licensing under APC-VF")
- `POST /` → process files → return ZIP

**Step 6:** Test locally.
```bash
python app.py
# Visit http://127.0.0.1:5000
# Upload files, download bundle, verify with OpenSSL
```

**Phase 3 outcome:** Working local service. Submit files → get back a timestamped, licensed, verifiable bundle.

---

## PHASE 4: Deploy (Week 2–3)

### Option A: Simple (Recommended to start)
Deploy the Flask app on a basic cloud instance or PaaS:
- **Railway.app** — free tier, Git push to deploy
- **Render.com** — free tier, auto-deploys from GitHub
- **Your own VPS** — full control, ~$5/month

### Option B: Static + API
- Host the frontend as a static page (GitHub Pages, Netlify — free)
- Backend as a serverless function (Cloudflare Workers, Vercel Functions)
- Keeps costs at $0

### Domain
Register something like `stamped.dev` or `proofstamp.org`. Point it at your deployment.

**Phase 4 outcome:** Public service. Anyone can use it.

---

## PHASE 5: Apply to Your Own Work (Ongoing)

For every piece of work you create going forward:

1. Create a Git repo
2. Add `LICENSE-APC-VF.md`, `VERIFY.md`, and the timestamp workflow
3. Push
4. The system handles the rest automatically

For non-Git work (PDFs, images, designs, etc.):
- Use the Stamped service (or the manual `openssl` + `curl` process)
- Store the bundle somewhere public (your website, a file host, anywhere)

---

## PHASE 6: Revenue Infrastructure (Month 1–2)

The license says revenue must be shared. Now you need a way to actually receive it.

### Minimum viable setup:
- A **Stripe account** (or PayPal, or whatever you prefer)
- A public page listing your APC-VF licensed works with:
  - Link to each repo/work
  - The stated revenue split
  - A "Pay the creator" button or invoice request form
  - Contact info for commercial licensing inquiries

### Better setup (later):
- A simple dashboard that lets users of your work self-report revenue and calculate their share
- Stripe Connect integration so payment splits happen automatically at the transaction level
- This is the actual ValueFlow protocol in action — start simple, automate later

**Phase 6 outcome:** You can actually receive money when people use your work.

---

## SUMMARY TABLE

| Phase | What | Time | Cost |
|---|---|---|---|
| 1. Harden ValueFlow | License + timestamps + automation on existing repo | 1 hour | $0 |
| 2. Timestamp backlog | Anchor historical commits | 30 min | $0 |
| 3. Build Stamped MVP | Flask app, local | 1–2 weeks | $0 |
| 4. Deploy | Put it online | 1 day | $0–5/mo |
| 5. Apply to your work | Use the system for everything you create | Ongoing | $0 |
| 6. Revenue infra | Stripe/PayPal + public listing | 1–2 days setup | $0 until revenue flows |

---

## WHAT YOU HAVE AT THE END

- Every piece of your work is cryptographically timestamped and publicly claimed
- Anyone can use it — under the condition that they share revenue
- No one can patent it — your prior art kills their application
- No one can deny you made it — the math proves it
- No one needs to approve, examine, or maintain it — it runs itself
- The service exists for others to do the same thing
- Revenue flows back to you automatically or by agreement

The philosophy is done. This is the wiring.
