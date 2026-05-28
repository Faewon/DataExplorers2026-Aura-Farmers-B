# DataExplorers 2026 – Hạng mục B: Business Intelligence Dashboard

Hệ thống phân tích kinh doanh đa chiều cho **Thống Nhất Bike** – doanh nghiệp sản xuất & phân phối xe đạp B2B với 200+ SKU và 798 đại lý toàn quốc.

---

## Tổng quan

| Chỉ số | Giá trị |
|---|---|
| Thời gian dữ liệu | T1/2025 – T3/2026 |
| Tổng doanh thu | 109 tỷ đồng |
| Số đơn hàng | 3,000+ |
| Sản phẩm bán | 72,000 |
| Đại lý hoạt động | 807 |
| Đơn T3/2026 (từ HM-A) | 1,132 – xử lý thành công 100% |

---

## Cấu trúc repo

```
├── dashboard/
│   ├── PowerBIDashboard.pbix          # File Power BI chính
│   ├── Tổng Quan Kinh Doanh.jpg       # Dashboard 1
│   ├── Phân Tích Thời Gian.jpg        # Dashboard 2
│   ├── Phân Tích Sản Phẩm.jpg         # Dashboard 3
│   ├── Phân Tích Đại Lý.jpg           # Dashboard 4
│   ├── Phân Tích Địa Lý.jpg           # Dashboard 5
│   └── Trạng Thái Vận Hành.jpg        # Dashboard 6
│
├── data/
│   ├── gold_fact_sales.csv            # [GOLD] Bảng tích hợp chính – 25,754 dòng
│   ├── silver_customer_geo.csv        # [SILVER] Khách hàng đã chuẩn hóa địa lý
│   ├── silver_province.csv            # [SILVER] Bảng tỉnh/thành chuẩn hóa
│   ├── fact_sales.csv                 # [BRONZE] Giao dịch bán hàng thô
│   ├── sales_order.csv                # [BRONZE] Đơn hàng
│   ├── order_line.csv                 # [BRONZE] Chi tiết dòng đơn hàng
│   ├── customer.csv                   # [BRONZE] Thông tin đại lý/khách hàng
│   ├── product.csv                    # [BRONZE] Danh mục sản phẩm
│   ├── product_line.csv               # [BRONZE] Dòng sản phẩm
│   ├── product_group.csv              # [BRONZE] Nhóm sản phẩm
│   ├── product_price.csv              # [BRONZE] Bảng giá
│   ├── province.csv                   # [BRONZE] Tỉnh/thành thô
│   └── email_log.csv                  # Log xử lý email T3/2026 (Dashboard 6)
│
├── docs/
│   └── BaoCaoKyThuat_HangMucB.md      # Báo cáo kỹ thuật đầy đủ
│
└── README.md
```

---

## Kiến trúc dữ liệu – Medallion 3 tầng

```
Bronze  →  Silver  →  Gold
 (thô)    (chuẩn hóa)  (tích hợp)
                         ↓
                  gold_fact_sales
                  (nguồn duy nhất
                   cho 6 dashboard)
```

- **Bronze:** Dữ liệu thô từ PostgreSQL – `sales_order`, `order_line`, `customer`, `product`, `product_line`, `product_group`
- **Silver:** Chuẩn hóa địa lý – `silver_province`, `silver_customer_geo`
- **Gold:** Bảng denormalized tích hợp – `gold_fact_sales` (25,754 dòng)

---

## 6 Dashboard

| # | Dashboard | Câu hỏi kinh doanh |
|---|---|---|
| 1 | Tổng Quan Kinh Doanh | Tình hình tổng thể? Trạng thái pipeline T3/2026? |
| 2 | Phân Tích Thời Gian | Xu hướng doanh số? Mùa cao điểm? YoY Q1? |
| 3 | Phân Tích Sản Phẩm | Nhóm nào dẫn đầu? Stars/Dogs là ai? Màu sắc nào thống trị? |
| 4 | Phân Tích Đại Lý | RFM phân khúc thế nào? 20% đại lý lớn chiếm bao nhiêu? |
| 5 | Phân Tích Địa Lý | Thị trường nào lớn nhất? Tỉnh nào tăng/giảm? |
| 6 | Trạng Thái Vận Hành | Pipeline xử lý email T3/2026 hoạt động thế nào? |

---

## Insights nổi bật

**1. Mùa vụ tháng 3 có thể dự báo được** — T3/2026 đạt 40.8 tỷ (+110% MoM). Pattern lặp lại cả 2 năm, đỉnh tập trung tuần 10–13.

**2. Rủi ro tập trung doanh thu ở mức báo động** — Top 20% (~159/798) đại lý chiếm ~68% doanh thu. 280 đại lý At Risk (34.7%) có thể ảnh hưởng 15–20% doanh thu nếu rời bỏ.

**3. Thị trường miền Nam bị bỏ ngỏ** — Miền Nam chỉ 5.5% (~6 tỷ). TP.HCM đứng thứ 7 với ~5.8 tỷ dù là thành phố đông dân nhất.

**4. Xe thể thao mất thị phần** — Nhôm -65%, thép -38% YoY trong khi tổng thị trường +120%. BCG xếp cả 2 vào Dogs.

**5. Chất lượng đại lý mới 2026 cải thiện đáng kể** — Cohort 2026-T1: retention T+2 đạt 74.5% so với 45.7% của 2025-T1.

**6. Quảng Ngãi tăng ~11x** — Tăng trưởng YoY cao nhất toàn hệ thống, cơ hội nhân rộng sang Bình Định, Quảng Nam.

---

## KPI mục tiêu Q2/2026

| KPI | Thực tế Q1/2026 | Mục tiêu Q2/2026 |
|---|---|---|
| Doanh thu | 109 tỷ | 120 tỷ |
| Sản lượng | 72K | 80K |
| Tỷ trọng xe phổ thông | 70.15% | ≤68% |
| Đại lý At Risk | 280 | ≤250 |
| Tỷ lệ xử lý email | 100% | ≥98% |

---

## Công nghệ

- **Power BI** – Visualization & DAX
- **PostgreSQL** – Nguồn dữ liệu gốc
- **Medallion Architecture** – Tổ chức data pipeline
- **RFM Analysis** – Xây dựng thuần DAX, không dùng Python
- **Cohort Analysis** – Xây dựng thuần DAX
