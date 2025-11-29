# Music Store SQL Analytics Project

This project explores a fictional **digital music store database** using SQL to uncover insights related to customer behavior, product performance, and revenue growth. The dataset includes **customers, invoices, tracks, albums, genres, and artists**, enabling both transactional and analytical queries.

---

## Objectives
- Analyze purchasing trends and customer segments
- Identify top-performing artists, genres, products, and countries
- Measure revenue growth and seasonality
- Explore customer behavior patterns such as first-purchase funnel and repeat purchase rates

---

## Tools & Technologies
| Component | Details |
|----------|---------|
| Language | SQL |
| Skills Used | Joins, CTEs, Window Functions, Aggregations, Subqueries |

---

## Key Business Questions Answered

| Category | Sample Analysis |
|---------|------------------|
| Product Insights | Top 10 most expensive tracks, best-selling artists |
| Customer Insights | Top revenue-generating customers, entry genre funnel |
| Geography | Revenue by country, revenue leakage regions |
| Seasonality | Monthly revenue trend over multiple years |
| Customer Retention | Repeat purchase rate |

---

## Highlight Queries

### Top 3 artists by revenue in every genre
```sql
WITH genre_artist_revenue AS (
  SELECT
      g.name AS genre,
      art.name AS artist,
      ROUND(SUM(i.total), 2) AS total_revenue,
      DENSE_RANK() OVER(PARTITION BY g.name ORDER BY SUM(i.total) DESC) AS ranked
  FROM music.invoice AS i
  JOIN music.invoice_line AS il ON i.invoice_id = il.invoice_id
  JOIN music.track AS t ON il.track_id = t.track_id
  JOIN music.album AS a ON t.album_id = a.album_id
  JOIN music.artist AS art ON a.artist_id = art.artist_id
  JOIN music.genre AS g ON g.genre_id = t.genre_id
  GROUP BY 1, 2
)
SELECT genre, artist, total_revenue
FROM genre_artist_revenue
WHERE ranked <= 3
ORDER BY 1, ranked;


**Interactive Visualization:**  
[![Music Store Sales Dashboard](https://public.tableau.com/static/images/Mu/MusicStoreSalesDashboard_17644350342060/Dashboard1/1.png)](https://public.tableau.com/views/MusicStoreSalesDashboard_17644350342060/Dashboard1?:language=en-US&:sid=&:redirect=auth&:display_count=n&:origin=viz_share_link) [web:24]



