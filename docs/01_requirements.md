# Requirements: Trading Account Anomaly Detection

## Problem
Trading platforms face three abuse patterns:
- **Trial abuse:** brand-new accounts making a huge number of trades to exhaust free limits.
- **Multi-accounting:** several accounts sharing a device or IP, often run by one person or a fraud ring.
- **Bot activity:** accounts trading with machine-like, very regular timing.

These cost the platform money and distort metrics, and manual review of every account is impossible.

## User
A risk or support analyst who reviews flagged accounts and decides what to do.
Humans make the final call. The system never bans or restricts accounts automatically.

## Input
One row of account features:
account age (days), trades per day, baseline trades per day,
linked account count, timing regularity.

## Output
- `suspicion_score` (higher = more suspicious)
- `flagged` (yes/no)
- `explanation` (one plain-English sentence)

## Scoring modes
1. **Daily batch:** score all accounts and flag the top 3% by score.
2. **On-demand API:** score a single account via `POST /score`, flagged against a
   saved score cutoff from the training data (the 3% budget only makes sense
   across many accounts at once).

## Alert budget
Analysts can review about 3% of accounts per day, so the system flags the
top 3% by score instead of using a fixed threshold. No contamination rate
is set from the true anomaly rate.

## Success metrics
- Precision and recall at the 3% budget
- Recall per abuse type (trial abuse, multi-account, bot)
- Recall for obvious vs. subtle anomalies
- All averaged over 5 random seeds, reported with spread

## Non-goals
- Automatic account bans
- Real-time streaming
- Cloud deployment
- Language-model-generated explanations
- Detection methods beyond the two compared (z-score, Isolation Forest)

## Important caveat
Real fraud data is not public, so all data is simulated. Results show the
pipeline works, not how it would perform on real fraud.