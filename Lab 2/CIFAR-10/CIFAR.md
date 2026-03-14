
# Báo cáo: Nhận dạng chữ số viết tay CIFAR-10 bằng Neural Network (1 Hidden Layer)

## Mô tả bài toán

Bài toán nhận dạng chữ số viết tay sử dụng tập dữ liệu **CIFAR-10**.

Mỗi ảnh chữ số có kích thước:

28 x 28 = **784 pixel**

Mạng Neural sử dụng:

- Input layer: **784 neuron**
- Hidden layer: **30 neuron**
- Output layer: **10 neuron** (tương ứng các chữ số 0 → 9)
- Activation function: **Sigmoid**
- Loss: **Cross Entropy**
- Tối ưu: **Gradient Descent + Backpropagation**

Ký hiệu:

- X ∈ R^(m×784): ma trận dữ liệu đầu vào
- Y ∈ R^(m×10): nhãn dạng one-hot
- W1 ∈ R^(784×30)
- b1 ∈ R^(1×30)
- W2 ∈ R^(30×10)
- b2 ∈ R^(1×10)

Hàm sigmoid:

σ(z) = 1 / (1 + e^{-z})

Đạo hàm sigmoid:

σ'(z) = σ(z)(1 - σ(z))

---

# Câu 1. Phương trình Forward

## Với một mẫu

Gọi:

x ∈ R^(1×784)

### Lớp ẩn

z1 = xW1 + b1

a1 = σ(z1)

### Lớp output

z2 = a1W2 + b2

a2 = σ(z2)

Trong đó:

a2 = ŷ = vector dự đoán xác suất của 10 lớp.

---

## Với nhiều mẫu

Với:

X ∈ R^(m×784)

Forward propagation:

Z1 = XW1 + b1

A1 = σ(Z1)

Z2 = A1W2 + b2

A2 = σ(Z2)

Trong đó:

A2 ∈ R^(m×10) là ma trận dự đoán của mô hình.

---

# Câu 2. Đạo hàm (Backpropagation)

Sử dụng hàm mất mát Cross Entropy cho nhiều lớp.

Loss:

L = -(1/m) Σ Σ [ y_ij log(a2_ij) ]

---

## Gradient tại lớp output

dZ2 = A2 - Y

Kích thước:

m × 10

---

## Gradient của W2 và b2

dW2 = (1/m) A1^T dZ2

db2 = (1/m) Σ_rows(dZ2)

---

## Lan truyền lỗi về lớp ẩn

dA1 = dZ2 W2^T

Áp dụng đạo hàm sigmoid:

dZ1 = dA1 ⊙ A1 ⊙ (1 - A1)

---

## Gradient của lớp ẩn

dW1 = (1/m) X^T dZ1

db1 = (1/m) Σ_rows(dZ1)

---

# Câu 3. Các bước của thuật toán Gradient Descent

1. Khởi tạo ngẫu nhiên các tham số:

W1, b1, W2, b2 với giá trị nhỏ.

---

2. Lặp theo số epoch

### Forward propagation

Z1 = XW1 + b1

A1 = σ(Z1)

Z2 = A1W2 + b2

A2 = σ(Z2)

---

### Tính loss

Cross Entropy Loss:

L = -(1/m) Σ Σ [ y log(a2) ]

---

### Backpropagation

dZ2 = A2 - Y

dW2 = (1/m) A1^T dZ2

db2 = (1/m) Σ_rows(dZ2)

dA1 = dZ2 W2^T

dZ1 = dA1 ⊙ A1 ⊙ (1 - A1)

dW1 = (1/m) X^T dZ1

db1 = (1/m) Σ_rows(dZ1)

---

### Cập nhật tham số

W1 = W1 - lr * dW1

b1 = b1 - lr * db1

W2 = W2 - lr * dW2

b2 = b2 - lr * db2

---

3. Lặp lại cho đến khi:

- đạt số epoch yêu cầu
- hoặc loss hội tụ.

---
