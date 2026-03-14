# Báo cáo: Giải bài toán XOR bằng Neural Network (Backpropagation)

## Mô tả bài toán

Bài toán XOR có dữ liệu:

  x1   x2   y
  ---- ---- ---
  0    0    0
  0    1    1
  1    0    1
  1    1    0

Mạng neural sử dụng:

-   Input layer: 2 neuron (x1, x2)
-   Hidden layer: 2 neuron
-   Output layer: 1 neuron
-   Activation: Sigmoid
-   Loss: Binary Cross Entropy
-   Tối ưu: Gradient Descent + Backpropagation

Ký hiệu:

-   X ∈ R\^(m×2): ma trận input
-   Y ∈ R\^(m×1): nhãn
-   W1 ∈ R\^(2×2), b1 ∈ R\^(1×2)
-   W2 ∈ R\^(2×1), b2 ∈ R\^(1×1)

Hàm sigmoid:

σ(z) = 1 / (1 + e\^{-z})


# Câu 1. Phương trình Forward

### Với 1 mẫu

Gọi x = \[x1, x2\]

1.  Tính lớp ẩn

z1 = xW1 + b1

a1 = σ(z1)

2.  Tính lớp output

z2 = a1W2 + b2

ŷ = a2 = σ(z2)

### Với nhiều mẫu

Với X ∈ R\^(m×2)

Z1 = XW1 + b1

A1 = σ(Z1)

Z2 = A1W2 + b2

A2 = σ(Z2)

Trong đó:

A2 = ŷ là vector dự đoán của mô hình.


# Câu 2. Đạo hàm (Backpropagation)

Loss Binary Cross Entropy:

L = -(1/m) Σ \[ y log(a2) + (1-y) log(1-a2) \]


## Gradient tại lớp output

Với sigmoid + BCE:

dZ2 = A2 - Y

(kích thước m×1)


### Gradient của lớp output

dW2 = (1/m) A1\^T dZ2

db2 = (1/m) Σ_rows(dZ2)


## Lan truyền lỗi về lớp ẩn

dA1 = dZ2 W2\^T

Đạo hàm sigmoid:

σ'(z) = σ(z)(1-σ(z))

Do đó:

dZ1 = dA1 ⊙ A1 ⊙ (1 - A1)


### Suy ra

dW1 = (1/m) X\^T dZ1

db1 = (1/m) Σ_rows(dZ1)


# Câu 3. Các bước thuật toán Gradient Descent

1.  Khởi tạo ngẫu nhiên W1, b1, W2, b2 với giá trị nhỏ.

2.  Lặp theo số epoch:

    Forward:

    -   Z1 = XW1 + b1
    -   A1 = σ(Z1)
    -   Z2 = A1W2 + b2
    -   A2 = σ(Z2)

    Tính loss BCE.

    Backward:

    -   dZ2 = A2 - Y

    -   dW2 = (1/m) A1\^T dZ2

    -   db2 = (1/m) Σ_rows(dZ2)

    -   dA1 = dZ2 W2\^T

    -   dZ1 = dA1 ⊙ A1 ⊙ (1-A1)

    -   dW1 = (1/m) X\^T dZ1

    -   db1 = (1/m) Σ_rows(dZ1)

3.  Cập nhật tham số:

W1 = W1 - lr \* dW1\
b1 = b1 - lr \* db1\
W2 = W2 - lr \* dW2\
b2 = b2 - lr \* db2

4.  Lặp lại cho đến khi đủ số epoch hoặc loss hội tụ.

5. Nếu giá trị dự đoán ≥ 0.5 → dự đoán lớp 1\
Nếu \< 0.5 → dự đoán lớp 0


