# 02 — Data-sets

> Before feeding anything to a model, you need to understand the shape and
> nature of your inputs. The shape of a dataset (how many rows against how
> many columns) and the type of each attribute (what arithmetic it permits)
> are not vocabulary — they are constraints that decide which models can work
> at all and which conclusions are legitimate.

---

## 1. Anatomy of a dataset

An alphanumeric dataset is a collection of **items** (also called rows,
records or observations), each described by the same set of
**attributes** (columns, features).

- **Size** $n$ — the number of rows.
- **Dimensionality** $p$ — the number of attributes (feature columns).

**The ratio matters more than either number alone.** That is the subject of
§2.

---

## 2. Size vs. dimensionality: degrees of freedom

### 2.1 Every row is an equation, every column is an unknown

Take a straight-line model $y=a+bx$ fitted to data. Each **row** of the data
gives one equation $a+bx_i=y_i$. Written as a matrix, for three days:

$$
\underbrace{\begin{pmatrix}1 & 1\\ 1 & 2\\ 1 & 3\end{pmatrix}}_{3\text{ rows}\times 2\text{ columns}}
\begin{pmatrix}a\\ b\end{pmatrix}
=\begin{pmatrix}3\\ 5\\ 6\end{pmatrix}
$$

- **Column 1 is all 1s.** Its partner is $a$, the intercept — a column of 1s
  because $a$ is added to every row in equal amount.
- **Column 2 is the $x$ values.** Its partner is $b$, the slope.

So there is **one parameter per column**. The parameters are not *in* the
data; they are the unknowns we solve for, and each one is attached to a
column. The $y$ values sit separately on the right-hand side — they are what
we try to match.

With $p$ features and an intercept, the number of unknowns is

$$
k=p+1
$$

The comparison that decides everything is **rows ($n$, equations) against
columns ($k$, unknowns)** — which is exactly size against dimensionality.

### 2.2 Square: a perfect fit that proves nothing

Two days of data, one feature:

| Day | $x$ | $y$ |
| :-: | :-: | :-: |
| 1 | 1 | 3 |
| 2 | 2 | 5 |

The two equations

$$
\begin{aligned}
a+b(1)&=3\\
a+b(2)&=5
\end{aligned}
$$

subtract to give $b(2-1)=5-3$, so $b=2$, and then $a=3-2=1$: the line
$y=1+2x$ passes through both points exactly.

**Why this proves nothing.** In matrix form,

$$
\begin{pmatrix}1 & x_1\\ 1 & x_2\end{pmatrix}\begin{pmatrix}a\\ b\end{pmatrix}=\begin{pmatrix}y_1\\ y_2\end{pmatrix}
$$

has a unique solution whenever the matrix can be inverted, i.e. whenever

$$
\det=(1)(x_2)-(x_1)(1)=x_2-x_1\neq0
$$

The only requirement is $x_1\neq x_2$. **The $y$ values never enter the
condition.** Whatever they are — even pure noise — a line passes through both
points. A fit that was guaranteed in advance carries no information.

(Edge case: if $x_1=x_2$ the points stack vertically, and there is either no
line through both, if $y_1\neq y_2$, or infinitely many, if $y_1=y_2$.)

### 2.3 Tall: a fit that could have failed

Add a third day: $x=3$, $y=6$. The line built from days 1–2 predicts

$$
\hat y_3=1+2(3)=7
$$

and the miss, actual minus predicted, is the **residual**:

$$
y_3-\hat y_3=6-7=-1
$$

(Negative: the line overshot.) No straight line fits all three points, since
the slopes between pairs differ:

$$
\frac{5-3}{2-1}=2\qquad\neq\qquad\frac{6-5}{3-2}=1
$$

**This is the good outcome.** The third row was a spare equation, and it
disagreed with the line. That disagreement is information. With a tall
matrix you cannot satisfy every equation, so you find the line that comes
*closest* to all the points, and the size of the leftover misses tells you how
good the model is. How "closest" is defined comes later in the course.

### 2.4 Degrees of freedom

Each unknown **uses up** one row. The rows left over after pinning down the
unknowns are the **degrees of freedom**:

$$
\text{df}=n-k
$$

- 2 points, a line: $\text{df}=2-2=0$ — nothing left to check with.
- 3 points, a line: $\text{df}=3-2=1$ — one spare point that could disagree.
- 252 days, a line: $\text{df}=252-2=250$ — a good fit now means something.

**The governing principle: a fit is only evidence if it could have failed.**
An everyday version: a friend sees your two points and declares "the rule is
$y=1+2x$." You cannot check him, since any two points fit some line. Give him
a third point he has not seen; if his rule predicts it, you have learned
something. That third point is the degree of freedom.

| Shape | Example | What happens | Is the fit evidence? |
| :--- | :--- | :--- | :--- |
| **Wide**: $n<k$ | 2 points, a curve with 3 knobs | Infinitely many perfect fits | No |
| **Square**: $n=k$ | 2 points, a line | Exactly one perfect fit, guaranteed | No |
| **Tall**: $n>k$ | 252 days, a line | Usually no perfect fit; find the closest | **Yes** |

**Von Neumann's elephant.** "With four parameters I can fit an elephant, and
with five I can make him wiggle his trunk." A model that can fit *anything*
tells you *nothing* — the same principle by which a theory no observation
could contradict says nothing about the world.

### 2.5 Applied to a trading bot

One year of daily gold data gives $n=252$. With an intercept plus $p$
indicators, the matrix becomes square when

$$
k=n \iff p+1=252 \iff p=251
$$

so 251 indicators guarantee a perfect — and meaningless — fit.

With 30 indicators:

$$
\text{df}=252-(30+1)=221
$$

Add 5 more columns and

$$
\text{df}=252-(35+1)=216
$$

**Adding columns reduces degrees of freedom**, because columns enter $k$ and
$k$ is subtracted. The only way to *increase* df is to add **rows** — more
data. The quick check: *am I adding observations, or adding knobs?*
Observations let you check a model; knobs make it easier for the model to fit
anything.

**Data snooping.** If you tried 100 indicators and *kept the 30 that fit
best*, the formula still reports $\text{df}=221$, but the data has also been
used to choose the columns. Some survivors will look useful purely by chance —
the usual recipe for an over-optimised backtest. The remedy is the one from
[[01 The Data Life Cycle.]]: judge the chosen model on data it never saw
while being built.

---

## 3. Data structures

- **Structured.** Every record has exactly the same attributes
  (homogeneous) — standard tabular price data.
- **Semi-structured.** Records can have different schemas with optional
  attributes (heterogeneous). Common formats are XML and JSON, which you meet
  constantly when pulling market data from web APIs.
- **Unstructured.** No explicit tags separating records or fields — financial
  news text, central bank statements.

### 3.1 Wide vs. long format

The same closing prices, two layouts. **Wide:**

| Date | XAUUSD | GBPJPY | EURUSD |
| :--- | :-: | :-: | :-: |
| 1 Oct | 2650 | 193.1 | 1.105 |
| 2 Oct | 2662 | 192.4 | 1.102 |
| 3 Oct | 2655 | 193.8 | 1.108 |

Here $n=3$ and $p=3$ — but the three assets are not genuinely *features* of a
date. All three columns hold the **same variable** (closing price) measured on
different things. Adding a 500th asset adds a 500th column without describing
any new kind of quantity. Wide format is still convenient for some jobs, such
as lining returns up side by side to compute correlations.

**Long** (3 dates × 3 assets = 9 rows):

| date | asset | close |
| :--- | :--- | :-: |
| 1 Oct | XAUUSD | 2650 |
| 1 Oct | GBPJPY | 193.1 |
| 1 Oct | EURUSD | 1.105 |
| 2 Oct | XAUUSD | 2662 |
| … | … | … |

This is **tidy data**: each variable is one column, each observation is one
row. A new asset adds rows, not columns.

### 3.2 Panel data and its natural key

In the long table, `date` alone does not identify a row (each date appears
three times), and neither does `asset`. The smallest unique identifier is the
**pair `(date, asset)`**. Data indexed by an **entity** and a **time** is
**panel data**, and that pair is its natural key (see §5).

---

## 4. Attribute domains (measurement scales)

Each scale permits certain arithmetic and forbids the rest.

| Scale | Example | Order? | Differences? | Ratios? |
| :--- | :--- | :-: | :-: | :-: |
| **Nominal** (categorical) | Asset class: "FX", "Commodities" | No | No | No |
| **Ordinal** | Credit rating AAA … C | Yes | No | No |
| **Interval** | Temperature in °C | Yes | Yes | No |
| **Ratio** | Gold price in \$ | Yes | Yes | Yes |

### 4.1 Interval vs. ratio

Interval data has an **arbitrary zero**; ratio data has an **absolute zero**.

**Temperature (interval).** 30°C against 15°C is not "twice as hot." Redo it
in Fahrenheit:

$$
\frac{86}{59}\approx1.46
$$

A genuine property of the weather cannot change because you changed units, so
the ratio was never meaningful — it depends on where someone put zero. The
**difference** survives the change of units: $15\times\tfrac95=27$°F, and
$86-59=27$ ✓. That is the defining test: **differences meaningful, ratios
not.** On a true ratio scale (Kelvin, where 0 K means no thermal energy),
$\tfrac{303.15}{288.15}\approx1.05$: physically only about 5% hotter.

**Gold (ratio).** \$1,000 → \$2,000 genuinely "doubled," because \$0 is a true
zero — the asset is worthless. Converting to shillings *multiplies* every
price by the exchange rate but does not shift zero, so "doubled" holds in any
currency. Ratio scales allow rescaling, not moving the zero.

### 4.2 The scale comes from the economics, not the numbers

In April 2020, WTI oil futures closed at about **−\$37**, from roughly \$18
the day before. The percentage return:

$$
r=\frac{-37-18}{18}=\frac{-55}{18}\approx-305.6\%
$$

a loss of more than 100%, impossible for a stock or a bar of gold. The log
return $\ln(-37/18)$ is **undefined**.

**A tempting fix that destroys data.** Taking the log of the positive version
and negating it gives $-\ln(37/18)\approx-0.72$. Converted back to a price,
$18\times e^{-0.72}\approx\$8.76$ — the "fix" silently replaces −\$37 with
+\$8.76, turning the most extreme day in oil-futures history into an ordinary
bad day.

**Why the price went negative.** A futures contract is an **obligation** to
take delivery. With storage at the delivery hub nearly full, holders of
expiring contracts had nowhere to put the oil and **paid** others to take the
contracts. A futures price is the price of a commitment, and a commitment can
be a liability. So the series is really **interval-like**: dollar differences
(profit or loss per contract) are always meaningful; ratios and percentages
break. The same family includes interest rates (negative in Europe and
Japan), spreads, and electricity prices (negative when the grid has too much
power).

**Rule:** for any series that can go negative, model **changes in dollars**,
not percentage or log returns. A variable's scale comes from the economics of
what it measures — gold cannot go below zero because owning it cannot cost
more than you paid; a futures contract can.

---

## 5. Encoding categorical and ordinal attributes

### 5.1 Dummy variables and the dummy variable trap

A categorical attribute enters a model through **dummy variables**: one 0/1
column per category. For weekdays, with an intercept:

| Day | Intercept | Mon | Tue | Wed | Thu | Fri |
| :--- | :-: | :-: | :-: | :-: | :-: | :-: |
| Monday | 1 | 1 | 0 | 0 | 0 | 0 |
| Tuesday | 1 | 0 | 1 | 0 | 0 | 0 |
| Wednesday | 1 | 0 | 0 | 1 | 0 | 0 |

Every row has exactly one day switched on, so

$$
\text{Mon}+\text{Tue}+\text{Wed}+\text{Thu}+\text{Fri}=1\quad\text{in every row}
$$

which is **identical to the intercept column**:

$$
\text{Intercept}-(\text{Mon}+\text{Tue}+\text{Wed}+\text{Thu}+\text{Fri})=\mathbf 0
$$

The **columns** are **linearly dependent**: one can be built exactly from the
others. (Columns, not rows — columns are where the parameters live, so
dependence between columns is what destroys your ability to solve for them.)

**The consequence.** Add any constant $c$ to $a$ and subtract $c$ from every
day coefficient. On a Monday row:

$$
(a+c)+(b_{\text{Mon}}-c)(1)+(b_{\text{Tue}}-c)(0)+\dots=a+b_{\text{Mon}}
$$

The shift cancels on every weekday, so **infinitely many** coefficient sets
produce identical predictions. The data cannot prefer one over another and no
unique solution exists — the same failure as the $x_1=x_2$ edge case in §2.2,
where the determinant was zero. This is the **dummy variable trap**, a case of
**perfect multicollinearity**. Some software errors; R silently drops a column
and reports `NA`.

**The fix: drop one dummy.** Keep the intercept plus Tue–Fri. Monday becomes
the **baseline**:

- $a$ is Monday's average level.
- Each $b$ is how much that day differs from Monday.

**Rule:** with an intercept, a category with $m$ levels needs $m-1$ dummies.
(For the bot in §2.5: 30 indicators + 4 dummies + intercept gives
$\text{df}=252-35=217$.)

**A coordinate choice.** Measuring every day against Monday or against Friday
gives identical predictions; only the labels on the coefficients change.
Whenever a model's output does not depend on some choice, that choice cannot
be learned from data — you must fix it yourself.

### 5.2 Ordinal attributes coded as numbers

Coding ratings as a single numeric column

| Rating | AAA | AA | A | BBB | BB | B |
| :--- | :-: | :-: | :-: | :-: | :-: | :-: |
| Code | 1 | 2 | 3 | 4 | 5 | 6 |

in the model $\text{yield}=a+b\times\text{code}$ forces

$$
\text{AAA}\to\text{AA}:\;b(2-1)=b\qquad\qquad\text{BBB}\to\text{BB}:\;b(5-4)=b
$$

**Every one-notch step is exactly $b$.** The encoding quietly turns an
**ordinal** variable (ranked, gaps unknown) into an **interval** one (equal
gaps). The model never learned that the gaps are equal; the choice of numbers
asserted it.

In reality the steps are not equal. BBB → BB is the boundary between
**investment grade** and **junk**, where many funds are *required* to sell, so
yields typically **jump** there.

**Fix: one dummy per rating, with a baseline.** Six levels need $6-1=5$
dummies; with AAA as baseline,

$$
\text{yield}=a+b_{\text{AA}}\text{AA}+b_{\text{A}}\text{A}+b_{\text{BBB}}\text{BBB}+b_{\text{BB}}\text{BB}+b_{\text{B}}\text{B}
$$

- $a$ is the average AAA yield; each $b$ is that rating's premium over AAA.
- Any step is a difference of coefficients — BBB → BB is
  $b_{\text{BB}}-b_{\text{BBB}}$ — and the data decides its size.

**The cost:** 5 columns instead of 1, using 4 extra degrees of freedom. The
recurring trade-off: **flexibility costs degrees of freedom.**

**Middle ground (2 columns):** keep the numeric code *and* add
$\text{Junk}=1$ for BB or below. The code captures the overall trend; the
dummy captures the cliff. If you know where the structure is, you can spend
fewer columns describing it.

**Tangent.** Default risk rises much faster than linearly as ratings fall, so
spreads look closer to exponential in the notch than linear. Practitioners
therefore often model the **log** of the spread against the notch number —
changing the scale of $y$ can make a straight line appropriate again.

---

## 6. Relational data

Relational databases, managed with SQL, are the standard way to organise and
link tabular data for a backtesting engine or trading database.

- **Databases and tables.** A database (`CREATE DATABASE`) is the overarching
  repository housing interconnected tables (`CREATE TABLE`). Each table has a
  defined **schema** listing its columns and data types.
- **Data types.** SQL requires strict typing: strings (categorical/ordinal
  data), numbers (integers or floats for prices and returns), temporal types
  (timestamps for execution logs), booleans.
- **Keys (the glue).** A key is the minimal set of attributes needed to
  uniquely identify a row. A **primary key** identifies the record; a
  **foreign key** links it to a row in another table.

### 6.1 Composite primary keys

For a prices panel, the primary key is `(asset_id, date)`. The database then
**refuses** a second price for the same asset on the same day — the
duplicates rule from [[01 The Data Life Cycle.]] enforced automatically.

### 6.2 Why a ticker is a bad key

Tickers are not permanent:

- **They change.** Facebook went from FB to META in 2022. Key on ticker and
  one company's history splits into two "different" assets.
- **They get reused.** After a delisting, a ticker can later be assigned to an
  unrelated company. Key on ticker and two companies' histories merge into one
  — reintroducing the survivorship and delisting problems of chapter 1.

**Fix:** a permanent internal identifier that never changes (CRSP calls theirs
PERMNO). The ticker becomes an ordinary attribute stored with the dates it was
valid.

### 6.3 Normalisation: store each fact once

```sql
CREATE TABLE assets (
  asset_id    INTEGER PRIMARY KEY,
  name        TEXT,
  asset_class TEXT              -- categorical: 'FX', 'Commodity', ...
);

CREATE TABLE ticker_history (
  asset_id    INTEGER REFERENCES assets(asset_id),   -- foreign key
  ticker      TEXT,
  valid_from  DATE,
  valid_to    DATE
);

CREATE TABLE prices (
  asset_id    INTEGER REFERENCES assets(asset_id),   -- foreign key
  date        DATE,
  close       NUMERIC,
  PRIMARY KEY (asset_id, date)
);
```

Storing the asset's name in every price row instead would mean a rename
touches thousands of rows, and missing one leaves the database contradicting
itself. Foreign keys guarantee every price row points to an asset that
exists.

### 6.4 A table that blocks look-ahead bias

Chapter 1 established that every data point has a **reference time** (what it
describes) and a **knowledge time** (when it became available). For anything
published or revised — earnings, GDP — store both:

```sql
CREATE TABLE fundamentals (
  asset_id    INTEGER REFERENCES assets(asset_id),
  period_end  DATE,     -- reference time: what it describes
  report_date DATE,     -- knowledge time: when it became public
  eps         NUMERIC,
  PRIMARY KEY (asset_id, period_end, report_date)
);
```

Every backtest query then asks *what was known as of date $t$*:

```sql
SELECT * FROM fundamentals
WHERE report_date <= :t;   -- only what was public at decision time
```

taking the latest version per period from the result. With knowledge time
stored and every query filtered on it, look-ahead bias is **impossible by
construction** rather than something you must remember to avoid.

**The as-of join.** The same idea in Python is `pandas.merge_asof`: each
trade is matched to the most recent quote **at or before** its timestamp,
never one after — the right tool for aligning a bot's fills with tick data.

---

## 7. Additions from the lecture notes (BSF 3216, §1.2–1.3)

### 7.1 Alphanumeric vs. multimedia data

- **Multimedia data** represents audiovisual information — video, images,
  audio.
- **Alphanumeric data** is collections of characters representing alphabetic
  (names) and numeric data. An alphanumeric *dataset* is a collection of data
  items.

An item goes by different names in different fields: a **row/tuple** in
databases, a **record** in computer science, an **observation** in statistics,
and an **entity, instance or (data) point** elsewhere. Each item describes a
real-world entity, fact or event through a group of related characteristics
(variables, features, measurements).

### 7.2 Size, dimensionality, simple vs. complex attributes, schema

- **Size / cardinality** — number of records (rows).
- **Dimensionality** — number of attributes in a record.
- An attribute is **simple** if its value is a number or a label, and
  **complex** if it has parts that each have a value (e.g. a timestamp with date
  and time parts).
- The attributes that make up a record are the record's **schema**.

### 7.3 Structured data and tabular data

**Structured** (homogeneous) data: all records have values for the same
attributes. **Tabular** data is structured data whose attributes are all
*simple* — presented as a table with rows (records) and columns (attributes).
Example: the New York flight dataset (transtats.bts.gov).

### 7.4 Semi-structured data is self-describing

Each record may have its own schema (heterogeneous): attributes may be
optional, a mix of simple and complex, or hold a *collection* of values.
Because each record carries both its schema and its data, semi-structured data
is called **self-describing**. XML and JSON are the common formats.

**Lecturer's example — an email dataset.** Each email has an ID, a header and a
body. The header is complex (timestamp, sender, receiver, subject); the
timestamp is itself complex (date, time); an optional CC attribute may hold one
value or a list.

**Unstructured** data has no tags or markers separating records into
attributes — usually natural-language text (e.g. the ProPublica *Hate Crime*
news index).

### 7.5 A nominal attribute coded with numbers is still nominal

The notes stress: values of a nominal attribute may be represented by numbers,
but that does not make them numerical — the same trap as the ordinal coding in
§5.2. Ordinal attributes allow enumerating a first, second, …, last element and
comparing positions, nothing more.

### 7.6 SQL details from the notes

```sql
CREATE DATABASE name;     -- create a database (schema)
USE database_name;        -- choose which database to work on

CREATE TABLE Revenues (
  name              char(64),
  sector            char(64),
  num_of_employees  int,
  gross_revenue     float
);
```

The table name is followed by its schema: each attribute has a name (unique
within the table) and a data type.

**Data types:**

- **Strings** — for nominal (and sometimes ordinal) attributes.
  `character(n)` / `char(n)` has **fixed** size $n$; `character varying(n)` /
  `varchar(n)` has **variable** size up to $n$.
- **Numbers** — `int`/`integer` for whole numbers; `float`/`decimal` for reals.
  Used for interval, ratio or absolute values.
- **Temporal** — date, time, timestamp, interval.
- **Booleans** — True/False.

**Keys** are the *minimal* set of attributes needed to tell rows apart — every
attribute in the key is needed. Copies of primary keys kept in other tables to
link data are **foreign keys**.

Besides the command line, a **GUI client** can manage connections to the
server; for MySQL the common free one is **MySQL Workbench**.
