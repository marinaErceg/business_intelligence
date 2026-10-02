# Midwest Airbnb Listings: Data Dictionary

**Dataset:** `listings` table in `midwest_airbnb.db` (SQLite), 14,887 rows and 29 columns
**Source:** Inside Airbnb (https://insideairbnb.com/get-the-data/), the detailed `listings.csv.gz` file for each of three regions: Chicago (snapshot 2026-07-20), Columbus (snapshot 2026-07-23), and Twin Cities MSA (snapshot 2026-07-21). Column meanings follow Inside Airbnb's data dictionary and assumptions (https://insideairbnb.com/data-assumptions/).
**Course:** ISA 401, Miami University

> One row is one listing that showed a nightly price on the snapshot date; listings with no price were dropped. Empty cells are stored as SQL `NULL`.

---

## Field Definitions

| Field | Type | Description |
|---|---|---|
| `city` | text | Which Inside Airbnb region the listing came from: `Chicago` (7,439 rows), `Columbus` (2,587), or `Twin Cities` (4,861). The Twin Cities file covers the Minneapolis-St. Paul metro area, not just the two cities. |
| `snapshot_date` | text | Date Inside Airbnb compiled the file, stored as an ISO text string, not a date: `2026-07-20` for Chicago, `2026-07-23` for Columbus, `2026-07-21` for Twin Cities. Every row of a city shares the same value. |
| `id` | text | Airbnb's listing id. Unique across the table (14,887 distinct values). Stored as text even though it looks numeric, so compare it to a quoted string. |
| `name` | text | Listing title as shown on Airbnb (for example "Tiny Studio Apartment 94 Walk Score"). Never empty. |
| `price` | real | Nightly price in U.S. dollars on the snapshot date, with the dollar sign and commas removed. Ranges from 2.56 to 11,412; never `NULL` (rows without a price were dropped). |
| `room_type` | text | Airbnb's four listing categories: `Entire home/apt` (11,652 rows), `Private room` (2,951), `Hotel room` (246), or `Shared room` (38). |
| `host_id` | text | Airbnb's id for the host. One host can own many listings, so it repeats: 6,970 distinct hosts across the 14,887 listings. Stored as text, so compare it to a quoted string. Use `COUNT(DISTINCT host_id)` to count hosts instead of listings. |
| `host_name` | text | The host's display name as shown on Airbnb (for example "Rebecca"). Sometimes a business name. `NULL` for 25 rows. |
| `host_since` | text | Intended to be the date the host joined Airbnb, but it is `NULL` in every row of this table. Do not filter, group, or sort by it, and tell the user it is not available if they ask about host tenure. |
| `host_is_superhost` | text | Whether the host is an Airbnb Superhost. The values are the text letters `'t'` (7,982 rows) and `'f'` (6,880), not true/false booleans. `NULL` for 25 rows, which should be left out of superhost comparisons. |
| `neighbourhood` | text | Inside Airbnb's `neighbourhood_cleansed`: the neighborhood or community area that the listing's coordinates fall in (for example `Hyde Park`, `West Town`, `Lincoln Park`). 119 distinct values across the three regions, and the names differ by city, so filter on `city` when comparing neighborhoods. |
| `latitude` | real | Latitude in decimal degrees (WGS84), from 39.88 to 46.24. Airbnb shows an approximate location, so a point can be slightly off the real address. |
| `longitude` | real | Longitude in decimal degrees (WGS84), from -94.53 to -82.78. Values are negative because the data is in the western hemisphere. |
| `property_type` | text | Airbnb's more specific description of the place, with 62 distinct values. Most common: `Entire rental unit` (5,581), `Entire home` (3,805), `Private room in home` (1,441), `Entire condo` (780), `Private room in rental unit` (716). Use `LIKE` to group them, for example `property_type LIKE 'Entire%'`. It is finer than `room_type`. |
| `accommodates` | integer | Maximum number of guests the listing can host, from 1 to 16. For "can host a party of ten" use `accommodates >= 10`. Never `NULL`. |
| `bedrooms` | real | Number of bedrooms, from 1 to 16 (stored as decimals such as `2.0`). There are no zeros, and it is `NULL` for 2,976 listings, so a bedroom filter silently drops those rows. |
| `beds` | real | Number of beds, from 1 to 32. `NULL` for 668 rows. |
| `bathrooms_text` | text | Bathroom count written as text, not a number, for example `1 bath`, `1.5 baths`, `1 shared bath`, `1 private bath`, `Shared half-bath`, `0 baths`. 32 distinct values, `NULL` for 71 rows. Match with `LIKE` (for example `LIKE '%shared%'`) instead of comparing numbers. |
| `minimum_nights` | integer | Fewest nights a guest can book in one stay, from 1 to 365. Large values usually mean monthly or long-term stays. `NULL` for 15 rows. |
| `availability_365` | integer | Number of nights the listing is open for booking in the 365 days after the snapshot date, from 0 to 365. A 0 means it has no open nights (blocked or booked), not that the listing is missing. Never `NULL`. |
| `number_of_reviews` | integer | Total reviews the listing has received over its lifetime, from 0 to 2,246. Never `NULL`. |
| `number_of_reviews_ltm` | integer | Reviews received in the last twelve months before the snapshot, from 0 to 1,220. A rough sign of recent booking activity. Never `NULL`. |
| `first_review` | text | Date of the listing's first review as an ISO text string (`YYYY-MM-DD`), from 2009-07-03 to 2026-07-20. `NULL` for the 1,761 listings with no reviews. |
| `last_review` | text | Date of the listing's most recent review as an ISO text string (`YYYY-MM-DD`), from 2014-08-23 to 2026-07-22. `NULL` for the same 1,761 listings with no reviews. |
| `review_scores_rating` | real | Overall guest rating on a 1 to 5 scale (for example 4.99). `NULL` for the 1,761 listings with no reviews; those are unrated, not zero, and averages ignore them. |
| `reviews_per_month` | real | Average number of reviews the listing gets per month, from 0.01 to 77.72. `NULL` for the 1,761 listings with no reviews. |
| `instant_bookable` | text | Intended to show whether guests can book without host approval (`'t'` or `'f'`), but it is `NULL` in every row of this table. Do not use it, and tell the user it is not available if they ask about instant booking. |
| `estimated_revenue_l365d` | real | Inside Airbnb's estimate of the listing's revenue in dollars over the 365 days before the snapshot, from 0 to 1,114,800. It is a model-based estimate (estimated booked nights times price), not the host's actual income. A 0 means no estimated bookings. Never `NULL`. |
| `amenities_count` | integer | Number of amenities in the listing's amenities list, from 0 to 100. This column is not from Inside Airbnb; it was computed for this course. Never `NULL`. |

---

## Notes for Writing Queries

- `host_since` and `instant_bookable` are `NULL` in every row. Do not use them; say they are unavailable.
- `host_is_superhost` holds the text letters `'t'` and `'f'`.
- `id`, `host_id`, and the date columns (`snapshot_date`, `first_review`, `last_review`) are stored as text.
- Reviews and ratings are `NULL` for the 1,761 listings that have no reviews. Do not treat them as zero.
- `bedrooms`, `beds`, and `bathrooms_text` have missing values, so filters on them drop some listings.
- Always compare neighborhoods within a single `city`, because names differ across the three regions.
