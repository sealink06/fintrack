# Family FinTrack

A single-page wealth tracker for a family. It shows the combined net worth of everyone in the household across Indian stocks, ETFs, mutual funds, US stocks, bonds, gold, silver, real estate, retirement accounts, deposits and loans, with correct profit and loss based on average buy prices.

It is one file (`index.html`). There is no server, no build step and no account to create. Open it in a browser and it works. Your data stays in your own browser.

> **Live demo:** `https://<your-username>.github.io/<repo-name>/`
> The hosted copy starts empty. Anything you enter is saved only in your own browser, not on GitHub.

---

## Contents

1. [What it does](#what-it-does)
2. [Quick start](#quick-start)
3. [Using the app](#using-the-app)
4. [Importing your data](#importing-your-data)
5. [How average buy price and P&L are worked out](#how-average-buy-price-and-pl-are-worked-out)
6. [Live prices](#live-prices)
7. [Sign-in and recovery code](#sign-in-and-recovery-code)
8. [Privacy and where your data lives](#privacy-and-where-your-data-lives)
9. [Using it on your phone](#using-it-on-your-phone)
10. [Hosting your own copy on GitHub Pages](#hosting-your-own-copy-on-github-pages)
11. [Backing up your data](#backing-up-your-data)
12. [Known limits](#known-limits)
13. [Troubleshooting](#troubleshooting)
14. [Tech notes](#tech-notes)

---

## What it does

- **Family net worth in one place.** A dashboard with total net worth, invested amount, unrealised and realised profit, day change, allocation by asset class and a net worth history.
- **A portfolio per family member.** Add each person, switch between "Whole family" and one member, and see each portfolio on its own.
- **Market holdings.** Indian stocks, ETFs, mutual funds, US stocks (valued in rupees using the dollar rate) and bonds.
- **Other assets.** Gold, silver, real estate, retirement (PPF, EPF, NPS), cash and deposits (savings, FD, RD), vehicles and other items. Gold and silver can be entered in grams and valued at the current rate.
- **Liabilities.** Home loans, car loans and credit cards are subtracted from net worth.
- **Automatic average buy price.** Add more shares to something you already own and the average buy price updates by itself.
- **Import from Excel or CSV.** Upload your own tracker, a holdings export or a broker transaction statement. Average buy prices and profit are worked out for you.
- **Live prices.** Mutual fund NAVs, stocks, ETFs, US shares, gold (24K, 22K, 18K), silver and the dollar rate update automatically.
- **Optional sign-in.** Lock the app with a username and password, with a recovery code in case you forget the password.
- **Works on a phone.** The layout adapts to small screens.
- **Hide amounts.** One tap blurs every figure, handy when someone is looking over your shoulder.

---

## Quick start

1. Open the page (the GitHub Pages link above, or `index.html` on your computer).
2. The first screen offers to create a login. Either create one, or choose **Skip for now** to try the app as it is. You can add a login later from Settings.
3. Add your family members under **Family**.
4. Add holdings with **+ Add**, or bring them in with **Import**.
5. Add gold, silver, property, deposits and loans under **Assets**.
6. Open **Settings** to set the family name and check the live price options.

---

## Using the app

| Section | What you do there |
|---|---|
| **Dashboard** | Net worth, invested amount, profit and loss, day change, allocation, top holdings and biggest movers. |
| **Holdings** | Every stock, ETF, mutual fund, US stock and bond. Search, filter by type or class, sort, and tap a row to open the holding. |
| **Assets** | Gold, silver, real estate, retirement, cash and deposits, other assets and liabilities. |
| **Family** | Add members, give each a colour, and see each member's share of the total. |
| **Transactions** | The full buy and sell history across holdings. |
| **Import** | Upload Excel or CSV files. Templates are provided for holdings and transactions. |
| **Settings** | Family name, rates, live price options, sign-in, export and backup, and delete all data. |

### Adding a holding

Choose **+ Add**, then the member, type (Indian stock, ETF, mutual fund, US stock, bond), symbol and name.

- **Buying now:** enter the date, quantity and buy price.
- **Already own it:** enter the quantity you hold and your **average buy price**. This is saved as an opening position, and later purchases are averaged on top of it.
- If you add a symbol that the member already holds, the form tells you the current quantity and average, and shows what the new average will be before you save.

### Holding details

Tap any holding to see quantity, average buy price, current price, value and profit. From there you can:

- add a **buy** or a **sale**;
- set the position directly (replaces the history with one opening entry);
- delete past transactions;
- update the price by hand;
- set the **Yahoo symbol** (stocks, ETFs, bonds, US shares) or **fund code** (mutual funds) used for live prices.

### Other assets

- **Gold and silver** can be entered by value, or in grams with a purity (for example 22K). Grams are valued from the gold and silver rates, so they move with the live rate.
- **Liabilities** are entered as positive amounts and subtracted from net worth.
- Each asset can hold a cost, a date and a note.

### Net worth history

The app saves one net worth point per day when you open it, and draws the history from those points. The chart fills in over time.

---

## Importing your data

Go to **Import** and drop in a file. Supported: `.xlsx`, `.xls`, `.csv`, `.tsv`, `.txt`. The file is read inside your browser and is never uploaded anywhere.

The importer looks at each sheet and works out what it contains. It recognises common column names such as symbol, name, quantity, average price, invested amount, LTP, current price, NAV, trade date, buy or sell, and price.

| Kind of sheet | What the importer does |
|---|---|
| **Transaction statement** (a row per buy or sell with date, symbol, quantity and price) | Builds the history, works out the average buy price and the realised profit. |
| **Holdings sheet** (symbol, quantity, average price or amount invested) | Creates holdings with an opening position at that average. |
| **Price list** (symbol and price only) | Updates prices on holdings you already have. |
| **Tracker with several blocks** (for example Indian stocks, mutual funds and US stocks on one sheet) | Splits the blocks and tags each as the right type and currency. |

Things the importer handles for you:

- Header rows are found even when they are not on the first row.
- Section labels such as "US stocks" or "Mutual funds" set the type and currency for the rows below.
- Percentage changes given as `0.012` or `1.2` are both understood.
- Repeat imports do not double count. Trades are matched by trade id, or by date, type, quantity and price.
- A row that already has a buy price beats a row for the same holding that does not.
- Before anything is saved you see a preview of what was found and what will be created.

Download the **holdings** or **transactions** template from the Import page if you want to prepare a file by hand. Pick which member the data belongs to under **Import into** (or create a new member there). Import one file per member. For holdings you already have, choose whether the file **replaces** the position or is **added** to it.

If a holding has no buy price, its value still counts toward net worth, but it is left out of profit and loss, and a banner tells you how many are affected.

---

## How average buy price and P&L are worked out

Family FinTrack uses the **average cost method**.

- **Opening position:** a quantity you already hold, at an average price you supply (or with the price unknown).
- **Buy:** adds quantity. The new average is total cost divided by total quantity. Fees are added to the cost.
- **Sell:** reduces quantity at the running average, and the difference between the sale price and the average is booked as **realised profit**.
- **Unrealised profit:** current price minus average buy price, times quantity held.
- **Unknown cost:** units whose buy price is not known are tracked separately and left out of profit and loss. The holding shows a "part" tag so you can see that the average only covers some of the units.
- **US holdings** are valued in rupees at the current dollar rate. Cost is converted at the same rate, so the profit figure includes currency movement.

Example: buy 10 at 100, then buy 10 at 120. The average becomes 110 for 20 units. Sell 5 at 130 and you realise 5 × (130 − 110) = 100, with 15 units left at an average of 110.

---

## Live prices

When the page is open, it refreshes prices every 10 minutes and once at start-up. The refresh button in the header does it on demand. Turn this off, or change the options, under **Settings → Live prices**.

| What | Where it comes from | Notes |
|---|---|---|
| **Mutual funds** | [mfapi.in](https://www.mfapi.in/) (AMFI NAV data) | Funds are matched by name to the Direct Growth plan. NAVs update once a day. |
| **Indian stocks, ETFs, bonds** | Yahoo Finance chart data | Tries NSE (`.NS`) first, then BSE (`.BO`). |
| **US stocks** | Yahoo Finance chart data | Plain ticker, for example `AVGO`. |
| **Dollar rate** | open.er-api.com, with Yahoo as backup | Used for all US holdings. |
| **Gold and silver** | gold-api.com spot price, with Yahoo futures as backup | Converted to rupees per gram. Gold is shown as 24K, 22K and 18K. |

### Things to know

- **Gold and silver use world spot prices.** Indian shop prices are usually higher because of import duty and GST. Under Settings, use **Add to metal spot price (%)** until the rate matches your jeweller. You can also turn off automatic metal rates and type the rates in yourself.
- **Yahoo does not allow web pages to call it directly.** The app reaches Yahoo through a free public relay. If the built-in relays stop working, set your own under **Own price relay**, using an address that contains `{url}`, for example `https://your-relay.example/?url={url}`.
- **A holding that cannot be matched is listed** in Settings under "could not be updated". Open it and set its **Yahoo symbol** (for example `HDFCBANK.NS`) or **fund code** (the AMFI scheme code) and tap **Save and refresh**.
- **Prices can be delayed.** These are free sources, so treat prices as indicative and not as trading quotes.
- **Bonds** update only if they have a Yahoo ticker. Otherwise keep their price by hand.
- Refreshes run only while the page is open and visible. Nothing runs in the background.
- Prices go from the sources straight to your browser. Your holdings, quantities and amounts are never sent. Only the symbols and fund names are used to look up prices, and the relay can see the symbols.

---

## Sign-in and recovery code

Sign-in is optional and protects data on the **device you are using**.

### Creating a login

1. On first open, enter a username and a password (at least 8 characters) and confirm it.
2. You are shown a **recovery code**, for example `BDCQ8-A6AW2-R2MSG-74VQF`. Copy it, download it, or screenshot it, and keep it somewhere safe such as a password manager. You must tick "I have saved my recovery code" to continue.
3. From then on the dashboard opens only after the right username and password. The username is not case-sensitive. The lock button in the header locks the app straight away.

### Forgot your password?

On the sign-in screen choose **Forgot your password?**, enter the recovery code, and set a new username and password. Your data is kept.

If you have lost both the password and the recovery code, there is no way to open the encrypted data. The only option left is **I do not have my recovery code**, which erases everything saved on that device so you can start again. Nobody, including the author of this app, can recover it for you.

### Skip for now

Choose **Skip for now** to use the app with no login. It remembers your choice. To add a login later, go to **Settings → Sign-in → Create a login**. Anything you have already entered is kept and then locked with the new login.

### Changing details later

Under **Settings → Sign-in** you can change the username or password, or generate a new recovery code (the old one stops working).

### How it is protected

- Your data is encrypted in the browser with **AES-256-GCM** using a random data key.
- That key is stored twice: once locked by your username and password, and once by your recovery code. Both locks use **PBKDF2-SHA-256 with 200,000 rounds**.
- Repeated wrong attempts add a short delay.
- Encryption uses the browser's built-in Web Crypto features. If a browser does not offer them, the lock is skipped and the app works without sign-in.

This is a lock for a personal device, not a bank-grade account system. It stops someone who opens your browser from reading your data. It cannot protect you from a device that already has malware.

---

## Privacy and where your data lives

- **Everything is saved in your own browser** (local storage), on the device you used.
- **There is no server and no database.** The page on GitHub is only the program. It does not receive or store your data.
- **Different devices do not share data.** A phone and a laptop each have their own copy. Use export and backup to move data between them.
- **Clearing browser data erases your data.** Use a private or incognito window and the data disappears when you close it. Back up first.
- **Outside requests the app makes:** price lookups (see Live prices) and the Excel-reading library, loaded from cdnjs. Nothing about your holdings is sent in these requests.
- **Do not put real family data into a public repository.** Only the app (`index.html`) belongs on GitHub, and it starts empty.

---

## Using it on your phone

Open the same link in Safari or Chrome. The layout fits small screens, with a compact holdings table. Each browser keeps its own data, so data entered on a computer does not appear on the phone unless you export and import it.

To open it like an app: on iPhone use **Share → Add to Home Screen**, and on Android use **menu → Add to Home screen**.

---

## Hosting your own copy on GitHub Pages

1. Create a new repository on GitHub, for example `family-fintrack`.
2. Upload `index.html` and this `README.md` to the main branch.
3. Go to **Settings → Pages**. Under **Build and deployment**, set **Source** to **Deploy from a branch**, branch **main**, folder **/ (root)**, and save.
4. After a minute or two the page is live at `https://<your-username>.github.io/<repo-name>/`.
5. To update later, upload a new `index.html` over the old one. Refresh the page (or open it in a private tab) to see the change.

Notes:

- On the free GitHub plan, Pages sites are public. That is fine for this app because the page holds no data. Keep it that way: never commit exports, backups or spreadsheets with real figures.
- A private page with access only for you needs GitHub Enterprise Cloud. To keep your own copy private on a free plan, open `index.html` from your own computer instead of hosting it.

---

## Backing up your data

Under **Settings → Export and backup** you can download:

- **Holdings (CSV)**
- **Transactions (CSV)**
- **Other assets (CSV)**
- **Full backup (JSON)**

The CSV files open in Excel and can be imported again. Back up before clearing browser data, switching browsers or changing devices.

A one-click restore from the JSON backup is not available yet. To move data, re-import the CSV files.

---

## Known limits

- Data is per browser. There is no cloud sync.
- Prices come from free public sources and can be delayed or occasionally unavailable.
- Some mutual funds may not match by name. You can set the fund code by hand.
- Gold and silver rates are world spot prices plus your premium setting, not a jeweller's rate.
- Holdings with no buy price are left out of profit and loss.
- There is no email or Google or Apple sign-in. Sign-in is a local lock only.
- Tax lots, capital gains tax and XIRR are not calculated.
- Corporate actions such as splits and bonus shares are not applied automatically. Adjust the quantity and average from the holding details.

---

## Troubleshooting

| Problem | What to try |
|---|---|
| The page shows an old version after an update | Hard refresh, or open it in a private tab. GitHub Pages can take a couple of minutes. |
| Prices do not update | Check the connection, then open **Settings → Live prices** and read the last refresh message. Tap **Refresh now**. If stock prices keep failing, set an own relay. |
| One holding never updates | Open it and set its Yahoo symbol or fund code. |
| Gold rate looks lower than the shop | Raise **Add to metal spot price (%)** in Settings. |
| Forgot the password | Use the recovery code on the sign-in screen. |
| Lost password and recovery code | The data cannot be opened. Erase and start again, then import from your backup files. |
| An import shows nothing found | Check that the sheet has a header row with names like symbol, quantity and price. Try the template from the Import page. |
| The data disappeared | The browser data was cleared or a different browser or device is in use. Import from your backup. |
| Buttons for exports do nothing | Allow downloads for the site in your browser settings. |

---

## Tech notes

- One HTML file with inline CSS and JavaScript. No framework and no build tooling.
- Reads Excel files with [SheetJS](https://sheetjs.com/) (`xlsx` 0.18.5), loaded from cdnjs.
- Storage: browser `localStorage`. With sign-in on, it is stored encrypted using the Web Crypto API (AES-GCM, PBKDF2).
- Design: a dark theme with an ink background, green for gains, red for losses and violet and cyan accents.
- Averages use the average cost method, with unknown-cost lots tracked separately.

---

## Disclaimer

Family FinTrack is a personal record-keeping tool. It is not financial, tax or investment advice. Prices and calculations are provided as is and may contain errors. Check important figures against your broker, fund house and bank statements.
