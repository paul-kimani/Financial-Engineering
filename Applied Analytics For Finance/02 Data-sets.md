To feed an algorithmic trading system, you must first understand the shape and nature of your inputs. Alphanumeric datasets are collections of items (also called rows, records, or observations) where each item has specific characteristics (attributes or features).

### The anatomy of Datasets.
- **Size vs. Dimensionality:** Size is simply the number of rows, while dimensionality is the number of attributes (columns).
- **Data Structures:**
	- **Structured:** Every record has the same exact attributes(homogenous). think of standard tabular price data.
	- **Semi-Structured:** Records can have different schemas with optional attributes (heterogenous). Common formats are XML and JSON, which you frequently encounter when pulling market data from Web APIs.
	- **Unstructured:** Data without explicit tags seperating records, like financial news text or central bank statements.
- **Attribute Domains:**
	- **Categorical(Nominal):** Finite labels with no mathematical order(e.g.., asset classes like "FOREX" or "Commodities").
	- **Ordinal:** Labels with a logical ranking (e.g .., bond ratings from AAA to C).
	- **Numerical:** True quantitative measurements. Interval data has an arbitrary zero(you can add/subtract but ratios don't make sense), while ratio data has an absolute zero

#### Relational Data.

When structuring historical market data for a backtesting engine or trading database, relational databases (managed via SQL) are the standard way to organize and link tabular data.
- **Databases and Tables:** A database is the overarching repository(`CREATE DATABASE`). Which houses interconnected tables (`CREATE TABLE`). Each table has a defined schema detailing its columns and datatypes.
- **Data Types:** SQL requires strict typing. You will define columns as strings(categorical/ordinal data), Numbers(integers or floats for prices/returns), Temporal (timestamps for execution log), or booleans.
- **Keys(The Glue):** Keys are the minimal set of attributes needed to uniquely identify a specific row. A primary key identifies the record, while a foreign key links that record to data in another table (e.g ., linking a trade execution table to a separate ticker table to maintain data connections.)