# Paycheck Estimator — College Program

A single-file, no-dependency web tool that estimates **take-home pay per pay period** for a College Program cast member working at Walt Disney World (FL) or Disneyland (CA). Enter your hours and rate, and it breaks down 401(k), federal withholding, Social Security, Medicare, state tax, and housing rent.

> **Unofficial.** This is a planning tool made by and for program participants. It is not affiliated with, endorsed by, or sponsored by The Walt Disney Company. It is not tax or financial advice, and your actual paycheck may differ.

## Features

- **Hours × rate** or **gross amount** entry. Hours mode pays overtime at 1.5× the hourly rate; gross mode has an "overtime included" box so 401(k) can still exclude it
- **Walt Disney World, FL** (no state income tax), **Disneyland, CA** (2026 state income tax + SDI), or **custom flat state rate**
- Weekly, biweekly, semimonthly, or monthly pay frequency
- Pre-tax 401(k) calculated on regular pay only (overtime excluded, matching real paystubs)
- After-tax housing rent deduction (default $227/period)
- Federal withholding that supports the W-4 **Step 2 "multiple jobs" checkbox** table
- Responsive layout with small-screen rules for phones; no tracking, no network requests

## How the estimate works

All math runs in your browser using the 2026 tables below. Each period's wages are annualized, run through the published percentage-method tables, then divided back to the pay period.

| Item | Rule | Source |
|---|---|---|
| 401(k) | Chosen % × regular pay (overtime excluded), pre-tax for income tax | Verified against real paystubs |
| Federal income tax | IRS Pub 15-T 2026 percentage method, Single, standard **or** Step 2-checked schedule | [IRS Pub 15-T](https://www.irs.gov/publications/p15t) |
| Social Security | 6.2% of gross, wage base $184,500 | [SSA](https://www.ssa.gov/oact/cola/cbb.html) |
| Medicare | 1.45% of gross | IRS |
| CA income tax | EDD Method B, annual Single table, $5,706 standard deduction, low-income exemption ($18,896), $168.30 credit per DE 4 allowance | [EDD 2026 Method B](https://edd.ca.gov/siteassets/files/pdf_pub_ctr/26methb.pdf) |
| CA SDI | 1.3% of gross, no wage cap | [EDD rates](https://edd.ca.gov/en/payroll_taxes/rates_and_withholding) |
| Housing rent | Flat after-tax deduction | User input |

The Florida calculation was tested against four real weekly paystubs and matched federal tax, 401(k), and net pay to within $0.01 per check.

### Assumptions and limitations

- Filing status **Single**, no W-4 Step 3 credits, Step 4(a)/(b) adjustments, or extra withholding
- Federal Step 2 box defaults to **checked**, which is what matched the stubs this was tuned on. Switch it to "Not checked" if your W-4 differs
- California allowances default to **0** (what EDD uses when no DE 4 is on file)
- Doesn't model pay types other than regular and overtime (e.g. holiday premiums, PTO, Vol Shift). Use gross-amount mode and enter those totals if needed
- Social Security wage-base cap is applied per period, which only matters at very high pay
- Doesn't handle supplemental wage rates, local taxes, or year-end tax liability

## Run locally

No build step. Open `index.html` in any modern browser:

```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
open index.html        # macOS; use `xdg-open` on Linux or double-click on Windows
```

## Deploy on GitHub Pages

1. Create a new repository on GitHub and push these files to the `main` branch. `index.html` must be in the repository root:
   ```bash
   git init
   git add index.html README.md
   git commit -m "Initial commit"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<your-repo>.git
   git push -u origin main
   ```
2. On GitHub, open **Settings → Pages**.
3. Under **Build and deployment**, set **Source** to **Deploy from a branch**, choose branch **main** and folder **/ (root)**, then **Save**.
4. After a minute or two the site is live at:
   ```
   https://<your-username>.github.io/<your-repo>/
   ```

Updates deploy automatically whenever you push to `main`.

## Updating for a new tax year

Everything year-specific is a constant near the top of the `<script>` block in `index.html`. Each January:

1. **`fedStd` / `fedChk`**: replace with the new Pub 15-T percentage-method tables (Single/MFS; standard and Step 2 checkbox)
2. **`SS_WAGE_CAP`**: update to the new Social Security wage base
3. **`caBrackets`, `CA_STD_DEDUCTION`, `CA_LOW_INCOME_EXEMPT`, `CA_ALLOWANCE_CREDIT`**: update from the new EDD Method B PDF (Tables 1, 3, 4, and 5)
4. **`CA_SDI_RATE`**: update from the EDD rates page
5. Re-check the result against a recent paystub

## Design

The color scheme uses six base colors, with a few darker shades/tints of them for text and surfaces so everything meets WCAG AA contrast:

| Swatch | Hex |
|---|---|
| Cyan | `#7EF1FC` |
| Yellow | `#FCEC7E` |
| Pink | `#FC7EC6` |
| Mauve | `#A78C9B` |
| Slate | `#647B7D` |
| Olive | `#7D7A64` |

Colors are CSS variables in the `:root` block at the top of `index.html`.

## License

Choose a license for your repository (for example [MIT](https://choosealicense.com/licenses/mit/)) and add it as `LICENSE`.
