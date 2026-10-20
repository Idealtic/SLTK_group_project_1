# CHƯƠNG 1: MẪU NGẪU NHIÊN VÀ PHÂN PHỐI MẪU
**Giảng viên:** Nguyễn Thị Thu Thủy — Khoa Toán - Tin, Đại học Bách Khoa Hà Nội (2026)  
**Môn học:** Suy luận thống kê (MI3031 / MI2020)

---

## 1. MẪU NGẪU NHIÊN

### 1.1. Khái niệm mẫu ngẫu nhiên và hàm thống kê

#### Biến ngẫu nhiên gốc và tổng thể
- Cần nghiên cứu một dấu hiệu $\mathcal{X}$ của tổng thể có kỳ vọng $E(\mathcal{X}) = \mu$ và phương sai $V(\mathcal{X}) = \sigma^2$ ($\mu, \sigma$ chưa biết).
- Lấy ngẫu nhiên một phần tử từ tổng thể và gọi $X$ là giá trị dấu hiệu $\mathcal{X}$ đo được trên phần tử đó $\Rightarrow X$ là biến ngẫu nhiên, gọi là **biến ngẫu nhiên gốc**.
- Phân phối xác suất của $X$ gọi là **phân phối gốc** $F_X(x)$, với $E(X) = \mu$ và $V(X) = \sigma^2$.

#### Định nghĩa Mẫu ngẫu nhiên (Định nghĩa 1)
- Cho biến ngẫu nhiên gốc $X$ có hàm phân phối xác suất $F_X(x)$. Một **mẫu ngẫu nhiên kích thước $n$** lập từ $X$ là tập hợp gồm $n$ biến ngẫu nhiên $X_1, X_2, \dots, X_n$ độc lập và có cùng phân phối xác suất $F_X(x)$ (i.i.d).
- Ký hiệu mẫu ngẫu nhiên: $W_X = (X_1, X_2, \dots, X_n)$.
- **Thể hiện (mẫu cụ thể):** Bộ gồm $n$ số thực cụ thể $W_x = (x_1, x_2, \dots, x_n)$ là kết quả quan sát đo đạc trên mẫu ngẫu nhiên.

#### Định nghĩa Hàm thống kê (Định nghĩa 2)
- Cho mẫu ngẫu nhiên $W_X = (X_1, X_2, \dots, X_n)$ kích thước $n$. Bất kỳ hàm thực nào của các biến ngẫu nhiên này:
  $$\widehat{\Theta} = g(X_1, X_2, \dots, X_n)$$
  được gọi là một **hàm thống kê** (gọi tắt là **thống kê**).
- Vì là hàm của các biến ngẫu nhiên nên thống kê $\widehat{\Theta}$ cũng là một biến ngẫu nhiên.
- Khi có mẫu cụ thể $W_x = (x_1, \dots, x_n)$, giá trị tính được $\hat{\theta} = g(x_1, \dots, x_n)$ được gọi là **giá trị quan sát** của thống kê $\widehat{\Theta}$.

---

### 1.2. Một số thống kê thông dụng

#### a. Trung bình mẫu (Sample Mean)
- **Định nghĩa (Định nghĩa 3):**
  $$\bar{X} = \frac{1}{n} \sum_{i=1}^n X_i$$
  Giá trị quan sát: $\bar{x} = \frac{1}{n}\sum_{i=1}^n x_i$.
- **Kỳ vọng và phương sai:**
  $$E(\bar{X}) = \mu$$
  $$V(\bar{X}) = \frac{\sigma^2}{n}$$

#### b. Phương sai mẫu (Sample Variance)
- **Định nghĩa (Định nghĩa 4):**
  $$\widetilde{S}^2 = \frac{1}{n} \sum_{i=1}^n (X_i - \bar{X})^2$$
  Giá trị quan sát: $\tilde{s}^2 = \frac{1}{n}\sum_{i=1}^n (x_i - \bar{x})^2$.
- **Kỳ vọng:**
  $$E(\widetilde{S}^2) = \frac{n-1}{n} \sigma^2$$
  *(Do $E(\widetilde{S}^2) \neq \sigma^2$, $\widetilde{S}^2$ là ước lượng chệch của $\sigma^2$).*

#### c. Phương sai mẫu hiệu chỉnh (Sample Variance - Unbiased)
- **Định nghĩa (Công thức 7, 8):** Nhân thêm hệ số hiệu chỉnh $\frac{n}{n-1}$:
  $$S^2 = \frac{n}{n-1}\widetilde{S}^2 = \frac{1}{n-1} \sum_{i=1}^n (X_i - \bar{X})^2$$
  Giá trị quan sát:
  $$s^2 = \frac{1}{n-1} \sum_{i=1}^n (x_i - \bar{x})^2$$
- **Kỳ vọng:**
  $$E(S^2) = \sigma^2$$
  *(Phương sai mẫu hiệu chỉnh là ước lượng không chệch của $\sigma^2$).*

#### d. Độ lệch chuẩn mẫu và độ lệch chuẩn mẫu hiệu chỉnh
- Độ lệch chuẩn mẫu: $\widetilde{S} = \sqrt{\widetilde{S}^2} = \sqrt{\frac{1}{n}\sum_{i=1}^n (X_i - \bar{X})^2}$
- Độ lệch chuẩn mẫu hiệu chỉnh: $S = \sqrt{S^2} = \sqrt{\frac{1}{n-1}\sum_{i=1}^n (X_i - \bar{X})^2}$

#### e. Tần suất mẫu (Sample Proportion)
- Nghiên cứu dấu hiệu định tính $A$, mỗi cá thể có hoặc không có dấu hiệu này.
- Gọi $p$ là tỷ lệ cá thể có dấu hiệu $A$ trong tổng thể. Mô hình hóa bằng biến ngẫu nhiên Bernoulli $X \sim \text{Bernoulli}(p)$ với $E(X) = p, V(X) = p(1-p)$.
- Số cá thể có dấu hiệu $A$ trong mẫu là $Y = \sum_{i=1}^n X_i$.
- **Thống kê tần suất mẫu:**
  $$\widehat{P} = \frac{Y}{n} = \frac{1}{n}\sum_{i=1}^n X_i$$
  Giá trị quan sát: $\hat{p} = \frac{y}{n}$.
- **Kỳ vọng và phương sai:**
  $$E(\widehat{P}) = p, \quad V(\widehat{P}) = \frac{p(1-p)}{n}$$

---

## 2. PHÂN PHỐI MẪU (SAMPLING DISTRIBUTIONS)

### 2.1. Phân phối mẫu của Trung bình mẫu $\bar{X}$

#### Trường hợp tổng thể phân phối chuẩn (Định lý 1)
Nếu biến ngẫu nhiên gốc $X \sim \mathcal{N}(\mu, \sigma^2)$ thì:
$$\bar{X} \sim \mathcal{N}\left(\mu, \frac{\sigma^2}{n}\right)$$
Thống kê chuẩn hóa:
$$Z = \frac{\bar{X} - \mu}{\sigma / \sqrt{n}} \sim \mathcal{N}(0, 1)$$

#### Trường hợp tổng thể bất kỳ — Định lý Giới hạn Trung tâm (CLT - Định lý 2)
- Nếu mẫu ngẫu nhiên kích thước $n$ được lấy từ tổng thể có kỳ vọng $\mu$ và phương sai $\sigma^2 < \infty$ bất kỳ, thì khi $n \to \infty$, thống kê:
  $$Z = \frac{\bar{X} - \mu}{\sigma / \sqrt{n}} \xrightarrow{d} \mathcal{N}(0, 1)$$
- **Quy tắc thực hành:** Xấp xỉ chuẩn được xem là tốt khi **$n \ge 30$** (nếu phân phối gốc không quá lệch chuẩn).

---

### 2.2. Phân phối mẫu của Hiệu hai trung bình mẫu $\bar{X}_1 - \bar{X}_2$ (Định lý 3)
- Cho hai mẫu ngẫu nhiên độc lập kích thước $n_1, n_2$ từ hai tổng thể độc lập có kỳ vọng $\mu_1, \mu_2$ và phương sai $\sigma_1^2, \sigma_2^2$.
- Kỳ vọng và phương sai của hiệu:
  $$\mu_{\bar{X}_1 - \bar{X}_2} = \mu_1 - \mu_2$$
  $$\sigma^2_{\bar{X}_1 - \bar{X}_2} = \frac{\sigma_1^2}{n_1} + \frac{\sigma_2^2}{n_2}$$
- Thống kê chuẩn hóa:
  $$Z = \frac{(\bar{X}_1 - \bar{X}_2) - (\mu_1 - \mu_2)}{\sqrt{\frac{\sigma_1^2}{n_1} + \frac{\sigma_2^2}{n_2}}}$$
  - Có phân phối $\mathcal{N}(0, 1)$ nếu hai tổng thể có phân phối chuẩn.
  - Xấp xỉ $\mathcal{N}(0, 1)$ theo CLT nếu $n_1 \ge 30$ và $n_2 \ge 30$.

---

### 2.3. Xấp xỉ chuẩn cho Phân phối Nhị thức
- Với biến nhị thức $Y \sim \text{Bin}(n, p)$, khi $n$ đủ lớn thì $Y$ và $\widehat{P} = Y/n$ xấp xỉ phân phối chuẩn:
  $$\widehat{P} \approx \mathcal{N}\left(p, \frac{p(1-p)}{n}\right)$$
- **Điều kiện xấp xỉ chuẩn:** $np \ge 5$ và $n(1-p) \ge 5$.

---

### 2.4. Phân phối Khi-bình phương ($\chi^2$) và Phân phối mẫu của $S^2$

#### Hàm Gamma (Định nghĩa 6)
$$\Gamma(r) = \int_0^{+\infty} x^{r-1} e^{-x} dx \quad (r > 0)$$
Tính chất: $\Gamma(r) = (r-1)\Gamma(r-1)$, $\Gamma(n) = (n-1)!$, $\Gamma(1/2) = \sqrt{\pi}$.

#### Phân phối Khi-bình phương (Định nghĩa 7)
- Biến ngẫu nhiên $X \sim \chi^2(\nu)$ với $\nu$ bậc tự do có hàm mật độ:
  $$f_X(x) = \begin{cases} \frac{1}{2^{\nu/2}\Gamma(\nu/2)} x^{(\nu/2)-1} e^{-x/2}, & x > 0 \\ 0, & x \le 0 \end{cases}$$
- **Kỳ vọng và phương sai (Định lý 4):**
  $$E(X) = \nu, \quad V(X) = 2\nu$$
- Đồ thị luôn lệch phải. Khi $\nu \to \infty$, phân phối $\chi^2(\nu)$ tiến về phân phối chuẩn.

#### Các tính chất quan trọng:
- **Định lý 5:** Nếu $X_1, \dots, X_n \stackrel{\text{i.i.d}}{\sim} \mathcal{N}(0, 1)$ thì $Z_n = \sum_{i=1}^n X_i^2 \sim \chi^2(n)$.
- **Định lý 6:** Nếu $X_i \sim \chi^2(\nu_i)$ độc lập thì $\sum X_i \sim \chi^2(\sum \nu_i)$.
- **Hệ quả 1:** Nếu $X_i \stackrel{\text{i.i.d}}{\sim} \mathcal{N}(\mu, \sigma^2)$ thì $\sum_{i=1}^n \left(\frac{X_i - \mu}{\sigma}\right)^2 \sim \chi^2(n)$.

#### Phân phối mẫu của $S^2$ (Định lý 7)
Nếu $S^2$ là phương sai mẫu hiệu chỉnh từ mẫu kích thước $n$ của tổng thể chuẩn $\mathcal{N}(\mu, \sigma^2)$, thì thống kê:
$$K^2 = \frac{(n-1)S^2}{\sigma^2} = \sum_{i=1}^n \frac{(X_i - \bar{X})^2}{\sigma^2} \sim \chi^2(n-1)$$
Giá trị quan sát: $k^2 = \frac{(n-1)s^2}{\sigma^2}$.

---

### 2.5. Phân phối Student ($t$) và Phân phối mẫu của $T$

#### Định nghĩa Phân phối $t$ (Định nghĩa 8)
- Biến ngẫu nhiên $X \sim t(\nu)$ có $\nu$ bậc tự do có hàm mật độ:
  $$f_X(x) = \left(1 + \frac{x^2}{\nu}\right)^{-\frac{\nu+1}{2}} \frac{\Gamma\left(\frac{\nu+1}{2}\right)}{\sqrt{\nu\pi}\,\Gamma\left(\frac{\nu}{2}\right)}, \quad -\infty < x < +\infty$$
- **Kỳ vọng và phương sai (Định lý 8):**
  $$E(X) = 0, \quad V(X) = \frac{\nu}{\nu - 2} \quad (\nu > 2)$$
- Đồ thị đối xứng qua 0, hình chuông nhưng đuôi nặng hơn phân phối chuẩn. Khi $\nu \ge 30$, phân phối $t(\nu)$ tiệm cận rất nhanh về $\mathcal{N}(0, 1)$.

#### Mối liên hệ cốt lõi (Định lý 9)
Nếu $Z \sim \mathcal{N}(0, 1)$ và $U \sim \chi^2(\nu)$ là hai biến ngẫu nhiên độc lập thì:
$$T = \frac{Z}{\sqrt{U / \nu}} \sim t(\nu)$$

#### Phân phối mẫu của thống kê $T$ (Hệ quả 3)
Nếu $X_1, \dots, X_n \stackrel{\text{i.i.d}}{\sim} \mathcal{N}(\mu, \sigma^2)$ với $\sigma^2$ **chưa biết**, thì thống kê:
$$T = \frac{\bar{X} - \mu}{S / \sqrt{n}} \sim t(n-1)$$
*(Trong đó $\bar{X}$ và $S^2$ độc lập với nhau khi tổng thể phân phối chuẩn).*

---

### 2.6. Phân phối Fisher ($F$) với hai phương sai mẫu

#### Định nghĩa Phân phối $F$ (Định nghĩa 9, 12)
Nếu $U \sim \chi^2(\nu_1)$ và $V \sim \chi^2(\nu_2)$ độc lập thì:
$$F = \frac{U / \nu_1}{V / \nu_2} \sim F(\nu_1, \nu_2)$$
- $\nu_1$: Bậc tự do của tử số; $\nu_2$: Bậc tự do của mẫu số.
- **Tính chất đảo bậc tự do (Định lý 11):**
  $$f_{1-\alpha}(\nu_1, \nu_2) = \frac{1}{f_\alpha(\nu_2, \nu_1)}$$

#### Phân phối mẫu của tỷ lệ phương sai (Định lý 13)
Cho hai mẫu độc lập từ hai tổng thể chuẩn $\mathcal{N}(\mu_1, \sigma_1^2)$ và $\mathcal{N}(\mu_2, \sigma_2^2)$:
$$F = \frac{S_1^2 / \sigma_1^2}{S_2^2 / \sigma_2^2} = \frac{\sigma_2^2 S_1^2}{\sigma_1^2 S_2^2} \sim F(n_1 - 1, \, n_2 - 1)$$

---

## 3. CÁC ĐẠI LƯỢNG VÀ KÝ HIỆU PHÂN VỊ TRA BẢNG

1. **Phân vị chuẩn tắc $z_\alpha$:** Giá trị thỏa mãn $P(Z > z_\alpha) = \alpha$, với $\Phi(z) = P(Z < z) = 1 - \alpha$.
   - $1 - \alpha = 0.95 \Rightarrow z_{0.025} = 1.96$
   - $1 - \alpha = 0.99 \Rightarrow z_{0.005} = 2.576 \approx 2.58$
2. **Phân vị Student $t_\alpha(\nu)$:** Thỏa mãn $P(T > t_\alpha(\nu)) = \alpha$. Tính đối xứng: $t_{1-\alpha}(\nu) = -t_\alpha(\nu)$.
3. **Phân vị Khi-bình phương $\chi^2_\alpha(\nu)$:** Thỏa mãn $P(X > \chi^2_\alpha(\nu)) = \alpha$.
4. **Phân vị Fisher $f_\alpha(\nu_1, \nu_2)$:** Thỏa mãn $P(F > f_\alpha(\nu_1, \nu_2)) = \alpha$.
