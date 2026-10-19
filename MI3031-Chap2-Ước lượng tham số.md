# CHƯƠNG 2: ƯỚC LƯỢNG THAM SỐ
**Giảng viên:** Nguyễn Thị Thu Thủy — Khoa Toán - Tin, Đại học Bách Khoa Hà Nội (2026)  
**Môn học:** Suy luận thống kê (MI3031)

---

## PHẦN 1: ƯỚC LƯỢNG ĐIỂM (POINT ESTIMATION)

### 1.1. Hàm ước lượng và Ước lượng điểm
- **Tham số tổng thể $\theta$:** Đại lượng chưa biết của tổng thể cần ước lượng (ví dụ: $\mu, \sigma^2, p$).
- **Hàm ước lượng (Estimator):** Thống kê $\widehat{\Theta} = g(X_1, \dots, X_n)$ xây dựng từ mẫu ngẫu nhiên để ước lượng $\theta$. Đây là một **biến ngẫu nhiên** và có phân phối xác suất riêng.
- **Ước lượng điểm (Estimate):** Con số thực tế $\hat{\theta} = g(x_1, \dots, x_n)$ thu được khi thay số liệu quan sát cụ thể vào hàm ước lượng.

| Tiêu chí | Hàm ước lượng ($\widehat{\Theta}$) | Ước lượng điểm ($\hat{\theta}$) |
|---|---|---|
| Bản chất | Là một thống kê / biến ngẫu nhiên | Là một con số cụ thể |
| Dữ liệu | Chưa có số liệu mẫu | Đã thay số liệu từ mẫu thực tế |
| Đặc trưng | Có phân phối xác suất riêng | Là kết quả tính toán cụ thể |

---

### 1.2. Tiêu chuẩn đánh giá hàm ước lượng

#### a. Tính không chệch (Unbiasedness)
- Hàm ước lượng $\widehat{\Theta}$ được gọi là **không chệch** nếu:
  $$E(\widehat{\Theta}) = \theta$$
- **Độ chệch (Bias):** $B(\widehat{\Theta}) = E(\widehat{\Theta}) - \theta$. Ước lượng không chệch $\Leftrightarrow B(\widehat{\Theta}) = 0$.
- *Ý nghĩa:* Không có sai số mang tính hệ thống; trung bình lâu dài sẽ tiến về đúng giá trị thực của tham số.

#### b. Tính hiệu quả / Phương sai nhỏ (Efficiency)
- Trong lớp các ước lượng không chệch, ước lượng nào có **phương sai nhỏ nhất** ($V(\widehat{\Theta})$ nhỏ nhất) được gọi là **ước lượng hiệu quả** (Efficient Estimator).
- *Ý nghĩa:* Phân tán tập trung chặt quanh giá trị thực $\theta$, giảm thiểu rủi ro sai lệch lớn.

#### c. Sai số chuẩn (Standard Error)
$$\sigma_{\widehat{\Theta}} = \sqrt{V(\widehat{\Theta})}$$
Đo lường độ chính xác thực tế của hàm ước lượng.

#### d. Sai số bình phương trung bình (MSE - Mean Squared Error)
- Đánh giá chất lượng tổng thể của ước lượng (kể cả khi bị chệch):
  $$\text{MSE}(\widehat{\Theta}) = E[(\widehat{\Theta} - \theta)^2]$$
- **Công thức khai triển kinh điển:**
  $$\text{MSE}(\widehat{\Theta}) = V(\widehat{\Theta}) + B^2(\widehat{\Theta})$$
  $$\text{MSE} = \text{Phương sai} + (\text{Độ chệch})^2$$
- **Hiệu suất tương đối:** Dùng so sánh hai ước lượng $\widehat{\Theta}_1, \widehat{\Theta}_2$:
  $$\text{Hiệu suất} = \frac{\text{MSE}(\widehat{\Theta}_1)}{\text{MSE}(\widehat{\Theta}_2)}$$
  Nếu tỷ số $< 1 \Rightarrow \widehat{\Theta}_1$ tốt hơn $\widehat{\Theta}_2$.

---

### 1.3. Phương pháp xây dựng hàm ước lượng

#### a. Phương pháp Mô men (Method of Moments - MoM) (Karl Pearson, 1894)
- **Tư tưởng:** Cân bằng giữa mô men lý thuyết của tổng thể với mô men mẫu tương ứng:
  - Mô men lý thuyết bậc $k$: $\alpha_k = E(X^k) = g_k(\theta_1, \dots, \theta_m)$
  - Mô men mẫu bậc $k$: $A_k = \frac{1}{n} \sum_{i=1}^n X_i^k$
- **Quy trình:**
  1. Tính $m$ mô men lý thuyết đầu tiên theo các tham số cần ước lượng.
  2. Thiết lập hệ $m$ phương trình: $g_j(\theta_1, \dots, \theta_m) = \frac{1}{n}\sum_{i=1}^n X_i^j$ ($j = 1, \dots, m$).
  3. Giải hệ phương trình để tìm ước lượng $\hat{\theta}_{j, \text{MM}}$.
- **Kết quả điển hình:**
  - Phân phối mũ $X \sim \text{Exp}(\lambda)$: $\hat{\lambda}_{\text{MM}} = \frac{1}{\bar{X}}$.
  - Phân phối chuẩn $X \sim \mathcal{N}(\mu, \sigma^2)$:
    $$\hat{\mu}_{\text{MM}} = \bar{X}, \quad \hat{\sigma}^2_{\text{MM}} = \frac{1}{n}\sum_{i=1}^n (X_i - \bar{X})^2$$
    *(Lưu ý: $\hat{\sigma}^2_{\text{MM}}$ chia cho $n$ nên là ước lượng chệch của $\sigma^2$).*

#### b. Phương pháp Hợp lý cực đại (Maximum Likelihood Estimation - MLE) (R. A. Fisher, 1922)
- **Tư tưởng:** Chọn giá trị tham số $\theta$ sao cho xác suất (hoặc mật độ) quan sát được mẫu hiện tại là lớn nhất:
  $$\hat{\theta}_{\text{MLE}} = \arg\max_\theta L(\theta)$$
- **Hàm hợp lý $L(\theta)$:** $L(\theta) = \prod_{i=1}^n f(x_i; \theta)$ (liên tục) hoặc $\prod_{i=1}^n p(x_i; \theta)$ (rời rạc).
- **Hàm Log-hợp lý $\ell(\theta)$:** $\ell(\theta) = \ln L(\theta) = \sum_{i=1}^n \ln f(x_i; \theta)$.
- **Quy trình:**
  1. Viết $L(\theta)$ và lấy logarit $\ell(\theta)$.
  2. Giải phương trình hợp lý: $\ell'(\theta) = 0$ (hoặc hệ đạo hàm riêng $\nabla \ell = 0$).
  3. Kiểm tra điều kiện đủ (đạo hàm cấp 2 mang dấu âm).
  4. Thay các quan sát $x_i$ bằng biến ngẫu nhiên $X_i$ để thu được $\widehat{\Theta}_{\text{MLE}}$.
- **Kết quả điển hình:**
  - Phân phối Bernoulli $X \sim \text{Bernoulli}(p)$: $\hat{p}_{\text{MLE}} = \bar{X} = \frac{1}{n}\sum_{i=1}^n X_i$ (chính là tần suất mẫu).
  - Phân phối chuẩn $X \sim \mathcal{N}(\mu, \sigma^2)$:
    $$\hat{\mu}_{\text{MLE}} = \bar{X}, \quad \hat{\sigma}^2_{\text{MLE}} = \frac{1}{n}\sum_{i=1}^n (X_i - \bar{X})^2 = \frac{n-1}{n} S^2$$
    *(Lưu ý: $\hat{\sigma}^2_{\text{MLE}}$ là ước lượng chệch nhưng thỏa mãn tính vững).*

---

## PHẦN 2: KHOẢNG TIN CẬY (CONFIDENCE INTERVALS)

### 2.1. Khái niệm và Cấu trúc chung

#### Định nghĩa Khoảng tin cậy
- Tìm hai thống kê $\widehat{\Theta}_1, \widehat{\Theta}_2$ sao cho:
  $$P(\widehat{\Theta}_1 < \theta < \widehat{\Theta}_2) = 1 - \alpha$$
- Với mẫu cụ thể, khoảng thực $(\hat{\theta}_1, \hat{\theta}_2)$ được gọi là **khoảng tin cậy $100(1-\alpha)\%$** cho tham số $\theta$.
  - $\hat{\theta}_1$: Cận dưới
  - $\hat{\theta}_2$: Cận trên
  - Độ dài khoảng: $I = \hat{\theta}_2 - \hat{\theta}_1$
- **Ý nghĩa thực sự của mức tin cậy $1-\alpha$ (ví dụ 95%):** Nếu lặp lại quy trình lấy mẫu độc lập nhiều lần và dựng khoảng theo cùng phương pháp, thì khoảng 95% số khoảng được xây dựng sẽ chứa tham số thật $\theta$. Mức tin cậy nói về **tần suất thành công của quy trình**, không phải xác suất tham số $\theta$ rơi vào một khoảng cụ thể đã quan sát.
- **Bài toán đánh đổi (Trade-off):** Độ tin cậy tăng ($1-\alpha \uparrow$) $\Rightarrow$ khoảng ước lượng rộng ra (độ chính xác giảm).

#### Cấu trúc tổng quát của khoảng tin cậy đối xứng
$$\text{Khoảng tin cậy} = \text{Ước lượng điểm} \pm (\text{Hệ số tới hạn}) \times (\text{Sai số chuẩn})$$
$$\bar{x} - E < \theta < \bar{x} + E$$
Trong đó $E = (\text{Hệ số tới hạn}) \times (\text{Sai số chuẩn})$ gọi là **sai số biên** (Margin of Error).

---

### 2.2. Các bài toán ước lượng cụ thể

---

### Bài toán 1: Khoảng tin cậy cho Kỳ vọng $\mu$ (Một tổng thể)

#### Trường hợp 1: Đã biết phương sai $\sigma^2$ (Định lý 1)
- **Điều kiện:** Tổng thể chuẩn $\mathcal{N}(\mu, \sigma^2)$, đã biết $\sigma^2$ (hoặc tổng thể bất kỳ với mẫu lớn $n \ge 30$ theo CLT).
- **Thống kê:** $Z = \frac{\bar{X} - \mu}{\sigma / \sqrt{n}} \sim \mathcal{N}(0, 1)$.
- **KTC hai phía 95%:**
  $$\bar{x} - z_{\alpha/2} \frac{\sigma}{\sqrt{n}} < \mu < \bar{x} + z_{\alpha/2} \frac{\sigma}{\sqrt{n}}$$
  Với $1-\alpha = 0.95 \Rightarrow z_{0.025} = 1.96$.
- **Cỡ mẫu tối thiểu (Định lý 2):** Để sai số không vượt quá $\varepsilon$:
  $$n \ge \frac{\sigma^2 (z_{\alpha/2})^2}{\varepsilon^2}$$
- **KTC một phía (Định lý 3):**
  - Cận trên (khoảng trái): $-\infty < \mu < \bar{x} + z_\alpha \frac{\sigma}{\sqrt{n}}$
  - Cận dưới (khoảng phải): $\bar{x} - z_\alpha \frac{\sigma}{\sqrt{n}} < \mu < +\infty$

#### Trường hợp 2: Mẫu lớn ($n \ge 40$), chưa biết $\sigma^2$ (Định lý 4)
- **Điều kiện:** Biến ngẫu nhiên có phân phối bất kỳ, $\sigma^2$ chưa biết, cỡ mẫu lớn ($n \ge 40$). Thay $\sigma$ bằng độ lệch chuẩn mẫu hiệu chỉnh $s$ theo CLT.
- **KTC hai phía 95%:**
  $$\bar{x} - z_{\alpha/2} \frac{s}{\sqrt{n}} < \mu < \bar{x} + z_{\alpha/2} \frac{s}{\sqrt{n}}$$
- **KTC một phía (Định lý 5):**
  - Cận trên: $-\infty < \mu < \bar{x} + z_\alpha \frac{s}{\sqrt{n}}$
  - Cận dưới: $\bar{x} - z_\alpha \frac{s}{\sqrt{n}} < \mu < +\infty$

#### Trường hợp 3: Tổng thể chuẩn, chưa biết phương sai $\sigma^2$ (Định lý 6)
- **Điều kiện:** Tổng thể có phân phối chuẩn $\mathcal{N}(\mu, \sigma^2)$ (hoặc tiệm cận chuẩn), $\sigma^2$ chưa biết.
- **Thống kê:** $T = \frac{\bar{X} - \mu}{S / \sqrt{n}} \sim t(n-1)$.
- **KTC hai phía 95%:**
  $$\bar{x} - t_{\alpha/2}^{(n-1)} \frac{s}{\sqrt{n}} < \mu < \bar{x} + t_{\alpha/2}^{(n-1)} \frac{s}{\sqrt{n}}$$
  Tra bảng phân phối Student $t$ với bậc tự do $\nu = n-1$.
- **Cỡ mẫu tối thiểu:** Dùng mẫu sơ bộ tính $s$, khi $n$ lớn thì $t_{\alpha/2}^{(n-1)} \approx z_{\alpha/2} \Rightarrow n \ge \frac{s^2 (z_{\alpha/2})^2}{\varepsilon^2}$.
- **KTC một phía (Định lý 7):**
  - Cận trên: $-\infty < \mu < \bar{x} + t_\alpha^{(n-1)} \frac{s}{\sqrt{n}}$
  - Cận dưới: $\bar{x} - t_\alpha^{(n-1)} \frac{s}{\sqrt{n}} < \mu < +\infty$

---

### Bài toán 2: Khoảng tin cậy cho Phương sai $\sigma^2$ (Một tổng thể)

#### KTC hai phía (Định lý 8)
- **Điều kiện (bắt buộc):** Tổng thể phải tuân theo phân phối chuẩn $\mathcal{N}(\mu, \sigma^2)$. Cả $\mu$ và $\sigma^2$ chưa biết.
- **Thống kê:** $K^2 = \frac{(n-1)S^2}{\sigma^2} \sim \chi^2(n-1)$.
- **Công thức KTC 95% cho phương sai $\sigma^2$:**
  $$\frac{(n-1)s^2}{\chi^2_{\alpha/2, \, n-1}} < \sigma^2 < \frac{(n-1)s^2}{\chi^2_{1-\alpha/2, \, n-1}}$$
- **KTC cho độ lệch chuẩn $\sigma$:** Lấy căn bậc hai hai cận:
  $$\sqrt{\frac{(n-1)s^2}{\chi^2_{\alpha/2, \, n-1}}} < \sigma < \sqrt{\frac{(n-1)s^2}{\chi^2_{1-\alpha/2, \, n-1}}}$$
  *(Lưu ý: Mẫu số ở cận dưới là $\chi^2_{\alpha/2}$ - giá trị lớn hơn; mẫu số ở cận trên là $\chi^2_{1-\alpha/2}$ - giá trị nhỏ hơn).*

#### KTC một phía cho phương sai (Định lý 9)
- Cận trên (khoảng trái): $0 < \sigma^2 < \frac{(n-1)s^2}{\chi^2_{1-\alpha, \, n-1}}$
- Cận dưới (khoảng phải): $\frac{(n-1)s^2}{\chi^2_{\alpha, \, n-1}} < \sigma^2 < +\infty$

---

### Bài toán 3: Khoảng tin cậy cho Tỷ lệ $p$ (Một tổng thể)

#### KTC hai phía (Định lý 10)
- **Điều kiện:** Cỡ mẫu lớn thỏa mãn điều kiện xấp xỉ chuẩn: $n\hat{p} \ge 5$ và $n(1-\hat{p}) \ge 5$.
- **Ước lượng điểm:** Tần suất mẫu $\hat{p} = \frac{y}{n}$ ($y$ là số phần tử mang dấu hiệu trong mẫu).
- **Thống kê:** $Z = \frac{\widehat{P} - p}{\sqrt{p(1-p)/n}} \xrightarrow{d} \mathcal{N}(0, 1)$.
- **Công thức KTC 95%:**
  $$\hat{p} - z_{\alpha/2} \sqrt{\frac{\hat{p}(1-\hat{p})}{n}} < p < \hat{p} + z_{\alpha/2} \sqrt{\frac{\hat{p}(1-\hat{p})}{n}}$$
  Sai số biên: $E = z_{\alpha/2} \sqrt{\frac{\hat{p}(1-\hat{p})}{n}}$.

#### Chọn cỡ mẫu tối thiểu:
- **Cách 1 (có mẫu sơ bộ $\hat{p}_k$):** $n \ge \frac{(z_{\alpha/2})^2 \hat{p}_k (1 - \hat{p}_k)}{\varepsilon^2}$
- **Cách 2 (thận trọng tối đa, không cần mẫu sơ bộ, lấy $p=0.5$):**
  $$n \ge \frac{(z_{\alpha/2})^2}{4\varepsilon^2}$$

#### KTC một phía (slide 67):
- Cận trên: $0 < p < \hat{p} + z_\alpha \sqrt{\frac{\hat{p}(1-\hat{p})}{n}}$
- Cận dưới: $\hat{p} - z_\alpha \sqrt{\frac{\hat{p}(1-\hat{p})}{n}} < p < 1$

---

### Bài toán 4: Khoảng tin cậy cho Hiệu hai kỳ vọng $\mu_1 - \mu_2$ (Hai tổng thể)

#### Trường hợp 1: Đã biết phương sai $\sigma_1^2, \sigma_2^2$ (Định lý 11)
- Hai mẫu độc lập, tổng thể chuẩn hoặc mẫu lớn theo CLT:
  $$(\bar{x}_1 - \bar{x}_2) \pm z_{\alpha/2} \sqrt{\frac{\sigma_1^2}{n_1} + \frac{\sigma_2^2}{n_2}}$$

#### Trường hợp 2: Chưa biết phương sai nhưng bằng nhau ($\sigma_1^2 = \sigma_2^2 = \sigma^2$) (Định lý 12)
- **Điều kiện:** Hai tổng thể chuẩn, hai mẫu độc lập, phương sai bằng nhau nhưng chưa biết.
- **Phương sai gộp (Pooled variance):**
  $$s_p^2 = \frac{(n_1 - 1)s_1^2 + (n_2 - 1)s_2^2}{n_1 + n_2 - 2}$$
- **Bậc tự do:** $\nu = n_1 + n_2 - 2$.
- **KTC hai phía 95%:**
  $$(\bar{x}_1 - \bar{x}_2) \pm t_{\alpha/2, \, \nu} \cdot s_p \sqrt{\frac{1}{n_1} + \frac{1}{n_2}}$$

#### Trường hợp 3: Chưa biết phương sai và khác nhau ($\sigma_1^2 \ne \sigma_2^2$) (Định lý 13)
- **Điều kiện:** Hai tổng thể chuẩn, hai mẫu độc lập, phương sai chưa biết và không bằng nhau.
- **Phép xấp xỉ Satterthwaite:** Thống kê tuân theo phân phối $t(\nu)$ với bậc tự do xấp xỉ:
  $$\nu = \frac{\left(\frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}\right)^2}{\frac{(s_1^2/n_1)^2}{n_1-1} + \frac{(s_2^2/n_2)^2}{n_2-1}} \quad \text{(làm tròn xuống số nguyên gần nhất)}$$
- **KTC hai phía 95%:**
  $$(\bar{x}_1 - \bar{x}_2) \pm t_{\alpha/2, \, \nu} \sqrt{\frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}}$$
- *(Mở rộng mẫu lớn theo CLT):* Khi $n_1 \ge 30, n_2 \ge 30$, thống kê hội tụ về chuẩn tắc nên có thể dùng $z_{\alpha/2}$ thay cho $t_{\alpha/2, \nu}$.

#### Trường hợp 4: Mẫu cặp (Paired samples) (Định lý 14)
- Khi các quan sát trên 2 tổng thể theo cặp (trước - sau).
- Đặt $Z_j = X_{1j} - X_{2j}$, quy về bài toán ước lượng kỳ vọng một mẫu cho biến $Z$:
  $$\bar{z} \pm t_{\alpha/2, \, n-1} \frac{s_Z}{\sqrt{n}}$$

---

### Bài toán 5: Khoảng tin cậy cho Tỷ lệ hai phương sai $\sigma_1^2 / \sigma_2^2$ (Định lý 15)
- **Điều kiện:** Hai mẫu độc lập từ hai tổng thể chuẩn.
- **KTC hai phía 95%:**
  $$\frac{s_1^2}{s_2^2} \frac{1}{f_{\alpha/2, \, \nu_1, \, \nu_2}} < \frac{\sigma_1^2}{\sigma_2^2} < \frac{s_1^2}{s_2^2} f_{\alpha/2, \, \nu_2, \, \nu_1}$$
  Trong đó $\nu_1 = n_1 - 1, \nu_2 = n_2 - 1$.
- *Ý nghĩa kiểm tra giả thiết phương sai bằng nhau:* Nếu khoảng chứa giá trị 1 thì chưa đủ bằng chứng kết luận 2 phương sai khác nhau.

---

### Bài toán 6: Khoảng tin cậy cho Hiệu hai tỷ lệ $p_1 - p_2$ (Định lý 16)
- **Điều kiện:** Hai mẫu độc lập kích thước lớn ($n_i\hat{p}_i \ge 5$ và $n_i(1-\hat{p}_i) \ge 5$).
- **KTC hai phía 95%:**
  $$(\hat{p}_1 - \hat{p}_2) \pm z_{\alpha/2} \sqrt{\frac{\hat{p}_1(1 - \hat{p}_1)}{n_1} + \frac{\hat{p}_2(1 - \hat{p}_2)}{n_2}}$$
- *Ý nghĩa:* Nếu khoảng chứa 0 thì chưa đủ cơ sở khẳng định có sự khác biệt giữa hai tỷ lệ ở mức ý nghĩa $\alpha$.
