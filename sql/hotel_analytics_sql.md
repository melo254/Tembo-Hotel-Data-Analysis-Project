# Tembo Hotel & Suites — Business Insights SQL

## 1. Revenue Analysis

### Total Revenue by Month
```sql
SELECT
    DATE_TRUNC('month', check_in_date)::DATE AS month,
    SUM(total_amount) AS total_revenue
FROM tembo_hotel.clean_bookings
WHERE check_in_date IS NOT NULL
  AND total_amount IS NOT NULL
GROUP BY DATE_TRUNC('month', check_in_date)
ORDER BY month;
```

### Total Revenue by Room Type
```sql
SELECT
    room_type,
    SUM(total_amount) AS total_revenue
FROM tembo_hotel.clean_bookings
WHERE total_amount IS NOT NULL
GROUP BY room_type
ORDER BY total_revenue DESC;
```

### Total Revenue by Payment Method
```sql
SELECT
    payment_method,
    SUM(total_amount) AS total_revenue
FROM tembo_hotel.clean_bookings
WHERE total_amount IS NOT NULL
GROUP BY payment_method
ORDER BY total_revenue DESC;
```

## 2. Occupancy Analysis

### Most-Booked Room Types
```sql
SELECT
    room_type,
    COUNT(*) AS total_bookings
FROM tembo_hotel.clean_bookings
GROUP BY room_type
ORDER BY total_bookings DESC;
```

### Average Nights Stayed by Room Type
```sql
SELECT
    room_type,
    ROUND(AVG(nights_stayed), 2) AS average_nights
FROM tembo_hotel.clean_bookings
WHERE nights_stayed IS NOT NULL
GROUP BY room_type
ORDER BY average_nights DESC;
```

## 3. Guest Insights

### Top 10 Guest Cities
```sql
SELECT
    guest_city,
    COUNT(*) AS total_bookings
FROM tembo_hotel.clean_bookings
WHERE guest_city IS NOT NULL
GROUP BY guest_city
ORDER BY total_bookings DESC
LIMIT 10;
```

### Average Guest Rating by Room Type
```sql
SELECT
    room_type,
    ROUND(AVG(guest_rating), 2) AS average_rating
FROM tembo_hotel.clean_bookings
WHERE guest_rating IS NOT NULL
GROUP BY room_type
ORDER BY average_rating DESC;
```

## 4. Staff Performance

### Most Bookings Handled by Staff
```sql
SELECT
    staff_name,
    COUNT(*) AS bookings_handled
FROM tembo_hotel.clean_bookings
GROUP BY staff_name
ORDER BY bookings_handled DESC;
```

### Revenue by Staff Department
```sql
SELECT
    staff_department,
    SUM(total_amount) AS total_revenue
FROM tembo_hotel.clean_bookings
WHERE total_amount IS NOT NULL
GROUP BY staff_department
ORDER BY total_revenue DESC;
```

## 5. Trends Analysis

### Month-over-Month Revenue Growth
```sql
WITH monthly_revenue AS (
    SELECT
        DATE_TRUNC('month', check_in_date)::DATE AS month,
        SUM(total_amount) AS revenue
    FROM tembo_hotel.clean_bookings
    WHERE check_in_date IS NOT NULL
      AND total_amount IS NOT NULL
    GROUP BY DATE_TRUNC('month', check_in_date)
)
SELECT
    month,
    revenue,
    LAG(revenue) OVER (ORDER BY month) AS previous_month_revenue,
    ROUND(
        (revenue - LAG(revenue) OVER (ORDER BY month))
        / NULLIF(LAG(revenue) OVER (ORDER BY month), 0) * 100,
        2
    ) AS mom_growth_percentage
FROM monthly_revenue
ORDER BY month;
```

### Busiest and Quietest Months
```sql
SELECT
    DATE_TRUNC('month', check_in_date)::DATE AS month,
    COUNT(*) AS total_bookings
FROM tembo_hotel.clean_bookings
WHERE check_in_date IS NOT NULL
GROUP BY DATE_TRUNC('month', check_in_date)
ORDER BY total_bookings DESC;
```

## 6. Cancellation Analysis

### Cancellation Rate by Room Type
```sql
SELECT
    room_type,
    COUNT(*) AS total_bookings,
    COUNT(*) FILTER (WHERE booking_status = 'Cancelled') AS cancelled_bookings,
    ROUND(
        COUNT(*) FILTER (WHERE booking_status = 'Cancelled')::NUMERIC
        / NULLIF(COUNT(*), 0) * 100,
        2
    ) AS cancellation_rate_percentage
FROM tembo_hotel.clean_bookings
GROUP BY room_type
ORDER BY cancellation_rate_percentage DESC;
```

### Recorded Booking Value Associated with Cancellations and No-Shows
```sql
SELECT
    booking_status,
    COUNT(*) AS affected_bookings,
    SUM(total_amount) AS recorded_booking_value
FROM tembo_hotel.clean_bookings
WHERE booking_status IN ('Cancelled', 'No Show')
  AND total_amount IS NOT NULL
GROUP BY booking_status
ORDER BY recorded_booking_value DESC;
```

### Total Recorded Booking Value Associated with Cancellations and No-Shows
```sql
SELECT
    COUNT(*) AS cancelled_or_no_show_bookings,
    SUM(total_amount) AS total_recorded_booking_value
FROM tembo_hotel.clean_bookings
WHERE booking_status IN ('Cancelled', 'No Show')
  AND total_amount IS NOT NULL;
```

