## Quy trình xử lý dữ liệu bản đồ 

Phần này hướng dẫn chi tiết cách trích xuất, lọc và chuẩn hóa dữ liệu từ OpenStreetMap (OSM) để phục vụ thuật toán tìm đường A*.

---

### 1. Công cụ yêu cầu
* **Osmium-tool**: Xử lý cắt và lọc file `.pbf`.
* **OSMnx**: Thư viện Python dùng để nạp dữ liệu XML thành đồ thị NetworkX.

---

### 2. Các câu lệnh xử lý dữ liệu

Thực hiện các bước sau để tối ưu hóa dung lượng file bản đồ trước khi đưa vào mã nguồn:

**Bước 1: Cắt vùng bản đồ mục tiêu (Bounding Box)**
Xác định tọa độ khu vực (MinLon, MinLat, MaxLon, MaxLat) để giảm tải cho hệ thống.
```bash
osmium extract -b 105.70,21.03,105.76,21.08 vietnam-latest.osm.pbf -o targeted_area.osm.pbf 
```

**Bước 2: Lọc dữ liệu đường bộ (Highway Filtering)
Chỉ giữ lại các đối tượng là đường giao thông, loại bỏ các dữ liệu rác như cây cối, tòa nhà.**
``` bash
osmium tags-filter targeted_area.osm.pbf w/highway -o roads_only.osm.pbf 
```
**Bước 3: Chuyển đổi định dạng sang XML
Chuyển file nhị phân sang định dạng XML để thư viện Python có thể đọc trực tiếp.**

``` bash
osmium cat roads_only.osm.pbf -o map_data.osm
```
