# NBP Recommendation System

Repository phục vụ nghiên cứu hệ thống gợi ý, hiện tập trung vào bài toán xếp
hạng khách sạn theo phiên của **Trivago RecSys Challenge 2019**.

## Nội dung chính

- [`notebooks/trivago_recsys_eda.ipynb`](notebooks/trivago_recsys_eda.ipynb):
  notebook EDA chính, gồm kiểm tra chất lượng dữ liệu, session, clickout,
  candidate, giá, metadata, cold-start và các thống kê phục vụ lựa chọn mô hình.
- [`docs/Week1/Slide_Week1.pdf`](docs/Week1/Slide_Week1.pdf): slide tuần 1.
- [`docs/Week2/Slide_Week2.pdf`](docs/Week2/Slide_Week2.pdf): slide tuần 2 về
  Trivago EDA và định hướng mô hình.
- [`docs/Week2/Paper/MLEUP.pdf`](docs/Week2/Paper/MLEUP.pdf): bài báo tham khảo
  MLEUP.
- `src/vsf_recommendation_system/`: mã nguồn Python của dự án.

## Cấu trúc repository

```text
NBP_RecommendationSystem/
├── docs/
│   ├── Week1/Slide_Week1.pdf
│   └── Week2/
│       ├── Slide_Week2.pdf
│       └── Paper/MLEUP.pdf
├── notebooks/
│   └── trivago_recsys_eda.ipynb
├── sample_data/
│   └── Trivago_RecSys_Challenge_2019/raw/  # CSV được quản lý bằng Git LFS
├── src/vsf_recommendation_system/
├── pyproject.toml
└── README.md
```

## Cài đặt

Yêu cầu Python 3.11 trở lên.

```bash
python -m pip install -e ".[eda]"
```

## Chuẩn bị dữ liệu Trivago

Bốn file CSV raw có tổng dung lượng khoảng 2,74 GiB và được quản lý bằng
**Git Large File Storage (Git LFS)**. Sau khi clone repository, tải dữ liệu bằng:

```bash
git lfs install
git lfs pull
```

Các file nằm tại:

```text
sample_data/Trivago_RecSys_Challenge_2019/raw/
├── train.csv
├── test.csv
├── item_metadata.csv
└── submission_popular.csv
```

Notebook EDA chỉ đọc `train.csv`, `test.csv` và `item_metadata.csv`;
`submission_popular.csv` được lưu cùng bộ dữ liệu nhưng không được dùng để tính
các thống kê EDA.

Có thể đặt dữ liệu ở vị trí khác bằng biến môi trường `TRIVAGO_DATA_ROOT`.
Giá trị biến phải trỏ tới thư mục chứa thư mục con `raw/`.

Ví dụ trên PowerShell:

```powershell
$env:TRIVAGO_DATA_ROOT = "D:\duong-dan\Trivago_RecSys_Challenge_2019"
```

## Chạy notebook EDA

```bash
jupyter lab notebooks/trivago_recsys_eda.ipynb
```

Để kiểm tra toàn bộ kết quả một cách nhất quán, chọn **Restart Kernel** rồi
**Run All**.

Notebook hiện nạp đầy đủ `train.csv`, `test.csv` và `item_metadata.csv` vào RAM
đúng một lần. Riêng ba DataFrame sử dụng xấp xỉ 13,9 GiB RAM; các tập hợp,
bộ đếm và bảng trung gian cần thêm bộ nhớ. Máy chạy cần có lượng RAM trống phù
hợp trước khi thực thi toàn bộ notebook.

## Quy ước phân tích

- Một session được nhận diện bằng khóa kép `(user_id, session_id)`.
- `clickout item` là hành động chuyển tới trang khách sạn/đối tác, không phải
  bằng chứng đặt phòng thành công.
- `impressions` là danh sách candidate; `prices` được căn theo cùng vị trí.
- Nhãn clickout bị thiếu có chủ đích trong một phần dữ liệu test, vì vậy các
  tỷ lệ cần nhãn chỉ được kết luận trên phạm vi train phù hợp.
- Hotel ID dùng cho thống kê được chuẩn hóa về chuỗi số; giá trị văn bản không
  được tính như một khách sạn.

## Trạng thái dự án

Repository hiện cung cấp EDA và tài liệu nghiên cứu. Chưa có kết quả huấn luyện
hoặc đánh giá mô hình MLEUP trên Trivago; các kết quả trong bài báo MLEUP không
được xem là kết quả thực nghiệm của repository này.
