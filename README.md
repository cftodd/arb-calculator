# Betting Calculators

A single-page, no-dependency HTML tool for calculating sports betting math. Everything runs client-side in the browser — no server, no build step, no external libraries. Open https://cftodd.github.io/arb-calculator/

The page has three tabs: **Wager Calculator**, **Arbitrage Calculator**, and **Odds Converter**.

---

## Wager Calculator

Tracks up to 10 individual bets at once and rolls them up into totals.

**Inputs (per row):**
- Stake ($)
- American odds (e.g. `+2000`, `-150`)

**Outputs:**
- Payout per bet
- **Total stake** — sum of all stakes entered
- **Total payout** — sum of all individual payouts
- **Total profit** — total payout minus total stake
- **Average odds** — weighted by stake size, so larger bets influence the average more than smaller ones

**Formulas:**
- Positive odds: `profit = stake × (odds / 100)`
- Negative odds: `profit = stake × (100 / |odds|)`
- `payout = stake + profit`
- `average odds = Σ(odds × stake) / Σ(stake)`

---

## Arbitrage Calculator

Given the odds on both sides of an event (e.g. from two different sportsbooks), calculates how to split a total stake so the payout is identical regardless of which side wins — and tells you whether that split is a true arbitrage (guaranteed profit) or a guaranteed loss.

**Inputs:**
- Total stake ($)
- American odds for Bet 1
- American odds for Bet 2

**Outputs:**
- Stake and payout for each bet
- **Total payout** — the guaranteed payout, same for either outcome
- **Total profit** — total payout minus total stake
- **ROI** — profit as a percentage of total stake
- **Combined implied probability** — sum of both bets' implied win probabilities
- A status message: arbitrage found (profit locked in) or no arbitrage (this split guarantees a loss)

**Formulas:**
- Decimal odds: positive → `1 + (odds / 100)`, negative → `1 + (100 / |odds|)`
- Implied probability: `1 / decimal odds`
- Stake per bet: `total stake × (implied probability / combined implied probability)`
- Combined implied probability under 100% = arbitrage opportunity; over 100% = guaranteed loss

---

## Odds Converter

Converts between the three common ways of expressing betting odds. Type a value into any one field and the other two update automatically.

**Fields:**
- American odds (e.g. `+150`, `-110`)
- Decimal odds (e.g. `2.50`)
- Implied probability (as a %, e.g. `40`)

**Formulas:**
- Decimal → American: if decimal ≥ 2, `american = (decimal − 1) × 100`; otherwise `american = −100 / (decimal − 1)`
- American → Decimal: positive odds → `1 + odds/100`; negative odds → `1 + 100/|odds|`
- Decimal → Implied probability: `1 / decimal odds` (shown as a %)
- Implied probability → Decimal: `100 / probability`
- Implied probability → American: if probability > 50%, `−(prob / (100 − prob)) × 100`; otherwise `((100 − prob) / prob) × 100`

---

## Tech

Plain HTML, CSS, and vanilla JavaScript — no frameworks, no build tools, no dependencies. Everything recalculates live as you type. Nothing is saved between sessions; refreshing the page clears all fields.
