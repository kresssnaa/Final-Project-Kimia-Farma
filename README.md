# Final-Project-Kimia-Farma
final tanks data analyst from kimia farma


SELECT
  t.transaction_id,
  t.date,
  t.branch_id,
  kc.branch_name,
  kc.kota,
  kc.provinsi,
  kc.rating AS rating_cabang,
  t.customer_name,
  t.product_id,
  p.product_name,
  p.product_category,
  p.price AS actual_price,
  t.discount_percentage,
  CASE
    WHEN p.price <= 50000 THEN 0.1
    WHEN p.price > 50000 AND p.price <= 100000 THEN 0.15
    WHEN p.price > 100000 AND p.price <= 300000 THEN 0.2
    WHEN p.price > 300000 AND p.price <= 500000 THEN 0.25
    ELSE 0.3
  END AS persentase_gross_laba,
  p.price * (1 - t.discount_percentage) AS nett_sales,
  (p.price * (1 - t.discount_percentage)) *
  CASE
    WHEN p.price <= 50000 THEN 0.1
    WHEN p.price > 50000 AND p.price <= 100000 THEN 0.15
    WHEN p.price > 100000 AND p.price <= 300000 THEN 0.2
    WHEN p.price > 300000 AND p.price <= 500000 THEN 0.25
    ELSE 0.3
  END AS nett_profit,
  t.rating AS rating_transaksi
FROM
  `kimia_farma.transaction` AS t
  INNER JOIN
  `kimia_farma.product` AS p
  ON t.product_id = p.product_id
  INNER JOIN
  `kimia_farma.kantor_cabang`
  AS kc
  ON t.branch_id = kc.branch_id
