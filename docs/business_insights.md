# Tembo Hotel & Suites - Business Analysis & Insights

> **A PostgreSQL-driven analysis of hotel bookings, revenue, occupancy, guest behaviour, staff performance, and cancellations.**

---

## Project Overview

This project analyzes the booking dataset of **Tembo Hotel & Suites** to identify patterns in:

* Revenue performance
* Room demand and occupancy
* Guest behaviour
* Staff booking activity
* Revenue trends
* Cancellations and no-shows

The objective is to transform operational booking data into **actionable business insights** that can support commercial, operational, and customer-experience decisions.

The analysis was conducted using **PostgreSQL** on a cleaned hotel booking dataset and was structured around six core business questions defined in the project brief.

---

# Executive Summary

| KPI                               |                                 Key Result |
| --------------------------------- | -----------------------------------------: |
| Highest Monthly Revenue           |               **KES 803,000 — March 2024** |
| Highest Booking Volume            |                **Standard — 114 bookings** |
| Highest Revenue Room Type         |                  **Suite — KES 2,731,100** |
| Longest Average Stay              |                    **Suite — 3.22 nights** |
| Top Guest City                    |                 **Nairobi — 129 bookings** |
| Busiest Months                    | **June & October 2024 — 24 bookings each** |
| Highest Cancellation Rate         |                     **Penthouse — 20.00%** |
| Cancelled & No-Show Booking Value |                          **KES 1,175,300** |

---

# 1. Revenue Analysis

### Business Questions

* What is the total revenue by month?
* Which room types generate the most revenue?
* Which payment methods generate the most revenue?

## Monthly Revenue

Revenue varied considerably throughout the analysis period, with a substantial increase from late 2023 into early 2024.

**Key observations:**

* **March 2024** recorded the highest monthly revenue at **KES 803,000**.
* **October 2023** recorded the lowest monthly revenue at **KES 119,500**.
* December 2023 recorded a substantial increase compared with November 2023.
* Revenue declined notably in **May and August 2024**, followed by subsequent recoveries.

## Revenue by Room Type

| Room Type | Total Revenue (KES) |
| --------- | ------------------: |
| Suite     |       **2,731,100** |
| Deluxe    |           2,406,700 |
| Standard  |           1,948,800 |
| Penthouse |           1,792,600 |

### Key Insight

**Suites generated the highest total recorded revenue at KES 2,731,100.**

However, total revenue should be interpreted alongside booking volume, room pricing, and length of stay because revenue is influenced by both **number of bookings and booking duration/value**.

## Revenue by Payment Method

| Payment Method | Total Revenue (KES) |
| -------------- | ------------------: |
| Bank Transfer  |       **2,466,500** |
| Cash           |           2,309,500 |
| M-Pesa         |           2,108,700 |
| Card           |           1,994,500 |

**Bank Transfer** accounted for the highest recorded revenue, while **Card** recorded the lowest.

---

# 2. Occupancy & Booking Analysis

### Business Questions

* Which room types are booked most frequently?
* What is the average number of nights stayed by room type?

## Bookings by Room Type

| Room Type | Bookings |
| --------- | -------: |
| Standard  |  **114** |
| Deluxe    |       90 |
| Suite     |       56 |
| Penthouse |       25 |

**Standard rooms were the most frequently booked**, with 114 bookings.

## Average Nights Stayed

| Room Type | Average Nights |
| --------- | -------------: |
| Suite     |       **3.22** |
| Deluxe    |           3.02 |
| Standard  |           2.95 |
| Penthouse |           2.80 |

**Suites recorded the longest average stay at 3.22 nights**, while Penthouse bookings had the shortest average stay at 2.80 nights.

### Key Business Insight

There is a clear distinction between **demand and revenue performance**:

> **Standard rooms had the highest booking volume, while Suites generated the highest total revenue.**

This suggests that booking frequency alone does not determine revenue contribution.

---

# 3. Guest Insights

### Business Questions

* Which guest cities generate the most bookings?
* What is the average guest rating by room type?

## Bookings by Guest City

The cleaned dataset contained nine non-null guest cities.

| Guest City | Bookings |
| ---------- | -------: |
| Nairobi    |  **129** |
| Kisumu     |       29 |
| Eldoret    |       28 |
| Mombasa    |       15 |
| Meru       |       14 |
| Nyeri      |       14 |
| Nakuru     |       14 |
| Machakos   |       14 |
| Thika      |       14 |

**Nairobi was the dominant guest market**, accounting for the largest number of bookings in the dataset.

## Average Rating by Room Type

| Room Type | Average Rating |
| --------- | -------------: |
| Standard  |       **3.13** |
| Penthouse |       **3.13** |
| Deluxe    |           2.94 |
| Suite     |           2.88 |

Standard and Penthouse rooms recorded the highest average rating at **3.13**, while Suites recorded the lowest at **2.88**.

### Key Observation

Suites generated the highest total revenue and had the longest average stay, but also recorded the lowest average guest rating.

> **Important:** This is a descriptive relationship in the dataset and does **not establish causation** between stay duration, revenue, and guest satisfaction.

---

# 4. Staff Performance

### Business Questions

* Which staff members handle the most bookings?
* Which department is associated with the highest recorded revenue?

## Bookings Handled by Staff

| Staff Member   | Bookings Handled |
| -------------- | ---------------: |
| Kelvin Omondi  |           **38** |
| Moses Kipchoge |               36 |
| Fatuma Hassan  |               36 |
| Tony Karanja   |               36 |
| Brenda Achieng |               36 |
| Amina Juma     |               35 |
| Joy Otieno     |               34 |
| Peter Ngugi    |               34 |

**Kelvin Omondi handled the highest number of bookings, with 38.**

The difference between staff members was relatively small, with booking volumes ranging from **34 to 38 bookings**.

## Revenue by Department

| Department   | Recorded Revenue (KES) |
| ------------ | ---------------------: |
| Front Desk   |          **2,357,500** |
| Housekeeping |              2,046,200 |
| Restaurant   |              2,035,200 |
| Management   |              1,370,000 |
| Security     |              1,070,300 |

**Front Desk was associated with the highest recorded revenue at KES 2,357,500.**

> **Note:** Department revenue should not be interpreted as revenue personally generated by individual employees. It represents the recorded booking value associated with records assigned to each department.

---

# 5. Revenue Trends

### Business Questions

* What is the month-over-month revenue growth?
* Which months are busiest and quietest?

## Month-over-Month Revenue Growth

| Period         | Revenue Movement |
| -------------- | ---------------: |
| December 2023  |     **+104.59%** |
| January 2024   |      **+65.93%** |
| May 2024       |      **-36.93%** |
| September 2024 |      **+56.13%** |
| June 2024      |      **+53.42%** |

The analysis shows **significant month-to-month revenue volatility**.

March 2024 generated the highest monthly revenue at **KES 803,000**, but its month-over-month growth was only **10.68%**.

### Key Insight

> **The month with the highest revenue is not necessarily the month with the highest growth rate.**

This distinction is important when evaluating hotel performance because revenue size and revenue momentum measure different aspects of business performance.

## Busiest and Quietest Months

### Busiest Months

| Month        | Bookings |
| ------------ | -------: |
| June 2024    |   **24** |
| October 2024 |   **24** |

### Quietest Months

| Month        | Bookings |
| ------------ | -------: |
| June 2023    |        4 |
| August 2023  |        4 |
| October 2023 |        4 |

Booking activity was substantially higher throughout **2024** than during the earlier months represented in the dataset.

---

# 6. Cancellation Analysis

### Business Questions

* What is the cancellation rate for each room type?
* What booking value is associated with cancellations and no-shows?

## Cancellation Rate by Room Type

| Room Type | Total Bookings | Cancelled | Cancellation Rate |
| --------- | -------------: | --------: | ----------------: |
| Penthouse |             25 |         5 |        **20.00%** |
| Standard  |            114 |         9 |             7.89% |
| Deluxe    |             90 |         7 |             7.78% |
| Suite     |             56 |         2 |         **3.57%** |

**Penthouse recorded the highest cancellation rate at 20.00%**, while Suite recorded the lowest at 3.57%.

Although Standard had the highest number of cancelled bookings, Penthouse had the higher **cancellation rate** because it had substantially fewer total bookings.

## Cancellation & No-Show Booking Value

| Booking Status | Affected Bookings | Recorded Booking Value (KES) |
| -------------- | ----------------: | ---------------------------: |
| Cancelled      |                23 |                      910,500 |
| No Show        |                 9 |                      264,800 |
| **Total**      |            **32** |                **1,175,300** |

There were **32 cancelled or no-show bookings**, representing **KES 1,175,300 in recorded booking value**.

> **Note:** This figure represents the recorded booking value associated with cancelled and no-show reservations. It should **not be interpreted as confirmed financial loss** because the dataset does not indicate whether these amounts were actually forfeited by the hotel.

---

# Overall Business Insights

The analysis identified several important patterns across Tembo Hotel & Suites.

### Revenue

* **Suites generated the highest total recorded revenue:** KES 2,731,100.
* **March 2024 recorded the highest monthly revenue:** KES 803,000.
* **Bank Transfer** recorded the highest revenue among payment methods at KES 2,466,500.
* Revenue demonstrated substantial month-to-month volatility.

### Occupancy & Demand

* **Standard rooms had the highest booking volume:** 114 bookings.
* **Suites had the longest average stay:** 3.22 nights.
* June and October 2024 were the busiest months, with **24 bookings each**.

### Guests

* **Nairobi was the dominant guest market:** 129 bookings.
* Standard and Penthouse rooms had the highest average guest rating at **3.13**.
* Suites had the lowest average rating at **2.88** despite generating the highest total revenue.

### Staff

* **Kelvin Omondi handled the most bookings:** 38.
* Front Desk was associated with the highest recorded departmental revenue: **KES 2,357,500**.

### Cancellations

* **Penthouse had the highest cancellation rate:** 20.00%.
* There were **23 cancellations and 9 no-shows**.
* Cancelled and no-show reservations represented **KES 1,175,300 in recorded booking value**.

---

# Analytical Considerations

This analysis describes **observed patterns in the cleaned booking dataset** and does not establish causal relationships.

### Data Quality

Two records — **BK9002** and **BK9003** — contain invalid date sequences where the check-out date occurs before the check-in date.

These records should be excluded from analyses that depend on valid stay durations or date intervals.

### Missing Values

Missing values were retained where the correct value could not be reliably determined from the available data.

For example:

* **BK9003** retains a `NULL` value for `nights_stayed`.
* **BK9003** retains a `NULL` value for `total_amount`.

### Cancellation & No-Show Interpretation

The reported **KES 1,175,300** represents the recorded booking value associated with cancelled and no-show reservations.

It should **not be treated as confirmed revenue loss**, because the available dataset does not provide sufficient information to determine whether these booking amounts were actually forfeited by the hotel.

---

# Tools & Technologies

* **PostgreSQL** — Data querying and business analysis
* **SQL** — Data transformation, aggregation, and analysis
* **Data Cleaning** — Preparation and validation of booking records
* **Business Analytics** — Revenue, occupancy, guest, staff, and cancellation analysis

---

# Business Value

This analysis demonstrates how hotel booking data can be transformed into meaningful business insights across:

**Revenue → Demand → Guests → Operations → Trends → Risk**

The findings provide a foundation for further analysis such as:

* Revenue forecasting
* Occupancy forecasting
* Customer segmentation
* Cancellation prediction
* Room pricing analysis
* Guest satisfaction analysis
* Seasonal demand analysis
* Hotel performance dashboards

---

# Project Focus

**Data Analytics | SQL | PostgreSQL | Business Intelligence | Hospitality Analytics**

> *Turning hotel booking data into clear, measurable business insights.*
