# Đồ Án Cuối Kỳ: Thuật Toán Tìm Đường Tối Ưu

[![Python](https://img.shields.io/badge/Python-3.8+-blue.svg)](https://www.python.org/)
[![Jupyter](https://img.shields.io/badge/Jupyter-Notebook-orange.svg)](https://jupyter.org/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

> **Môn học:** Lập Trình Phân Tích Dữ Liệu  
> **Trường:** Đại học Kinh tế TP.HCM (UEH)  

---

## 📋 Mục Lục

- [Giới Thiệu](#-giới-thiệu)
- [Cấu Trúc Dự Án](#-cấu-trúc-dự-án)
- [Thuật Toán A*](#-thuật-toán-a)
- [Thuật Toán Dijkstra](#-thuật-toán-dijkstra)
- [Cài Đặt](#-cài-đặt)
- [Sử Dụng](#-sử-dụng)
- [Kết Quả](#-kết-quả)
- [Công Nghệ](#-công-nghệ)
- [Tác Giả](#-tác-giả)

---

## 🎯 Giới Thiệu

Dự án nghiên cứu và triển khai hai thuật toán tìm đường tối ưu phổ biến trong lĩnh vực Trí tuệ Nhân tạo và Khoa học Máy tính:

### 1. **Thuật Toán A*** - Robot Hút Bụi Thông Minh
Mô phỏng robot hút bụi tự động tìm đường đi tối ưu để dọn sạch các ô bẩn trên ma trận 2D.

**Đặc điểm:**
- ✅ Di chuyển 8 hướng (bao gồm đường chéo)
- ✅ Chi phí động: phạt theo số ô bẩn còn lại
- ✅ Heuristic thông minh: Chebyshev + MST
- ✅ So sánh với BFS, DFS, Dijkstra

### 2. **Thuật Toán Dijkstra** - Tìm Đường Ngắn Nhất Trên Đồ Thị
Giải bài toán tìm đường đi ngắn nhất từ đỉnh nguồn đến đỉnh đích trên đồ thị có trọng số.

**Đặc điểm:**
- ✅ Đồ thị vô hướng có trọng số
- ✅ Visualization đẹp mắt với matplotlib
- ✅ Debug từng bước chi tiết
- ✅ Thống kê hiệu suất

---

## 📁 Cấu Trúc Dự Án

```
Github/
│
├── Astar/                          # Thuật toán A* - Robot Hút Bụi
│   ├── Astar debug.ipynb          # Debug từng bước với bảng dữ liệu
│   ├── Astar_4_4_2.ipynb          # Demo: lưới 4×4, 2 ô bẩn
│   ├── Astar_30_30_2.ipynb        # Test: lưới 30×30, 2 ô bẩn
│   └── Astar_30_30_5.ipynb        # Test: lưới 30×30, 5 ô bẩn
│
├── Dijkstra/                       # Thuật toán Dijkstra - Đồ Thị
│   ├── Dijktra debug.ipynb        # Debug từng bước với visualization
│   ├── Dijkstra 15.ipynb          # Đồ thị 15 đỉnh
│   ├── Dijkstra 20.ipynb          # Đồ thị 20 đỉnh
│   └── Dijkstra 24.ipynb          # Đồ thị 24 đỉnh
│
└── README.md                       # File này
```

---

## 🤖 Thuật Toán A*

### Mô Tả Bài Toán

Robot hút bụi hoạt động trên ma trận `m × n` với các ô có trạng thái:
- **Free**: Ô trống, có thể đi qua
- **Start**: Vị trí bắt đầu của robot
- **Dirty**: Ô bẩn cần dọn
- **Clean**: Ô đã được dọn sạch

### Hành Động

1. **MOVE**: Di chuyển đến ô láng giềng (8 hướng)
   - Chi phí: `1 + số_ô_bẩn_còn_lại`

2. **SUCK**: Hút bụi tại vị trí hiện tại
   - Chi phí: `1 + số_ô_bẩn_còn_lại_sau_khi_hút`

### Heuristic Function

```python
h(state) = d_min + MST + penalty
```

Trong đó:
- **d_min**: Khoảng cách Chebyshev đến ô bẩn gần nhất
- **MST**: Chi phí cây khung nhỏ nhất nối các ô bẩn
- **penalty**: Ước lượng penalty từ ô bẩn còn lại

### Tính Năng Debug

- 📊 **Bảng Open List**: Hiển thị heap với g, h, f
- 📋 **Bảng Neighbors**: Các trạng thái mới sinh ra
- 🗺️ **Visualization**: Hình ảnh ma trận từng bước
- 📈 **So sánh thuật toán**: A* vs BFS vs DFS vs Dijkstra

### Kết Quả Điển Hình

**Bài toán 30×30 với 5 ô bẩn:**

| Thuật Toán | Chi Phí | Số Bước | Node Mở Rộng | Thời Gian |
|------------|---------|---------|--------------|-----------|
| A* (Chebyshev) | 261 | 69 | 13,267 | 122 ms |
| A* (Manhattan) | 261 | 69 | 10,769 | 110 ms |
| Dijkstra | 261 | 69 | 19,271 | 93 ms |
| BFS | 261 | 69 | 25,280 | 93 ms |
| DFS | 5,536 | 1,197 | 2,353 | 8 ms |

**Nhận xét:**
- ✅ A* tìm được nghiệm tối ưu với số node mở rộng ít hơn Dijkstra và BFS
- ✅ Manhattan heuristic hiệu quả nhất (ít node nhất)
- ✅ DFS nhanh nhưng không tối ưu

---

## 🔍 Thuật Toán Dijkstra

### Mô Tả Bài Toán

Tìm đường đi ngắn nhất từ đỉnh `A` (start) đến một đỉnh đích trên đồ thị vô hướng có trọng số.

### Đặc Điểm

- **Đồ thị**: Vô hướng, trọng số dương
- **Input**: File CSV chứa danh sách cạnh `(u, v, weight)`
- **Output**: 
  - Đường đi ngắn nhất
  - Tổng chi phí
  - Visualization đồ thị
  - Bảng chi tiết từng bước

### Cấu Trúc Dữ Liệu

```python
class Graph:
    def __init__(self, edges):
        self.adj = defaultdict(list)  # Adjacency list
        self.positions = {}            # Node positions for plotting
        self.edge_list = edges
```

### Tính Năng

1. **Layout Thông Minh**: Tự động bố trí đỉnh theo kiểu neural network
2. **Visualization Đẹp**: 
   - Màu sắc phân biệt: start (xanh lá), goal (đỏ), path (vàng)
   - Hiển thị trọng số cạnh
   - Animation đường đi
3. **Debug Mode**: 
   - In từng bước thuật toán
   - Bảng distances và predecessors
   - Trạng thái priority queue

### Kết Quả Điển Hình

**Đồ thị 15 đỉnh, 25 cạnh:**
- ✅ Tìm đường đi ngắn nhất từ A → O
- ✅ Thời gian: < 1ms
- ✅ Visualization rõ ràng, dễ hiểu

---

## 💻 Cài Đặt

### Yêu Cầu Hệ Thống

- Python 3.8 trở lên
- Jupyter Notebook/Lab
- pip hoặc conda

## 🛠️ Công Nghệ

### Ngôn Ngữ & Framework

- **Python 3.8+**: Ngôn ngữ chính
- **Jupyter Notebook**: Môi trường phát triển interactive

### Thư Viện

| Thư Viện | Mục Đích |
|----------|----------|
| `numpy` | Tính toán số học, ma trận |
| `matplotlib` | Visualization, vẽ đồ thị |
| `pandas` | Xử lý và hiển thị bảng dữ liệu |
| `heapq` | Cấu trúc dữ liệu heap (priority queue) |
| `collections` | defaultdict, deque |

---

## 👥 Tác Giả

**Nhóm 1**

- Thái Hoài An
- Hoàng Thụy Hồng Ân
- Dương Phương Anh
- Nguyễn Đức Tuấn Anh
- Nguyễn Thị Minh Anh

**Giảng viên hướng dẫn:**
- Nguyễn An Tế

**Trường:** Đại học Kinh tế TP.HCM (UEH)  
**Môn học:** Lập Trình Phân Tích Dữ Liệu  
**Học kỳ:** Học kỳ cuối - Năm học 2024-2025


## 🙏 Lời Cảm Ơn

Cảm ơn thầy/cô giảng viên và các bạn trong lớp đã hỗ trợ trong quá trình thực hiện đồ án.

---

<div align="center">
  
### ⭐ Nếu thấy dự án hữu ích, hãy cho chúng tôi một ngôi sao! ⭐

</div>

