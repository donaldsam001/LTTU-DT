# HƯỚNG DẪN GIẢI BÀI TOÁN TỐI ƯU BẰNG EXCEL SOLVER

Tài liệu hướng dẫn sử dụng **Microsoft Excel Solver** để giải: 1. Bài
toán quy hoạch tuyến tính bằng phương pháp đơn hình (Simplex LP). 2. Bài
toán vận tải (Transportation Problem).

Các công thức được viết bằng Unicode và Markdown thông thường, không sử
dụng LaTeX/MathJax.

------------------------------------------------------------------------

# 1. Chuẩn bị Excel Solver

## 1.1. Kích hoạt Solver Add-in

1.  Mở Microsoft Excel.
2.  Chọn **File → Options**.
3.  Chọn **Add-ins**.
4.  Tại mục **Manage**, chọn **Excel Add-ins → Go**.
5.  Tích chọn **Solver Add-in**.
6.  Nhấn **OK**.

Sau khi kích hoạt, Solver xuất hiện tại **Data → Solver**.

## 1.2. Các thành phần trong Solver

  Thành phần                                  Ý nghĩa
  ------------------------------------------- -----------------------------
  Set Objective                               Ô chứa hàm mục tiêu
  To                                          Chọn Max, Min hoặc Value Of
  By Changing Variable Cells                  Các ô chứa biến quyết định
  Subject to the Constraints                  Các ràng buộc
  Make Unconstrained Variables Non-Negative   Yêu cầu biến không âm
  Select a Solving Method                     Phương pháp giải

Đối với bài toán quy hoạch tuyến tính, chọn **Simplex LP**.

------------------------------------------------------------------------

# 2. Giải bài toán quy hoạch tuyến tính bằng Simplex LP

## 2.1. Mô hình bài toán

**Hàm mục tiêu:**

Min/Max Z = c₁x₁ + c₂x₂ + ... + cₙxₙ

**Các ràng buộc:**

-   a₁₁x₁ + a₁₂x₂ + ... + a₁ₙxₙ ≤ b₁
-   a₂₁x₁ + a₂₂x₂ + ... + a₂ₙxₙ ≥ b₂
-   ...
-   aₘ₁x₁ + aₘ₂x₂ + ... + aₘₙxₙ = bₘ

**Điều kiện không âm:**

x₁, x₂, ..., xₙ ≥ 0

Trong đó: - x₁, x₂, ..., xₙ là các biến quyết định. - c₁, c₂, ..., cₙ là
hệ số hàm mục tiêu. - aᵢⱼ là hệ số của biến xⱼ trong ràng buộc thứ i. -
bᵢ là hằng số ở vế phải của ràng buộc thứ i.

## 2.2. Thiết lập bảng dữ liệu Excel

Ví dụ minh họa với 4 biến quyết định và 3 ràng buộc:

  Hàng      A    B    C    D E                  F
  ------ ---- ---- ---- ---- -------------- -----
  1        x₁   x₂   x₃   x₄                
  2         0    0    0    0                
  3         3    6    8   10 Hàm mục tiêu   
  4         1    1    2    3                  100
  5         1    2    1    0                   50
  6         1    1    1    2                   70

Ý nghĩa: - **A2:D2:** Các biến quyết định x₁, x₂, x₃, x₄. Nhập giá trị
ban đầu, ví dụ 0. - **A3:D3:** Hệ số của hàm mục tiêu. - **A4:D6:** Hệ
số của các ràng buộc. - **F4:F6:** Giá trị giới hạn ở vế phải. - **E3:**
Ô tính hàm mục tiêu. - **E4:E6:** Các ô tính vế trái của ràng buộc.

## 2.3. Sử dụng hàm SUMPRODUCT

Hàm `SUMPRODUCT` nhân các phần tử tương ứng trong hai vùng dữ liệu rồi
tính tổng.

**Cú pháp:**

``` excel
=SUMPRODUCT(array1, array2)
```

### Bước 1: Tính hàm mục tiêu

Tại ô **E3**, nhập:

``` excel
=SUMPRODUCT(A3:D3,$A$2:$D$2)
```

Công thức: Z = 3x₁ + 6x₂ + 8x₃ + 10x₄.

### Bước 2: Tính các ràng buộc

Tại ô **E4**, nhập:

``` excel
=SUMPRODUCT(A4:D4,$A$2:$D$2)
```

Tại ô **E5**, nhập:

``` excel
=SUMPRODUCT(A5:D5,$A$2:$D$2)
```

Tại ô **E6**, nhập:

``` excel
=SUMPRODUCT(A6:D6,$A$2:$D$2)
```

Các biểu thức tương ứng: - E4 = x₁ + x₂ + 2x₃ + 3x₄ - E5 = x₁ + 2x₂ +
x₃ - E6 = x₁ + x₂ + x₃ + 2x₄

## 2.4. Thiết lập Solver

Mở **Data → Solver**.

**Set Objective:**

``` text
$E$3
```

**To:** - Chọn **Max** nếu tìm giá trị lớn nhất. - Chọn **Min** nếu tìm
giá trị nhỏ nhất.

**By Changing Variable Cells:**

``` text
$A$2:$D$2
```

**Subject to the Constraints:**

  Cell Reference   Relation   Constraint
  ---------------- ---------- ------------
  \$A$2:$D\$2      \>=        0
  \$E\$4           \<=        \$F\$4
  \$E\$5           \>=        \$F\$5
  \$E\$6           =          \$F\$6

Các ràng buộc tương ứng: - x₁, x₂, x₃, x₄ ≥ 0 - x₁ + x₂ + 2x₃ + 3x₄ ≤
100 - x₁ + 2x₂ + x₃ ≥ 50 - x₁ + x₂ + x₃ + 2x₄ = 70

**Select a Solving Method:**

``` text
Simplex LP
```

Có thể tích chọn **Make Unconstrained Variables Non-Negative**. Nếu đã
khai báo riêng ràng buộc biến ≥ 0, hãy kiểm tra để tránh thiết lập mâu
thuẫn.

## 2.5. Tiến hành giải

1.  Kiểm tra công thức hàm mục tiêu tại E3.
2.  Kiểm tra công thức ràng buộc tại E4:E6.
3.  Kiểm tra vùng biến quyết định A2:D2.
4.  Khai báo đầy đủ các ràng buộc trong Solver.
5.  Chọn **Simplex LP**.
6.  Nhấn **Solve**.
7.  Trong hộp thoại Solver Results, chọn **Keep Solver Solution**.
8.  Nhấn **OK**.

Kết quả nhận được: - Giá trị tối ưu của hàm mục tiêu tại E3. - Giá trị
các biến quyết định tại A2:D2. - Giá trị vế trái của các ràng buộc tại
E4:E6.

------------------------------------------------------------------------

# 3. Giải bài toán vận tải bằng Excel Solver

## 3.1. Mô hình bài toán

Bài toán vận tải gồm: - m nguồn cung cấp hàng: A₁, A₂, ..., Aₘ. - n nơi
nhận hàng: B₁, B₂, ..., Bₙ. - aᵢ: Lượng hàng cung cấp tại nguồn Aᵢ. -
bⱼ: Nhu cầu tại nơi nhận Bⱼ. - cᵢⱼ: Chi phí vận chuyển một đơn vị hàng
từ Aᵢ đến Bⱼ. - xᵢⱼ: Lượng hàng vận chuyển từ Aᵢ đến Bⱼ.

**Hàm mục tiêu:**

Min Z = Σᵢ₌₁ᵐ Σⱼ₌₁ⁿ cᵢⱼxᵢⱼ

Tức là tổng chi phí vận chuyển trên tất cả các tuyến.

**Ràng buộc cung:**

Với mỗi nguồn Aᵢ:

xᵢ₁ + xᵢ₂ + ... + xᵢₙ = aᵢ

**Ràng buộc cầu:**

Với mỗi nơi nhận Bⱼ:

x₁ⱼ + x₂ⱼ + ... + xₘⱼ = bⱼ

**Điều kiện không âm:**

xᵢⱼ ≥ 0 với mọi i, j.

Bài toán vận tải cân bằng khi:

Tổng cung = Tổng cầu

a₁ + a₂ + ... + aₘ = b₁ + b₂ + ... + bₙ

## 3.2. Thiết lập bảng dữ liệu Excel

Ví dụ ma trận chi phí gồm 3 nguồn và 4 nơi nhận:

  Nguồn / Nơi nhận     B1   B2   B3   B4
  ------------------ ---- ---- ---- ----
  A1                   23   27   16   18
  A2                   12   17   20   51
  A3                   22   28   12   32

Các giá trị trên là chi phí vận chuyển cᵢⱼ.

Thiết lập ma trận biến quyết định riêng:

  Nguồn / Nơi nhận   B1    B2    B3    B4    Tổng xuất
  ------------------ ----- ----- ----- ----- -----------
  A1                 x₁₁   x₁₂   x₁₃   x₁₄   a₁
  A2                 x₂₁   x₂₂   x₂₃   x₂₄   a₂
  A3                 x₃₁   x₃₂   x₃₃   x₃₄   a₃
  Tổng nhận          b₁    b₂    b₃    b₄    

Quy ước địa chỉ ô trong ví dụ: - Ma trận biến quyết định: B2:E4. - Tổng
lượng hàng xuất: F2:F4. - Tổng lượng cung: G2:G4. - Tổng lượng hàng
nhận: B5:E5. - Nhu cầu: B6:E6. - Ma trận chi phí: B9:E11. - Ô hàm mục
tiêu: F1.

Nhập lượng cung tại G2:G4 và nhu cầu tại B6:E6 theo dữ liệu đề bài. Các
ô B2:E4 là biến quyết định, có thể khởi tạo bằng 0.

## 3.3. Sử dụng hàm SUMPRODUCT và SUM

### Bước 1: Tính tổng chi phí vận chuyển

Tại ô **F1**, nhập:

``` excel
=SUMPRODUCT(B2:E4,B9:E11)
```

Hàm nhân từng lượng hàng vận chuyển với chi phí tương ứng rồi cộng tất
cả các tích.

Với ma trận chi phí ví dụ:

Z = 23x₁₁ + 27x₁₂ + 16x₁₃ + 18x₁₄

-   12x₂₁ + 17x₂₂ + 20x₂₃ + 51x₂₄

-   22x₃₁ + 28x₃₂ + 12x₃₃ + 32x₃₄

### Bước 2: Tính tổng lượng hàng xuất từ mỗi nguồn

Tại ô **F2**, nhập:

``` excel
=SUM(B2:E2)
```

Sao chép công thức xuống F3:F4.

-   F2: Tổng lượng hàng xuất từ A1.
-   F3: Tổng lượng hàng xuất từ A2.
-   F4: Tổng lượng hàng xuất từ A3.

### Bước 3: Tính tổng lượng hàng nhận tại mỗi nơi

Tại ô **B5**, nhập:

``` excel
=SUM(B2:B4)
```

Sao chép công thức sang C5:E5.

-   B5: Tổng lượng hàng nhận tại B1.
-   C5: Tổng lượng hàng nhận tại B2.
-   D5: Tổng lượng hàng nhận tại B3.
-   E5: Tổng lượng hàng nhận tại B4.

## 3.4. Thiết lập Solver

Mở **Data → Solver**.

**Set Objective:**

``` text
$F$1
```

**To:**

``` text
Min
```

Bài toán vận tải cần tối thiểu hóa tổng chi phí.

**By Changing Variable Cells:**

``` text
$B$2:$E$4
```

Đây là vùng chứa các biến quyết định xᵢⱼ.

**Subject to the Constraints:**

  Cell Reference   Relation   Constraint
  ---------------- ---------- -------------
  \$B$2:$E\$4      \>=        0
  \$F$2:$F\$4      =          \$G$2:$G\$4
  \$B$5:$E\$5      =          \$B$6:$E\$6

Ý nghĩa: 1. Lượng hàng trên mọi tuyến phải không âm. 2. Tổng hàng xuất
từ mỗi nguồn phải bằng lượng cung của nguồn đó. 3. Tổng hàng nhận tại
mỗi nơi phải bằng nhu cầu của nơi đó.

**Select a Solving Method:**

``` text
Simplex LP
```

## 3.5. Tiến hành giải

1.  Nhập ma trận chi phí vận chuyển.
2.  Nhập lượng cung tại G2:G4.
3.  Nhập nhu cầu tại B6:E6.
4.  Khởi tạo các biến vận chuyển B2:E4, ví dụ bằng 0.
5.  Kiểm tra công thức tổng chi phí tại F1.
6.  Kiểm tra tổng lượng xuất tại F2:F4.
7.  Kiểm tra tổng lượng nhận tại B5:E5.
8.  Thiết lập hàm mục tiêu, biến quyết định và các ràng buộc trong
    Solver.
9.  Chọn **Simplex LP**.
10. Nhấn **Solve**.
11. Chọn **Keep Solver Solution** để giữ kết quả.

Kết quả thu được: - Ma trận phân phối hàng hóa tối ưu. - Lượng hàng vận
chuyển trên từng tuyến. - Tổng chi phí vận chuyển nhỏ nhất.

## 3.6. Trường hợp bài toán không cân bằng

Nếu tổng cung khác tổng cầu, cần cân bằng bài toán trước khi giải.

**Trường hợp 1: Tổng cung lớn hơn tổng cầu**

Bổ sung một nơi nhận giả có nhu cầu:

Nhu cầu giả = Tổng cung - Tổng cầu

**Trường hợp 2: Tổng cầu lớn hơn tổng cung**

Bổ sung một nguồn cung giả có lượng cung:

Cung giả = Tổng cầu - Tổng cung

Chi phí vận chuyển đến hoặc đi từ điểm giả thường được đặt bằng 0 nếu đề
bài không quy định mức chi phí khác.

Sau khi cân bằng, thiết lập Solver như bài toán vận tải thông thường.

------------------------------------------------------------------------

# 4. Một số lưu ý khi sử dụng Solver

## 4.1. Đối với bài toán Simplex LP

-   Chọn đúng Max hoặc Min theo yêu cầu đề bài.
-   Khai báo đúng vùng chứa biến quyết định.
-   Công thức hàm mục tiêu phải sử dụng đúng hệ số.
-   Không bỏ sót các ràng buộc.
-   Chọn đúng phương pháp Simplex LP.
-   Kiểm tra điều kiện không âm của các biến.

## 4.2. Đối với bài toán vận tải

-   Phân biệt ma trận chi phí và ma trận biến quyết định.
-   Không nhập chi phí vận chuyển vào vùng biến quyết định.
-   Kiểm tra tổng cung và tổng cầu.
-   Bổ sung nguồn giả hoặc nơi nhận giả nếu bài toán không cân bằng.
-   Các biến vận chuyển phải không âm.
-   Chỉ thêm điều kiện số nguyên nếu đề bài yêu cầu lượng hàng phải là
    số nguyên.

## 4.3. Các lỗi thường gặp

  -----------------------------------------------------------------------
  Lỗi                                 Cách khắc phục
  ----------------------------------- -----------------------------------
  Không tìm thấy Solver               Kích hoạt Solver Add-in

  Solver không tìm được nghiệm khả    Kiểm tra dữ liệu và tính tương
  thi                                 thích của các ràng buộc

  Kết quả không chính xác             Kiểm tra vùng tham chiếu trong
                                      SUMPRODUCT

  Biến có giá trị âm                  Thêm ràng buộc biến ≥ 0

  Tổng cung không bằng tổng cầu       Cân bằng bài toán bằng nguồn giả
                                      hoặc nơi nhận giả

  Chọn sai kết quả tối ưu             Kiểm tra lại lựa chọn Max/Min

  Công thức trả về lỗi                Kiểm tra địa chỉ ô và kiểu dữ liệu
  -----------------------------------------------------------------------

------------------------------------------------------------------------

# 5. So sánh hai dạng bài toán

  Tiêu chí             Quy hoạch tuyến tính           Bài toán vận tải
  -------------------- ------------------------------ ---------------------------
  Hàm mục tiêu         Max hoặc Min                   Thường là Min
  Biến quyết định      x₁, x₂, ..., xₙ                xᵢⱼ
  Ràng buộc            Bất đẳng thức hoặc đẳng thức   Cung và cầu
  Điều kiện            Biến không âm                  Lượng vận chuyển không âm
  Hàm Excel            SUMPRODUCT                     SUMPRODUCT, SUM
  Phương pháp Solver   Simplex LP                     Simplex LP

------------------------------------------------------------------------

# 6. Kết luận

Microsoft Excel Solver hỗ trợ giải các bài toán quy hoạch tuyến tính và
bài toán vận tải mà không cần thực hiện thủ công toàn bộ các bước tính
toán.

**Quy trình chung:**

1.  Xây dựng mô hình toán học.
2.  Thiết lập bảng dữ liệu trong Excel.
3.  Xác định các ô biến quyết định.
4.  Sử dụng SUMPRODUCT và SUM để tính hàm mục tiêu, các ràng buộc.
5.  Thiết lập Solver.
6.  Chọn phương pháp Simplex LP.
7.  Chạy Solver và đọc kết quả tối ưu.

**Lưu ý:** Solver sử dụng thuật toán tối ưu hóa để tìm nghiệm. Việc chọn
Simplex LP không đồng nghĩa với việc Excel hiển thị từng bảng đơn hình
trung gian như khi giải bằng tay.
