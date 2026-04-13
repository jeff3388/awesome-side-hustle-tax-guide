# Contributing to Awesome Side Hustle Tax Guide

Thank you for contributing! Freelancers and gig workers are often the most under-served group when it comes to clear, accurate tax guidance — your contributions directly help millions of people navigate a complex system.

---

## What We Welcome

- **1099-K threshold updates** — The $600 threshold was finalized for 2025 tax year. Any IRS announcements that modify this should be updated immediately with a link to the official IRS notice.
- **Platform-specific guides** — Tax treatment varies across Uber, DoorDash, Etsy, eBay, Airbnb, YouTube, etc. Accurate, detailed platform guides from real practitioners or official IRS guidance are highly valued.
- **Standard mileage rate updates** — IRS announces the business mileage rate each January. Annual updates are always welcome.
- **State-level self-employment resources** — Official state tax authority guidance on SE income, local taxes, and occupational taxes.
- **New deduction categories** — As the gig economy evolves, new deductible expense types emerge. Add them with an IRS citation.
- **SE tax formula corrections** — If any formula produces incorrect results, open an issue immediately with the correct computation and IRS Schedule SE citation.
- **S-Corp and business structure updates** — If IRS changes S-Corp filing thresholds or rules, update accordingly.
- **Retirement contribution limit updates** — IRS announces new 401(k)/SEP-IRA limits each October.

## What We Don't Accept

- Advice encouraging misclassification of income (e.g., calling taxable income a "gift")
- Resources that suggest hiding income from the IRS
- Platform-specific content that is promotional rather than educational
- Financial projections or predictions about future tax rates

---

## How to Contribute

1. Fork this repository
2. Create a descriptive branch: `git checkout -b update/2026-mileage-rate`
3. Make your changes following the style guide
4. Verify all IRS links point to current official pages
5. Submit a Pull Request with what changed and why

---

## Style Guide

### Adding Platform Tax Guidance

Use this structure for each platform section:

```markdown
### Platform Name (Uber / DoorDash / Etsy / etc.)

Income: What forms you'll receive and threshold.
Key deductions: Bulleted list specific to this platform.
Common mistake: One common error specific to this platform.
```

### Code Accuracy

All tax calculation code must:
- Include tax year in the function docstring
- Cite the specific IRS form/publication used
- Include at least one example with expected output
- Note FICA wage base (changes annually)

### Mileage Rate Updates

When the IRS announces the new standard mileage rate (January each year):
1. Update the rate in the Vehicle Deduction section
2. Update the year reference
3. Link to the IRS announcement (IRS Notice YYYY-XX)

---

## Accuracy Standards

All numerical data (SE tax rates, FICA limits, safe harbor rules) must cite:
- IRS forms (Schedule SE, Schedule C, Form 1040-ES)
- IRS publications (Pub 334, Pub 535, Pub 587)
- IRS announcements (IR-YYYY-XXX, Notice YYYY-XX)

If a rule is unsettled or contested (e.g., Airbnb substantial services test), mark it clearly as "IRS position" vs "Tax court guidance pending."

---

## Questions?

Open an issue with the `question` label. For platform-specific edge cases, note the platform and describe the specific transaction type.
