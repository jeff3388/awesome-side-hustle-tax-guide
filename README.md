# Awesome Side Hustle Tax Guide [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> A curated list of authoritative resources, formulas, deduction checklists, and open-source tools for self-employed individuals, freelancers, gig workers, and side hustlers navigating US taxes — covering 1099-K, SE tax, quarterly payments, deductions, and platform-specific guidance.

**Last Updated:** April 2026 | Tax Year 2025–2026

> **Disclaimer:** All resources are for educational and informational purposes only. Tax situations vary by individual. Nothing here constitutes tax, legal, or financial advice. Consult a qualified CPA or tax professional for your specific situation.

---

## Contents

- [Official IRS Resources for Self-Employed](#official-irs-resources-for-self-employed)
- [Who Must File Self-Employment Tax](#who-must-file-self-employment-tax)
- [SE Tax Calculation Formulas](#se-tax-calculation-formulas)
- [Quarterly Estimated Tax](#quarterly-estimated-tax)
- [1099-K & Platform Income Reporting](#1099-k--platform-income-reporting)
- [Deductible Business Expenses](#deductible-business-expenses)
- [Home Office Deduction](#home-office-deduction)
- [Vehicle Deduction](#vehicle-deduction)
- [Gig Platform Tax Guides](#gig-platform-tax-guides)
- [Retirement Accounts for Self-Employed](#retirement-accounts-for-self-employed)
- [Health Insurance Deduction](#health-insurance-deduction)
- [Business Structure Considerations](#business-structure-considerations)
- [Open-Source Tools & Libraries](#open-source-tools--libraries)
- [State-Level Self-Employment Tax](#state-level-self-employment-tax)
- [Learning Resources](#learning-resources)

---

## Official IRS Resources for Self-Employed

- [IRS Self-Employed Tax Center](https://www.irs.gov/businesses/small-businesses-self-employed/self-employed-individuals-tax-center) — Official hub for freelancers and self-employed.
- [IRS Schedule C Instructions](https://www.irs.gov/pub/irs-pdf/i1040sc.pdf) — Profit or Loss from Business — the core self-employment form.
- [IRS Schedule SE Instructions](https://www.irs.gov/pub/irs-pdf/i1040sse.pdf) — Self-Employment Tax calculation.
- [IRS Form 1040-ES](https://www.irs.gov/pub/irs-pdf/f1040es.pdf) — Estimated Tax for Individuals (quarterly payments).
- [IRS Publication 334](https://www.irs.gov/publications/p334) — Tax Guide for Small Business.
- [IRS Publication 535](https://www.irs.gov/publications/p535) — Business Expenses (comprehensive deduction guide).
- [IRS Publication 587](https://www.irs.gov/publications/p587) — Business Use of Your Home.
- [IRS Publication 463](https://www.irs.gov/publications/p463) — Travel, Gift, and Car Expenses.
- [IRS Topic 554: Self-Employment Tax](https://www.irs.gov/taxtopics/tc554) — Concise overview.

---

## Who Must File Self-Employment Tax

```
You owe SE tax if:
  Net self-employment income ≥ $400 in a tax year

This includes:
  ✓ Freelance / contract income (1099-NEC)
  ✓ Gig platform income (Uber, DoorDash, Instacart, Fiverr, Upwork)
  ✓ Online selling (eBay, Etsy, Amazon) if operated as a business
  ✓ Rental income where you provide substantial services (e.g., Airbnb + cleaning)
  ✓ Crypto mining (treated as self-employment if business activity)
  ✓ Content creator / influencer ad revenue
  ✓ Side jobs paid in cash (legally required to report)
  ✓ Tips (must report all tips regardless of whether employer tracks them)

Does NOT include (generally):
  ✗ W-2 wages (employer handles FICA)
  ✗ Passive rental income (no substantial services)
  ✗ Investment gains
  ✗ Hobby income — BUT IRS scrutinizes "hobby vs. business" distinction
```

### Hobby vs. Business (IRS 9-Factor Test)

The IRS evaluates whether an activity is a for-profit business using these factors:

1. Does the activity make a profit in 3 of 5 consecutive years?
2. Does the taxpayer depend on income from it?
3. Does the time and effort suggest a profit motive?
4. Do assets appreciate in value?
5. Has the taxpayer succeeded in similar activities before?
6. Is the history of income/losses explainable by startup/unusual events?
7. Is the profit occasionally large enough to indicate profit motive?
8. Is the financial status of the taxpayer relevant? (Wealthy hobbyists get more scrutiny)
9. Are there elements of personal pleasure or recreation involved?

**Consequence:** Hobby losses cannot offset other income; business losses can.

---

## SE Tax Calculation Formulas

### Complete SE Tax Calculation (2026)

```python
def calculate_se_tax(net_self_employment_income: float) -> dict:
    """
    Calculate self-employment tax for 2026.
    Source: IRS Schedule SE, IRS Notice on FICA limits

    Args:
        net_self_employment_income: Gross self-employment income minus business expenses

    Returns:
        Dict with SE tax, deductible portion, and effective rate
    """
    SS_WAGE_BASE = 176_100       # 2026 Social Security wage base
    SS_RATE = 0.124              # Combined employee + employer rate
    MEDICARE_RATE = 0.029        # Combined employee + employer rate
    SE_ADJUSTMENT = 0.9235       # IRS allows 7.65% deduction of SE income

    se_tax_base = net_self_employment_income * SE_ADJUSTMENT

    # Social Security portion (capped at wage base)
    ss_taxable = min(se_tax_base, SS_WAGE_BASE)
    ss_tax = ss_taxable * SS_RATE

    # Medicare portion (no cap)
    medicare_tax = se_tax_base * MEDICARE_RATE

    total_se_tax = ss_tax + medicare_tax
    deductible_half = total_se_tax * 0.5  # Deducted from AGI

    return {
        "net_se_income": net_self_employment_income,
        "se_tax_base": round(se_tax_base, 2),
        "ss_tax": round(ss_tax, 2),
        "medicare_tax": round(medicare_tax, 2),
        "total_se_tax": round(total_se_tax, 2),
        "deductible_half": round(deductible_half, 2),
        "effective_se_rate": round(total_se_tax / net_self_employment_income * 100, 2),
    }

# Examples
print(calculate_se_tax(50_000))
# total_se_tax: $7,065, deductible_half: $3,532, effective_se_rate: 14.13%

print(calculate_se_tax(200_000))
# SS caps at wage base; Medicare continues; total ~$24,200
```

### Full Tax Burden Estimate (Income + SE Tax)

```python
def total_tax_estimate(
    gross_se_income: float,
    business_expenses: float,
    filing_status: str = "single",
    other_income: float = 0
) -> dict:
    """
    Estimate total federal tax for a self-employed individual.
    Simplified — does not account for all credits/deductions.
    """
    STANDARD_DEDUCTION = 15_000  # Single, 2026

    net_se = gross_se_income - business_expenses
    se_result = calculate_se_tax(net_se)

    # AGI = net SE - deductible half of SE tax + other income
    agi = net_se - se_result["deductible_half"] + other_income

    # Taxable income
    taxable_income = max(0, agi - STANDARD_DEDUCTION)

    # Federal income tax (simplified brackets, single 2026)
    brackets = [
        (11_925, 0.10),
        (48_475, 0.12),
        (103_350, 0.22),
        (197_300, 0.24),
        (250_525, 0.32),
        (626_350, 0.35),
        (float('inf'), 0.37),
    ]
    income_tax = 0
    prev = 0
    for limit, rate in brackets:
        if taxable_income <= prev:
            break
        income_tax += (min(taxable_income, limit) - prev) * rate
        prev = limit

    total_tax = income_tax + se_result["total_se_tax"]

    return {
        "gross_income": gross_se_income,
        "business_expenses": business_expenses,
        "net_se_income": net_se,
        "se_tax": se_result["total_se_tax"],
        "agi": round(agi, 2),
        "taxable_income": round(taxable_income, 2),
        "federal_income_tax": round(income_tax, 2),
        "total_federal_tax": round(total_tax, 2),
        "effective_total_rate": round(total_tax / gross_se_income * 100, 2),
    }

# Example: $80k gross, $15k expenses
result = total_tax_estimate(80_000, 15_000)
print(result)
# total_federal_tax: ~$15,400, effective_total_rate: ~19.3%
```

> For an interactive self-employed tax calculator that handles all 50 state taxes as well, [TaxLogic.cc's Self-Employed Tax Calculator](https://taxlogic.cc/tools/tax/self-employed-tax-calculator) provides detailed breakdowns including SE tax, federal, and state income tax.

---

## Quarterly Estimated Tax

### 2026 Due Dates

| Quarter | Income Period | Due Date |
|---|---|---|
| Q1 | January 1 – March 31 | **April 15, 2026** |
| Q2 | April 1 – May 31 | **June 16, 2026** |
| Q3 | June 1 – August 31 | **September 15, 2026** |
| Q4 | September 1 – December 31 | **January 15, 2027** |

### Safe Harbor Rules (Avoid Underpayment Penalty)

```
Pay the LESSER of:
  Option A: 90% of current year's actual tax liability
  Option B: 100% of prior year's tax liability
             (110% if prior year AGI > $150,000)

Safe Harbor Example:
  2025 total tax: $12,000
  2026 estimated quarterly payment: $12,000 / 4 = $3,000/quarter
  → No underpayment penalty regardless of 2026 actual tax
```

### Quarterly Payment Calculation

```python
def quarterly_payment(
    estimated_annual_tax: float,
    prior_year_tax: float,
    prior_year_agi: float
) -> float:
    """
    Calculate safe harbor quarterly estimated tax payment.
    """
    # Safe harbor from prior year
    multiplier = 1.10 if prior_year_agi > 150_000 else 1.00
    prior_year_safe_harbor = prior_year_tax * multiplier

    # Safe harbor from current year estimate
    current_year_safe_harbor = estimated_annual_tax * 0.90

    # Pay the lesser
    annual_required = min(prior_year_safe_harbor, current_year_safe_harbor)
    return round(annual_required / 4, 2)
```

### IRS Payment Methods

- [IRS Direct Pay](https://www.irs.gov/payments/direct-pay) — Free ACH payment, no registration required.
- [IRS EFTPS](https://www.eftps.gov/) — Electronic Federal Tax Payment System (must enroll; allows scheduling).
- [IRS2Go App](https://www.irs.gov/newsroom/irs2goapp) — Mobile payment option.
- Mail: Check payable to "United States Treasury" with Form 1040-ES voucher.

---

## 1099-K & Platform Income Reporting

### 2026 1099-K Threshold

```
Reporting threshold (effective tax year 2025, filed in 2026):
  $600 total payments processed through a single platform
  (reduced from prior $20,000 / 200 transactions threshold)

Platforms that send 1099-K:
  PayPal, Venmo (business transactions), Cash App, Stripe,
  Etsy, eBay, Amazon Marketplace, Airbnb, VRBO, Rover, TaskRabbit

Does NOT generate 1099-K:
  Personal payments between friends ("Dinner split" on Venmo)
  However, platforms increasingly ask users to classify transactions
```

### 1099-NEC vs 1099-K vs 1099-MISC

| Form | Issued By | Threshold | Reports |
|---|---|---|---|
| 1099-NEC | Clients/businesses you worked for | $600 | Non-employee compensation |
| 1099-K | Payment processors / platforms | $600 (2025+) | Gross payment volume |
| 1099-MISC | Various | $600 | Rent, prizes, other income |

**Critical:** 1099-K reports **gross** payments, not profit. You must subtract your cost of goods sold and expenses when reporting on Schedule C.

```
eBay Example:
  1099-K shows: $8,000 (gross sales)
  Your Schedule C:
    Gross receipts: $8,000
    Cost of goods sold: -$5,500
    eBay fees: -$800
    Shipping: -$300
    Net profit: $1,400  ← This is what you're taxed on
```

---

## Deductible Business Expenses

### Common Self-Employment Deductions (Schedule C)

| Expense | Notes |
|---|---|
| Advertising / Marketing | Social media ads, website costs, business cards |
| Bank Fees | Business account fees, merchant processing fees |
| Car & Truck | Standard mileage (67¢/mile, 2025) or actual expenses |
| Commissions Paid | Payments to subcontractors |
| Contract Labor | Other freelancers you hire (issue 1099-NEC if ≥ $600) |
| Depreciation | Equipment, computers (Section 179 allows immediate expensing) |
| Home Office | Simplified ($5/sq ft, max 300 sq ft) or actual expenses |
| Insurance | Business liability insurance, E&O insurance |
| Legal & Professional | CPA, attorney fees related to business |
| Meals | 50% deductible if business purpose, properly documented |
| Office Supplies | Postage, printer ink, stationery |
| Phone & Internet | Business-use percentage of your bill |
| Rent | Co-working space, studio, storage unit |
| Software & Subscriptions | Adobe, Slack, Zoom, accounting software |
| Tools & Equipment | Professional tools relevant to your trade |
| Travel | Airfare, hotel, 50% meals when traveling for business |
| Education | Courses/training to maintain or improve current skills |

### Section 179 Expensing (2026)

```
Instead of depreciating equipment over multiple years, Section 179 lets you
deduct the full cost in year of purchase.

2026 Limit: $1,220,000 (indexed for inflation)
Phase-out begins: $3,050,000 of total equipment placed in service

Eligible: Computers, phones, cameras, vehicles (limits apply), furniture,
          off-the-shelf software

Not eligible: Real estate, inventory, items used outside the US
```

### Standard Mileage Rate 2025–2026

```
Business use: 67 cents per mile (2025)
              Check IRS Notice in January 2026 for updated rate

Medical/moving: 21 cents per mile (2025)
Charitable: 14 cents per mile (fixed by statute)

Mileage log requirements:
  - Date of each trip
  - Business destination
  - Business purpose
  - Miles driven
  Recommend: MileIQ, Everlance, or manual logbook
```

---

## Home Office Deduction

### Two Methods

**Simplified Method:**
```
Deduction = $5 × square feet used exclusively for business
Maximum: 300 sq ft → max deduction = $1,500/year
No depreciation recapture risk on home sale
```

**Actual Expense Method:**
```
Business Use % = Home Office Area ÷ Total Home Area

Deductible = Business Use % × (Rent or Mortgage Interest + Utilities
             + Insurance + Repairs + Depreciation)

Depreciation = (Home Cost - Land Value) / 39 years × Business Use %
WARNING: Depreciation must be recaptured at sale (up to 25% rate)
```

### Exclusive Use Requirement

```
The home office space must be used:
  ✓ Regularly (not just occasionally)
  ✓ Exclusively (no personal use — even a guest bed disqualifies the room)

Exception: Daycare providers and storage use have relaxed rules.
```

---

## Vehicle Deduction

### Standard Mileage vs Actual Expenses

```python
def compare_vehicle_methods(
    business_miles: float,
    total_miles: float,
    vehicle_cost: float,
    actual_expenses: float,  # Gas, insurance, maintenance, registration
    mileage_rate: float = 0.67  # 2025 IRS standard mileage rate
) -> dict:
    """Compare Standard Mileage vs Actual Expense Method."""

    # Standard mileage
    standard_deduction = business_miles * mileage_rate

    # Actual expense method
    business_pct = business_miles / total_miles
    depreciation = vehicle_cost / 5 * business_pct  # Simplified 5-year MACRS
    actual_deduction = actual_expenses * business_pct + depreciation

    return {
        "business_percentage": round(business_pct * 100, 1),
        "standard_mileage_deduction": round(standard_deduction, 2),
        "actual_expense_deduction": round(actual_deduction, 2),
        "recommended": "Standard" if standard_deduction > actual_deduction else "Actual",
    }

# Example: Uber driver, 25k business miles of 30k total, $30k vehicle
result = compare_vehicle_methods(25_000, 30_000, 30_000, 8_000)
print(result)
```

---

## Gig Platform Tax Guides

### Rideshare (Uber / Lyft)

```
Income: Fares + surge + tips — reported on 1099-K (if ≥ $600) and 1099-NEC (referrals)
Key deductions:
  - Mileage (standard rate recommended — simplest for rideshare)
  - Phone mount, phone charger for passengers
  - Car washes (if maintaining vehicle for rideshare)
  - Tolls and parking (business trips)
  - Portion of phone plan

Common mistake: Mileage starts at first pickup, not from home.
Track: Miles from when you go online, not just when passengers are in car.
```

### Food Delivery (DoorDash / UberEats / Instacart)

```
Income: Deliveries + tips — reported on 1099-NEC (DoorDash) or 1099-K
Key deductions:
  - Mileage (standard rate — tracks from restaurant to drop-off)
  - Insulated delivery bags
  - Phone holder
  - Portion of phone plan

Special: Instacart shoppers may deduct grocery bags purchased for deliveries.
```

### Short-Term Rental (Airbnb / VRBO)

```
14-Day Rule:
  If rented ≤ 14 days/year: Income is tax-free (no reporting required!)
  If rented > 14 days/year: Must report all rental income

When taxable:
  Income reported on Schedule E (passive) or Schedule C (active — if substantial services)
  Substantial services = cleaning, laundry, meals → Schedule C (SE tax applies)

Key deductions:
  - Prorated mortgage interest and property taxes (rental days / 365)
  - Cleaning costs and supplies
  - Repairs (not improvements)
  - Depreciation of rental portion of home
  - Platform fees (Airbnb service fee)
  - Utilities (prorated)
  - Furniture and furnishings (Section 179 eligible)
```

### Online Selling (Etsy / eBay / Amazon)

```
Hobby vs. Business Test applies (see above)

If a business (Schedule C):
  Gross receipts = Total sales (from 1099-K or your records)
  COGS = Purchase price + shipping to you + preparation costs
  Other deductions: Platform fees, packing materials, shipping, photography equipment

Inventory tracking (required if COGS > $0):
  Beginning inventory + Purchases - Ending inventory = Cost of Goods Sold
```

### Content Creation (YouTube / TikTok / Substack)

```
Income sources (all taxable):
  - AdSense / platform ad revenue (1099-MISC or 1099-K)
  - Sponsorships (1099-NEC if from business paying ≥ $600)
  - Merchandise sales
  - Tips / Super Chats / Patreon subscriptions

Key deductions:
  - Camera, lighting, microphone, editing equipment (Section 179)
  - Editing software subscriptions
  - Music licensing fees
  - Studio space or home office
  - Props and materials used in videos
  - Travel for content creation
```

---

## Retirement Accounts for Self-Employed

### Comparison (2026)

| Account | Contribution Limit | Deadline | Best For |
|---|---|---|---|
| SEP-IRA | 25% of net SE income or $70,000 | Tax filing + extensions | High earners, simple setup |
| Solo 401(k) | $23,500 employee + 25% employer = up to $70,000 | Dec 31 (must open by year-end) | Maximum contribution flexibility |
| SIMPLE IRA | $16,500 + $3,500 catch-up | Employee by Dec 31 | Small teams (up to 100 employees) |
| Traditional IRA | $7,000 + $1,000 catch-up | April 15 next year | Supplement to above |

### SEP-IRA Calculation

```python
def sep_ira_max_contribution(net_se_income: float) -> float:
    """
    Max SEP-IRA contribution for self-employed.
    Net SE income = Gross SE - Business Expenses - (SE Tax × 0.5)
    """
    MAX_CONTRIBUTION = 70_000  # 2026
    contribution = min(net_se_income * 0.25, MAX_CONTRIBUTION)
    return round(contribution, 2)

# Example: $120,000 net SE income
print(sep_ira_max_contribution(120_000))  # $30,000

# Example: $300,000 net SE income
print(sep_ira_max_contribution(300_000))  # $70,000 (capped)
```

---

## Health Insurance Deduction

```
Self-employed individuals can deduct 100% of health insurance premiums paid for:
  - Themselves
  - Spouse
  - Dependents
  - Children under age 27

Reported on: Schedule 1, Line 17 (above-the-line deduction — reduces AGI)

Limitation: Cannot exceed net SE profit for the year.
Cannot deduct if eligible for employer-subsidized plan (spouse's employer plan counts).

ACA Interaction:
  If receiving ACA premium tax credits, only the net premium (after credit) is deductible.
```

---

## Business Structure Considerations

### Sole Proprietor vs Single-Member LLC vs S-Corp

| Factor | Sole Proprietor | Single-Member LLC | S-Corp Election |
|---|---|---|---|
| SE Tax | On all net profit | On all net profit | Only on "reasonable salary" |
| Formation | None | State filing (~$50–$500) | LLC + IRS Form 2553 |
| Legal protection | None | Yes | Yes |
| SE tax savings | None | None | Potential (on profits above salary) |
| Compliance cost | Low | Low | High (payroll, separate returns) |
| Minimum net profit to benefit | N/A | N/A | ~$40,000+ |

### S-Corp SE Tax Savings Example

```
Self-employed net profit: $100,000
SE tax as sole proprietor: ~$14,130

With S-Corp:
  Reasonable salary: $50,000 → FICA on salary only: $7,650
  Distribution: $50,000 → No SE tax
  S-Corp savings: ~$6,480/year

BUT: Additional costs:
  Payroll setup: ~$500–$1,500/year
  Tax preparation (Form 1120-S): ~$1,000–$2,000/year
  Net benefit meaningful only above ~$40,000–$50,000 net profit
```

---

## Open-Source Tools & Libraries

- [OpenFisca-US](https://github.com/PolicyEngine/policyengine-us) — Full US tax microsimulation including SE tax, credits, and benefits.
- [tax-calculator (Python)](https://github.com/PSLmodels/Tax-Calculator) — Policy Simulation Library's open-source US federal tax model.
- [self-employment-tax](https://www.npmjs.com/package/self-employment-tax) — npm package for SE tax calculations.
- [mileage-tracker (Python)](https://github.com/derwentx/mileage-tracker) — GPS-based mileage log generator.

---

## State-Level Self-Employment Tax

States do not impose a separate "self-employment tax" — SE tax is federal only. However, states tax self-employment **income** as ordinary income.

### States with No Income Tax (No State SE Income Tax)

Alaska, Florida, Nevada, New Hampshire, South Dakota, Tennessee, Texas, Washington, Wyoming

### Notable State SE Considerations

- **New York City:** NYC residents pay city income tax (3.876%) on top of NY state (up to 10.9%)
- **California:** SDI (State Disability Insurance) — self-employed can voluntarily opt in
- **Ohio, Pennsylvania:** Many municipalities have local earned income tax (1–3%)
- **Oregon:** Statewide Transit Tax applies to self-employment income

---

## Learning Resources

### Official IRS Videos & Courses

- [IRS Small Business Tax Workshop](https://www.irsvideos.gov/SmallBusinessTaxpayer/) — Free online tax course.
- [IRS YouTube Channel](https://www.youtube.com/irsvideos) — Short explainer videos.

### Communities

- [r/freelance](https://reddit.com/r/freelance) — 500k+ members; frequent tax Q&A threads
- [r/tax](https://reddit.com/r/tax) — Active community with CPA participation
- [r/Entrepreneur](https://reddit.com/r/Entrepreneur) — Business structure and tax strategy
- [r/UberDrivers](https://reddit.com/r/UberDrivers) — Platform-specific tax discussions

### Recommended Annual Resources

- [IRS Publication 334](https://www.irs.gov/publications/p334) — Re-read each January for updated limits
- [NASE (National Association for the Self-Employed)](https://www.nase.org/) — Advocacy and tax resources
- [SCORE Mentorship](https://www.score.org/) — Free mentorship from retired executives; tax guidance for small business

---

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

---

## Repo Info

**GitHub Topics:** `self-employment`, `freelance`, `gig-economy`, `1099`, `side-hustle`, `tax`, `us-tax`, `personal-finance`, `awesome-list`

**About:** A curated list of US self-employment tax resources, formulas, deduction guides, and gig platform tax breakdowns for freelancers, contractors, and side hustlers.
