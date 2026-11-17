# 📚 Tổng hợp Công thức Toán học: Phương pháp Đơn hình & Tối ưu Phi tuyến

Tài liệu này tổng hợp đầy đủ các ký hiệu, công thức toán học và quy tắc thuật toán cho **Phương pháp Đơn hình** (Quy hoạch tuyến tính) và **Tối ưu Phi tuyến** (bao gồm điều kiện tối ưu, dạng toàn phương và phương pháp bình phương tối thiểu).

---

## 📌 MỤC LỤC
1. [Phương pháp Đơn hình (Simplex Method)](#1-phương-pháp-đơn-hình-simplex-method)
2. [Tối ưu Phi tuyến & Điều kiện tối ưu](#2-tối-ưu-phi-tuyến--điều-kiện-tối-ưu)
3. [Tối ưu dạng toàn phương (Quadratic Form)](#3-tối-ưu-dạng-toàn-phương-quadratic-form)
4. [Bài toán xấp xỉ tuyến tính (Bình phương tối thiểu)](#4-bài-toán-xấp-xỉ-tuyến-tính-bình-phương-tối-thiểu)

---

## 1. Phương pháp Đơn hình (Simplex Method)

### 1.1. Ký hiệu cơ bản
* $C_B$: Cột hệ số mục tiêu của các ẩn cơ sở.
* $X_B$: Cột các ẩn cơ sở.
* $x_0 (B)$: Cột giá trị phương án.
* $A_j$: Cột hệ số của biến $x_j$ trong hệ ràng buộc ($j = 1, \dots, n$).

### 1.2. Các công thức tính toán ban đầu

* **Tính hệ số ước lượng ($\Delta_j$):**
  * Đối với bài toán **Min**:

  $$
  \Delta_j = C_B^T A_j - c_j \quad (j = 1, \dots, n)
  $$

  *(Với mọi biến cơ sở $x_j \in X_B$, ta luôn có $\Delta_j = 0$)*

  * Đối với bài toán **Max**:

  $$
  \Delta_j = c_j - C_B^T A_j \quad (j = 1, \dots, n)
  $$

* **Tính giá trị hàm mục tiêu tại phương án xuất phát:**

  $$
  f_0 = C_B^T x_0
  $$

* **Nghiệm cơ sở nhận được ban đầu ($x^{(0)}$):**

  $$
  x^{(0)} = (x_1, \dots, x_n)
  $$

  Trong đó:
  * $x_j = x_{0i}$ nếu $x_j = (x_B)_i$
  * $x_j = 0$ nếu $x_j \notin X_B$
  * Giá trị hàm mục tiêu tương ứng: $f(x^{(0)}) = f_0$

### 1.3. Tiêu chuẩn tối ưu & Không giới hạn

* **Điều kiện tối ưu:**
  * Bài toán **Min**: $\Delta_j \le 0, \quad \forall j = 1, \dots, n \implies x^{(0)}$ là nghiệm tối ưu, giá trị tối ưu là $f_0$.
  * Bài toán **Max**: $\Delta_j \le 0, \quad \forall j = 1, \dots, n$.

* **Dấu hiệu hàm mục tiêu không bị chặn (Không có nghiệm tối ưu):**
  * Tồn tại chỉ số $k$ sao cho $\Delta_k > 0$ và mọi phần tử trong cột $a_{ik} \le 0$ ($\forall i = 1, \dots, m$) $\implies$ Bài toán không có nghiệm tối ưu.

### 1.4. Quy tắc đổi cơ sở và Biến đổi bảng đơn hình

* **Bước 1: Chọn cột chủ yếu (Cần đưa vào cơ sở $x_p$):**

  $$
  \Delta_p = \max_j \{ \Delta_j \mid \Delta_j > 0 \}
  $$

  $\implies$ Cột $A_p$ là **cột chủ yếu**.

* **Bước 2: Chọn dòng chủ yếu (Cần loại khỏi cơ sở $x_q$):**
  Tính các tỉ số $\lambda_i$ cho các dòng có $a_{ip} > 0$:

  $$
  \lambda_i = \frac{x_{0i}}{a_{ip}} \quad (a_{ip} > 0)
  $$

  Chọn dòng $q$ thỏa mãn tỉ số nhỏ nhất:

  $$
  \lambda_q = \min_i \{ \lambda_i \mid a_{ip} > 0 \}
  $$

  $\implies$ Dòng $q$ là **dòng chủ yếu**, phần tử trục (hệ số chủ yếu) là $a_{qp} > 0$.

* **Bước 3: Biến đổi bảng đơn hình tiếp theo:**
  * **Cập nhật dòng chuẩn (dòng $q$ mới):**

  $$
  \text{Dòng } q^{(\text{mới})} = \frac{1}{a_{qp}} \cdot \text{Dòng } q^{(\text{cũ})}
  $$

  Chi tiết từng phần tử:

  $$
  a_{qj}^{(\text{mới})} = \frac{a_{qj}^{(\text{cũ})}}{a_{qp}}, \quad x_{0q}^{(\text{mới})} = \frac{x_{0q}^{(\text{cũ})}}{a_{qp}}
  $$

  * **Cập nhật các dòng còn lại ($i \neq q$):**

  $$
  \text{Dòng } i^{(\text{mới})} = \text{Dòng } i^{(\text{cũ})} - a_{ip} \cdot \text{Dòng } q^{(\text{mới})}
  $$

  Chi tiết từng phần tử:

  $$
  a_{ij}^{(\text{mới})} = a_{ij}^{(\text{cũ})} - \frac{a_{ip} \cdot a_{qj}^{(\text{cũ})}}{a_{qp}}
  $$

  $$
  x_{0i}^{(\text{mới})} = x_{0i}^{(\text{cũ})} - \frac{a_{ip} \cdot x_{0q}^{(\text{cũ})}}{a_{qp}}
  $$

---

## 2. Tối ưu Phi tuyến & Điều kiện tối ưu

### 2.1. Bài toán tối ưu tổng quát
Phát biểu bài toán tìm cực trị của hàm mục tiêu $f: D \to \mathbb{R}^n$:

$$
\min_{x \in D} f(x) \quad \text{(hoặc } \max_{x \in D} f(x) \text{)}
$$

* **$D \subset \mathbb{R}^n$**: Tập nghiệm chấp nhận được (tập ràng buộc).
  * Nếu $D = \mathbb{R}^n$: Bài toán tối ưu **không có ràng buộc**.
  * Nếu $D \subset \mathbb{R}^n$: Bài toán tối ưu **có ràng buộc**.
* **Nghiệm tối ưu toàn cục**: Điểm $x^* = (x_1^*, x_2^*, \dots, x_n^*) \in D$ thỏa mãn $f(x^*) \le f(x)$ với mọi $x \in D$.
* **Nghiệm tối ưu địa phương**: Điểm $x^* \in D$ nếu tồn tại hằng số $\varepsilon > 0$ sao cho với mọi $x \in D$ thỏa mãn khoảng cách:

  $$
  \|x - x^*\|_2 = \sqrt{(x_1 - x_1^*)^2 + (x_2 - x_2^*)^2 + \dots + (x_n - x_n^*)^2} < \varepsilon
  $$

  thì $f(x^*) \le f(x)$.
* **Quan hệ Min - Max**:

  $$
  \min \{f(x) \mid x \in D\} = -\max \{-f(x) \mid x \in D\}
  $$

### 2.2. Điều kiện tối ưu (Không ràng buộc trên $\mathbb{R}^n$)

* **Điều kiện bậc nhất:**
  * Nếu $x^* \in \mathbb{R}^n$ là nghiệm cực tiểu địa phương của hàm khả vi $f$ thì:

  $$
  \nabla f(x^*) = \vec{0}
  $$

  *(Điểm $x^*$ thỏa mãn $\nabla f(x^*) = \vec{0}$ được gọi là **điểm nghi ngờ có cực trị**).*
  * Nếu $f$ là **hàm lồi** khả vi trên $\mathbb{R}^n$: $x^*$ là nghiệm cực tiểu toàn cục $\iff \nabla f(x^*) = \vec{0}$.

* **Điều kiện bậc hai qua ma trận Hessian:** Giả sử hàm $f$ có ma trận Hessian $\nabla^2 f(x)$ liên tục:
  * **Cực tiểu địa phương**: $\nabla f(x^*) = \vec{0}$ và ma trận $\nabla^2 f(x^*)$ **xác định dương**.
  * **Cực đại địa phương**: $\nabla f(x^*) = \vec{0}$ và ma trận $\nabla^2 f(x^*)$ **xác định âm**.

---

## 3. Tối ưu dạng toàn phương (Quadratic Form)

Cho ma trận vuông $A = (a_{ij})_{n \times n}$, vector $b = (b_1, b_2, \dots, b_n)^T$ và hằng số $c$. Hàm số có dạng toàn phương được biểu diễn dưới dạng ma trận và khai triển tổng:

$$
f(x_1, x_2, \dots, x_n) = \frac{1}{2} x^T A x + b^T x + c
$$

$$
f(x_1, x_2, \dots, x_n) = \sum_{i,j=1}^{n} a_{ij} x_i x_j + \sum_{i=1}^{n} b_i x_i + c
$$

* **Nhận xét về ma trận Hessian** (khi $A$ đối xứng):

  $$
  \nabla^2 f = A
  $$

---

## 4. Bài toán xấp xỉ tuyến tính (Bình phương tối thiểu)

### 4.1. Thiết lập bài toán
Cho bảng dữ liệu thực nghiệm gồm $n$ mẫu quan sát với $m$ biến độc lập $(x_{i1}, x_{i2}, \dots, x_{im})$ và giá trị mục tiêu $y_i$ ($i = 1, \dots, n$). Hàm tuyến tính xấp xỉ có dạng:

$$
y_j = p_0 + p_1 x_{j1} + p_2 x_{j2} + \dots + p_m x_{jm}
$$

Mục tiêu là tìm bộ tham số $P = (p_0, p_1, \dots, p_m)^T$ sao cho tổng bình phương sai số đạt giá trị nhỏ nhất:

$$
f(P) = \sum_{i=1}^{n} \left[ (p_0 + p_1 x_{i1} + p_2 x_{i2} + \dots + p_m x_{im}) - y_i \right]^2 \to \min
$$

### 4.2. Biểu diễn dạng ma trận & Quy về tối ưu toàn phương
Đặt các ma trận và vector:

$$
Q = \begin{pmatrix} 
1 & x_{11} & x_{12} & \dots & x_{1m} \\ 
1 & x_{21} & x_{22} & \dots & x_{2m} \\ 
\vdots & \vdots & \vdots & \ddots & \vdots \\ 
1 & x_{n1} & x_{n2} & \dots & x_{nm} 
\end{pmatrix}, \quad 
P = \begin{pmatrix} p_0 \\ p_1 \\ \vdots \\ p_m \end{pmatrix}, \quad 
Y = \begin{pmatrix} y_1 \\ y_2 \\ \vdots \\ y_n \end{pmatrix}
$$

Khi đó, hàm mục tiêu $f(P)$ được viết gọn lại dưới dạng ma trận:

$$
f(P) = \| Q P - Y \|^2 = \langle QP - Y, QP - Y \rangle = (QP - Y)^T (QP - Y)
$$

$$
f(P) = P^T (Q^T Q) P - 2 Y^T Q P + Y^T Y
$$

Đặt $A = Q^T Q$, $b = -Y^T Q$, và $c = Y^T Y$, bài toán trở thành **tối ưu toàn phương không ràng buộc** với ẩn $P$:

$$
\min_{P} \left( P^T A P + 2 b P + c \right) \iff \min_{P} \left( \frac{1}{2} P^T A P + b P + \frac{1}{2} c \right)
$$

* **Trường hợp đặc biệt ($m = 1$ - Hồi quy tuyến tính đơn biến):**
  Tìm $p_0, p_1$ để cực tiểu hóa:

  $$
  f(p_0, p_1) = \sum_{i=1}^{n} (p_0 + p_1 x_{i1} - y_i)^2
  $$
