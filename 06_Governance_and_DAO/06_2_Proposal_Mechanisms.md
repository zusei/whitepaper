# 06.2 Proposal Mechanisms: The Algorithmic Democracy

**Focus:** On-Chain Parameter Tuning  
**Key Mechanism:** Zusei Improvement Proposals (ZIPs)

---

## The Philosophy: Governance as Code

In the Zusei protocol, governance is not a suggestion box. It is the control panel for a living economic machine. When the community votes to change a parameter, they are interacting directly with the smart contracts that manage the **Burn Rates** and **Energy Zones.**

All changes follow the strict **ZIP (Zusei Improvement Proposal)** lifecycle:
1.  **Signal:** Forum discussion to gauge sentiment.
2.  **Draft:** Technical specification with code diffs.
3.  **Vote:** On-chain voting via the Governance Portal.
4.  **Execute:** Automatic implementation via Timelock Controller (48-hour delay).

---

## Mechanism 1: Adjusting the Burn Rate

The most critical economic lever in Zusei is the **Burn-to-Mint Ratio**.
* **Scenario:** If $ZST price skyrockets, the entry cost for new merchants might become too high.
* **The Fix:** The community votes to adjust the "Oracle Sensitivity" to lower the burn requirement.

### The Adjustment Formula
We utilize a **Bounded Adjustment Logic** to prevent volatility shocks. The community cannot change the rate by more than ±10% in a single epoch.

$$R_{new} = R_{current} \times (1 + \Delta_{vote})$$

Where:
* $R_{current}$: The current tokens burned per 1 ZP.
* $\Delta_{vote}$: The net weighted vote outcome (normalized between -0.10 and +0.10).

**The Logic:**
If 80% of the voting power signals "Lower Costs," the $\Delta_{vote}$ shifts negative (e.g., -0.05).
$$R_{new} = 100 \times (1 - 0.05) = 95 \text{ Tokens}$$

This ensures the economy steers smoothly, rather than jerking violently based on hype.

---

## Mechanism 2: Activating New "Energy Zones"

Zusei does not launch everywhere at once. New districts (e.g., "Jurong East" or "Changi Business Park") must be **Activated** by governance.

### The Density Threshold Formula
A zone is only eligible for activation (and ZP rewards) if it meets the **Minimum Viable Density (MVD)**. This prevents "ghost zones" where users have nowhere to spend their credits.

$$D_{zone} = \frac{\sum (N_{active} \times U_{uptime})}{Area_{km^2}}$$

* $N_{active}$: Number of registered Merchant Nodes in the zone.
* $U_{uptime}$: Average verified uptime (0.0 - 1.0) of those nodes.
* $Area_{km^2}$: The geographic size of the zone.

**The Voting Trigger:**
If $D_{zone} > T_{threshold}$ (e.g., 50 Nodes/km²), a **Zone Activation ZIP** is automatically drafted.
* **Vote YES:** The zone goes live. Rewards flow.
* **Vote NO:** The zone remains in "Testnet Mode" until quality improves.

---

## Mechanism 3: Voting Power Calculation (Time-Weighted)

To prevent "Flash Loan Attacks" (where an attacker borrows millions of tokens just to vote), Zusei uses a **Time-Weighted Voting** formula.

Your voice is not just defined by *how much* you have, but *how long* you are committed.

$$V_{power} = S_{tokens} \times \sqrt{T_{lock}}$$

* $S_{tokens}$: Amount of $ZST staked.
* $T_{lock}$: Duration of the lock (in weeks).

**Example:**
* **Speculator Steve:** Stakes 10,000 Tokens for 1 week.
    * $V = 10,000 \times 1 = 10,000$ Votes.
* **Merchant Mary:** Stakes 2,000 Tokens for 52 weeks (1 Year).
    * $V = 2,000 \times 7.21 = 14,420$ Votes.

**Result:** Mary (the long-term believer) has **44% more power** than Steve (the rich speculator), despite having 80% less capital. This mathematically aligns governance with long-term network health.

---

## Conclusion

By reducing governance to mathematical formulas, we remove emotion from the equation. We don't argue about "feelings"; we vote on **variables**. This makes Zusei adaptable, resilient, and famously hard to kill.
