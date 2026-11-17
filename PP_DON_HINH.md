# 📊 Tổng hợp công thức Phương pháp Đơn hình (Simplex Method)

Tài liệu tổng hợp các ký hiệu, công thức toán học và quy tắc thuật toán được sử dụng trong **Phương pháp Đơn hình (Simplex Method)** để giải các bài toán quy hoạch tuyến tính (Linear Programming), bao gồm bài toán tối thiểu hóa (Min) và tối đa hóa (Max).

---

## 1. Ký hiệu cơ bản

| Ký hiệu | Ý nghĩa |
|:---:|---|
| \(C_B\) | Vector hệ số hàm mục tiêu của các biến cơ sở |
| \(X_B\) | Tập hợp các biến cơ sở |
| \(x_0\) | Vector phương án hiện tại (cột giá trị vế phải) |
| \(A_j\) | Vector hệ số của biến \(x_j\) trong hệ ràng buộc |
| \(c_j\) | Hệ số của biến \(x_j\) trong hàm mục tiêu |
| \(a_{ij}\) | Phần tử tại dòng \(i\), cột \(j\) của bảng đơn hình |
| \(m\) | Số lượng ràng buộc |
| \(n\) | Số lượng biến |
| \(\Delta_j\) | Hệ số ước lượng của biến \(x_j\) |
| \(f_0\) | Giá trị hàm mục tiêu tại phương án hiện tại |
| \(a_{qp}\) | Phần tử trục (Pivot Element) |

Trong đó:
- \(i=1,\ldots,m\): Chỉ số dòng.
- \(j=1,\ldots,n\): Chỉ số biến.

## 2. Các công thức tính toán ban đầu

### 2.1. Tính hệ số ước lượng ($\Delta_j$)

**Đối với bài toán Min:**

$$
\Delta_j = C_B^T A_j - c_j
$$

**Đối với bài toán Max:**

$$
\Delta_j = c_j - C_B^T A_j
$$

Trong đó:

$$
C_B^T A_j = \sum_{i=1}^{m} c_{B_i}a_{ij}
$$

Đối với mọi biến cơ sở:

$$
\boxed{\Delta_j = 0,\quad x_j \in X_B}
$$

### 2.2. Tính giá trị hàm mục tiêu

$$
f_0 = C_B^T x_0
$$

Hay:

$$
f_0 = \sum_{i=1}^{m} c_{B_i}x_{0i}
$$

### 2.3. Nghiệm cơ sở ban đầu

$$
x^{(0)} =
\begin{pmatrix}
x_1\\
x_2\\
\vdots\\
x_n
\end{pmatrix}
$$

Với:

$$
x_j =
\begin{cases}
x_{0i}, & \text{nếu } x_j = (X_B)_i\\
0, & \text{nếu } x_j \notin X_B
\end{cases}
$$

Giá trị hàm mục tiêu:

$$
f(x^{(0)}) = f_0
$$

---

## 3. Tiêu chuẩn tối ưu và điều kiện không giới hạn

### 3.1. Điều kiện tối ưu

**Bài toán Min:**

$$
\boxed{
\Delta_j \leq 0,\quad j=1,\ldots,n
}
$$

**Bài toán Max:**

$$
\boxed{
\Delta_j \leq 0,\quad j=1,\ldots,n
}
$$

Giá trị tối ưu:

$$
f_{\mathrm{opt}} = f_0
$$

### 3.2. Điều kiện không giới hạn

Nếu tồn tại biến $x_k$ thỏa mãn:

$$
\Delta_k > 0
$$

và:

$$
a_{ik} \leq 0,\quad i=1,\ldots,m
$$

Khi đó:

$$
\boxed{
\text{Bài toán không có nghiệm tối ưu hữu hạn}
}
$$

---

## 4. Quy tắc đổi cơ sở (Pivot)

### Bước 1. Chọn cột chủ yếu

$$
\boxed{
\Delta_p =
\max_{1\leq j\leq n}
\left\{
\Delta_j \mid \Delta_j > 0
\right\}
}
$$

### Bước 2. Chọn dòng chủ yếu

Tính tỷ số:

$$
\lambda_i = \frac{x_{0i}}{a_{ip}},
\quad a_{ip}>0
$$

Chọn tỷ số nhỏ nhất:

$$
\boxed{
\lambda_q =
\min_{\substack{1\leq i\leq m\\a_{ip}>0}}
\frac{x_{0i}}{a_{ip}}
}
$$

Phần tử trục:

$$
\boxed{a_{qp}>0}
$$

### Bước 3. Biến đổi bảng đơn hình

**3.1. Cập nhật dòng chủ yếu**

$$
R_q^{(\mathrm{new})}
=
\frac{R_q^{(\mathrm{old})}}{a_{qp}}
$$

Cập nhật từng phần tử:

$$
a_{qj}^{(\mathrm{new})}
=
\frac{a_{qj}^{(\mathrm{old})}}{a_{qp}}
$$

$$
x_{0q}^{(\mathrm{new})}
=
\frac{x_{0q}^{(\mathrm{old})}}{a_{qp}}
$$

**3.2. Cập nhật các dòng còn lại**

Với $i\neq q$:

$$
R_i^{(\mathrm{new})}
=
R_i^{(\mathrm{old})}
-
a_{ip}R_q^{(\mathrm{new})}
$$

Cập nhật từng phần tử:

$$
a_{ij}^{(\mathrm{new})}
=
a_{ij}^{(\mathrm{old})}
-
\frac{
a_{ip}a_{qj}^{(\mathrm{old})}
}{
a_{qp}
}
$$

Cập nhật giá trị phương án:

$$
x_{0i}^{(\mathrm{new})}
=
x_{0i}^{(\mathrm{old})}
-
\frac{
a_{ip}x_{0q}^{(\mathrm{old})}
}{
a_{qp}
}
$$

**3.3. Cập nhật tập biến cơ sở**

$$
X_B^{(\mathrm{new})}
=
\left(
X_B^{(\mathrm{old})}
\setminus \{x_q\}
\right)
\cup \{x_p\}
$$

---

## 5. Công thức tổng quát của phép Pivot

Có thể tổng hợp phép biến đổi bảng đơn hình bằng công thức:

$$
\boxed{
a_{ij}^{(\mathrm{new})}
=
\begin{cases}
\dfrac{1}{a_{qp}}, & i=q,\ j=p\\[8pt]
\dfrac{a_{qj}^{(\mathrm{old})}}{a_{qp}}, & i=q,\ j\neq p\\[8pt]
0, & i\neq q,\ j=p\\[8pt]
a_{ij}^{(\mathrm{old})}
-
\dfrac{a_{ip}a_{qj}^{(\mathrm{old})}}{a_{qp}},
& i\neq q,\ j\neq p
\end{cases}
}
$$

Sau phép Pivot, cột chủ yếu trở thành cột đơn vị:

$$
A_p^{(\mathrm{new})}
=
\begin{pmatrix}
0\\
\vdots\\
1\\
\vdots\\
0
\end{pmatrix}
$$

Trong đó phần tử $1$ nằm tại dòng $q$.

---

## 6. Quy trình thuật toán Simplex

$$
\boxed{
\text{Khởi tạo}
\rightarrow
\text{Tính }\Delta_j
\rightarrow
\text{Kiểm tra tối ưu}
}
$$

Nếu chưa tối ưu:

$$
\boxed{
\text{Chọn cột}
\rightarrow
\text{Chọn dòng}
\rightarrow
\text{Pivot}
\rightarrow
\text{Lặp lại}
}
$$

## 6. Tóm tắt công thức quan trọng

| Nội dung | Công thức |
|---|---|
| Hệ số ước lượng (Min) | \(\Delta_j=C_B^TA_j-c_j\) |
| Hệ số ước lượng (Max) | \(\Delta_j=c_j-C_B^TA_j\) |
| Hàm mục tiêu | \(f_0=C_B^Tx_0\) |
| Điều kiện tối ưu | \(\Delta_j\leq 0\) |
| Chọn cột chủ yếu | \(\Delta_p=\max\{\Delta_j>0\}\) |
| Tỷ số kiểm tra | \(\lambda_i=x_{0i}/a_{ip}\) |
| Chọn dòng chủ yếu | \(\lambda_q=\min\{\lambda_i\}\) |
| Phần tử trục | \(a_{qp}\) |
| Dòng chủ yếu mới | \(R_q^{new}=R_q^{old}/a_{qp}\) |
| Dòng còn lại mới | \(R_i^{new}=R_i^{old}-a_{ip}R_q^{new}\) |

---

**Lưu ý:** Tài liệu sử dụng quy ước hệ số ước lượng đã nêu tại mục 2. Khi áp dụng thuật toán, cần bảo đảm phương án cơ sở ban đầu khả thi và các biến thỏa mãn điều kiện không âm. Trường hợp có nhiều lựa chọn cùng tỷ số nhỏ nhất có thể xuất hiện suy biến (Degeneracy); khi đó cần áp dụng quy tắc phá hòa phù hợp để tránh lặp chu trình.
