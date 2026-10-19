# SPEC: Bài tập nhóm Suy luận thống kê
## Chủ đề: Phân tích chỉ số tài chính các công ty thuộc rổ S&P 500

---

## Thông tin chung

- **Hình thức nộp:** Jupyter Notebook (`.ipynb`) + file dữ liệu `financials.csv`
- **Ngôn ngữ:** Python (môi trường Anaconda, env `sltk_project`)
- **Thư viện cần cài:** `pandas`, `numpy`, `scipy`, `matplotlib`, `seaborn`
- **Nguồn dữ liệu:** [S&P 500 Companies with Financial Information](https://www.kaggle.com/datasets/paytonfisher/sp-500-companies-with-financial-information) trên Kaggle
- **File dữ liệu:** `financials.csv` — 505 dòng, 14 cột

---

## Cấu trúc Notebook

Notebook gồm **6 phần lớn**, mỗi phần gồm nhiều cell Markdown + Code xen kẽ.

---

### Phần 0: Setup & Import

**Cell 0.1 (Code):** Import thư viện và đọc dữ liệu

- Import: `pandas`, `numpy`, `math`, `scipy.stats` (chỉ dùng để tra bảng phân vị `norm.ppf`, `t.ppf`, `chi2.ppf`), `matplotlib.pyplot`, `seaborn`
- Đọc file: `pd.read_csv('financials.csv')`
- Hiển thị `df.head()`, `df.shape`, `df.info()`

---

### Phần 1: Mô tả bộ dữ liệu (Yêu cầu 1 & 2 của đề bài)

**Cell 1.1 (Markdown):** Giới thiệu bộ dữ liệu

Nội dung cần trình bày:

- **Nguồn dữ liệu:** Bộ dữ liệu "S&P 500 Companies with Financial Information" trên Kaggle, gồm 505 quan sát (công ty) và 14 thuộc tính tài chính.
- **Bối cảnh & Ý nghĩa:**
  - S&P 500 là rổ chỉ số gồm 500 công ty có vốn hóa lớn nhất trên thị trường chứng khoán Mỹ (NYSE, NASDAQ).
  - Bộ dữ liệu phản ánh bức tranh định giá và chính sách cổ tức của các doanh nghiệp vốn hóa lớn nhất thị trường Mỹ.
  - **Tổng thể nghiên cứu:** Toàn bộ các doanh nghiệp vốn hóa lớn tại Mỹ. Các công ty trong S&P 500 được xem là **mẫu đại diện** cho tổng thể này.
- **Giải thích các khái niệm chuyên ngành:**
  - **P/E (Price-to-Earnings Ratio):** Hệ số giá trên thu nhập — cho biết nhà đầu tư sẵn sàng trả bao nhiêu đồng cho 1 đồng lợi nhuận của công ty. P/E cao → thị trường kỳ vọng công ty tăng trưởng mạnh trong tương lai. P/E âm → công ty đang thua lỗ (EPS âm), không có ý nghĩa định giá.
  - **Dividend Yield (Tỷ suất cổ tức):** Tỷ lệ giữa cổ tức hàng năm trên giá cổ phiếu. Dividend Yield = 0 nghĩa là công ty không trả cổ tức (thường là các công ty tăng trưởng, tái đầu tư toàn bộ lợi nhuận).
  - **Market Cap (Vốn hóa thị trường):** Giá trị thị trường của toàn bộ cổ phiếu đang lưu hành = Giá cổ phiếu × Số lượng cổ phiếu.
  - **EBITDA:** Lợi nhuận trước lãi vay, thuế và khấu hao — thước đo năng lực sinh lời từ hoạt động kinh doanh cốt lõi.
  - **Sector (Ngành):** Phân loại ngành nghề hoạt động chính của công ty (ví dụ: Information Technology, Health Care, Financials,...).

- **Phân loại biến sử dụng trong bài:**

| Biến | Loại | Mô tả | Vai trò trong bài |
|---|---|---|---|
| `Price/Earnings` | Định lượng (liên tục) | Hệ số P/E | Biến chính cho câu 3a, 3b, 3d |
| `Market Cap` | Định lượng (liên tục) | Vốn hóa thị trường (USD) | Biến định lượng thứ 2 (đáp ứng yêu cầu "ít nhất 2 biến định lượng") |
| `Pay_Dividend` | Định tính (nhị phân) | = 1 nếu `Dividend Yield > 0`, = 0 nếu `Dividend Yield = 0` | Biến cho câu 3c |
| `Sector` | Định tính (danh mục) | Ngành nghề hoạt động | Biến phân nhóm cho câu 3d |

**Cell 1.2 (Code):** Tiền xử lý dữ liệu

Các bước xử lý:
1. Chọn các cột cần thiết: `Symbol`, `Name`, `Sector`, `Price/Earnings`, `Dividend Yield`, `Market Cap`
2. Loại bỏ các dòng có `Price/Earnings` bị missing (NaN) → loại 2 dòng
3. Loại bỏ các dòng có `Price/Earnings <= 0` (công ty thua lỗ, P/E âm không có ý nghĩa) → loại 11 dòng
4. Loại bỏ các điểm ngoại lai với `Price/Earnings > 100` (P/E cực cao gây méo kỳ vọng mẫu) → loại 12 dòng
5. Tạo cột mới `Pay_Dividend`: `= 1` nếu `Dividend Yield > 0`, `= 0` nếu `Dividend Yield == 0`
6. In ra cỡ mẫu sau khi lọc (kỳ vọng: ~480 dòng), thống kê mô tả cơ bản (`describe()`)

**Lý giải chi tiết về các quyết định tiền xử lý dữ liệu:**

- **Tại sao loại bỏ `Price/Earnings` bị missing (NaN) (2 dòng):**
  - *Toán học & Lập trình:* Không thể tính trung bình mẫu $\bar{X} = \frac{1}{n}\sum X_i$ hay phương sai mẫu $S^2$ khi mảng dữ liệu chứa giá trị khuyết thiếu (phép toán sẽ trả về `NaN`).
  - *Thống kê:* Số lượng missing cực nhỏ (2/505 dòng, < 0.4%). Việc loại bỏ trực tiếp (listwise deletion) hoàn toàn không ảnh hưởng đến tính đại diện của mẫu và an toàn hơn nhiều so với việc tự ý gán giá trị nhân tạo (imputation).
- **Tại sao loại bỏ `Price/Earnings <= 0` (11 dòng):**
  - *Bản chất công thức:* $P/E = \frac{\text{Giá cổ phiếu (Price)}}{\text{Lợi nhuận trên một cổ phiếu (EPS)}}$. Vì giá cổ phiếu trên thị trường luôn dương ($P > 0$), nên $P/E \le 0$ chỉ xảy ra khi $EPS \le 0$ (doanh nghiệp thua lỗ hoặc hòa vốn).
  - *Triệt tiêu ý nghĩa kinh tế:* Chỉ số P/E đo lường "nhà đầu tư sẵn sàng trả bao nhiêu đồng vốn cho 1 đồng lợi nhuận". Khi doanh nghiệp không có lợi nhuận (bị lỗ), câu hỏi này hoàn toàn vô nghĩa.
  - *Nghịch lý toán học khi so sánh:* Một công ty lỗ rất nặng ($EPS = -10\$, P = 50\$ \implies P/E = -5$) lại có giá trị số học lớn hơn một công ty chỉ lỗ nhẹ ($EPS = -1\$, P = 50\$ \implies P/E = -50$), do $-5 > -50$. Dấu âm làm đảo lộn hoàn toàn logic định giá.
  - *Làm méo mó tham số thống kê:* Dữ liệu có giá trị âm cực đoan (như $-251.53$), nếu đưa vào tính toán sẽ kéo lệch mạnh trung bình mẫu $\bar{X}$, làm sai lệch ước lượng mức định giá tổng thể của các doanh nghiệp đang tạo ra giá trị.
  - *Chuẩn mực tài chính quốc tế:* Các tổ chức tài chính lớn (Bloomberg, Morningstar, S&P Dow Jones) đều quy định công ty thua lỗ có $P/E$ là "N/A" và loại khỏi việc tính trung bình ngành.
- **Tại sao loại bỏ ngoại lai `Price/Earnings > 100` (12 dòng):**
  - P/E > 100 thường do lợi nhuận EPS nhất thời sụt giảm quá nhỏ về gần 0 (khiến mẫu số tí hon) hoặc do đầu cơ bong bóng đột biến, tạo thành các điểm ngoại lai cực đoan (outliers) kéo lệch đuôi phải nghiêm trọng, làm sai lệch kỳ vọng mẫu.
- **Tại sao bài toán 3c (Tỷ lệ trả cổ tức) lại tính trên toàn bộ 505 công ty gốc:**
  - *Tránh sai lệch chọn mẫu (Selection Bias):* Các công ty lỗ ($P/E \le 0$) hay công ty tăng trưởng nóng ($P/E > 100$) vẫn là các thực thể thực tế trên thị trường và có chính sách chi trả cổ tức rõ ràng (thường là không trả cổ tức). Nếu loại 25 công ty này ra trước khi tính tỷ lệ cổ tức, ta đã chủ động loại bỏ nhóm ít trả cổ tức nhất, khiến tỷ lệ mẫu $\hat{p}$ bị thổi phồng giả tạo (chệch lên cao).
  - *Biến nhị phân không có ngoại lai:* Biến $Pay\_Dividend$ là biến Bernoulli chỉ nhận giá trị $\{0, 1\}$. Biến nhị phân không có khái niệm "ngoại lai cực trị" làm méo phân phối như biến liên tục $P/E$. Cột `Dividend Yield` trong dữ liệu gốc có đầy đủ 505/505 dòng, hoàn toàn sạch.
  - *Nguyên tắc độc lập giữa các biến:* Lỗi hoặc đặc tính cực trị của biến $P/E$ không được làm mất đi các quan sát hợp lệ của biến $Pay\_Dividend$.

---

### Phần 2: Bài toán 3a — Khoảng tin cậy 95% cho kỳ vọng P/E (μ)

**Cell 2.1 (Markdown):** Lý thuyết & Thiết kế bài toán

Nội dung:

- **Mục tiêu:** Ước lượng kỳ vọng (trung bình tổng thể) μ của hệ số P/E cho toàn bộ các doanh nghiệp vốn hóa lớn tại Mỹ.
- **Biến làm việc:** `Price/Earnings` (sau khi đã lọc: 0 < P/E ≤ 100).
- **Cơ sở lý thuyết (chuỗi suy luận):**
  1. Biến ngẫu nhiên gốc X là giá trị P/E của một công ty lấy ngẫu nhiên từ tổng thể, có E(X) = μ, V(X) = σ² (chưa biết).
  2. Mẫu ngẫu nhiên W = (X₁, ..., Xₙ) gồm n công ty i.i.d.
  3. Trung bình mẫu X̄ = (1/n)ΣXᵢ là ước lượng không chệch của μ (vì E(X̄) = μ), với V(X̄) = σ²/n.
  4. Phương sai mẫu hiệu chỉnh S² = (1/(n-1))Σ(Xᵢ - X̄)² là ước lượng không chệch của σ² (vì E(S²) = σ²).
  5. **Phân phối mẫu của thống kê T** (Hệ quả 3, Chap 1 / Định lý 6, Chap 2): Khi tổng thể phân phối chuẩn N(μ, σ²) và σ² chưa biết:
     $$T = \frac{\bar{X} - \mu}{S / \sqrt{n}} \sim t(n-1)$$
  6. Khi tổng thể không chuẩn nhưng n ≥ 30, theo CLT (Định lý 2, Chap 1): X̄ xấp xỉ phân phối chuẩn → thống kê T xấp xỉ t(n-1), và KTC vẫn áp dụng được.

- **Công thức KTC 95% cho μ** (Định lý 6, Chap 2):

$$\bar{x} \pm t_{n-1,\, \alpha/2} \cdot \frac{s}{\sqrt{n}}$$

Trong đó:
  - $\bar{x} = \frac{1}{n} \sum x_i$ là trung bình mẫu (giá trị quan sát của ước lượng điểm $\bar{X}$).
  - $s = \sqrt{\frac{1}{n-1} \sum (x_i - \bar{x})^2}$ là **độ lệch chuẩn mẫu hiệu chỉnh** (giá trị quan sát của $S$).
  - $t_{n-1, \alpha/2}$ là phân vị Student bậc tự do $n-1$ thỏa $P(T > t_{\alpha/2}) = \alpha/2$ (tra bảng Student).
  - $\alpha = 0.05$ (độ tin cậy $1 - \alpha = 95\%$).

- **Làm rõ các khái niệm đo lường độ phân tán và sai số:**
  1. **Độ lệch chuẩn mẫu ($s$ - Standard Deviation):** Đo mức độ phân tán giữa **các quan sát cá thể $X_i$** trong tập dữ liệu so với giá trị trung bình mẫu $\bar{x}$ (cho biết các công ty chênh lệch mức P/E nhau nhiều hay ít).
  2. **Sai số chuẩn ($SE$ - Standard Error):**
     $$SE = \frac{s}{\sqrt{n}}$$
     - Bản chất: Là **độ lệch chuẩn của biến ngẫu nhiên Trung bình mẫu $\bar{X}$** (theo Định lý 1 Chap 1: $V(\bar{X}) = \frac{\sigma^2}{n} \implies \sigma_{\bar{X}} = \frac{\sigma}{\sqrt{n}} \approx \frac{s}{\sqrt{n}}$).
     - Ý nghĩa: Đo lường mức độ dao động của thống kê $\bar{X}$ xung quanh kỳ vọng thực $\mu$ nếu thực hiện lặp lại việc lấy mẫu nhiều lần. Do trung bình của $n$ quan sát luôn ổn định hơn từng quan sát đơn lẻ (các giá trị cực trị bù trừ cho nhau), $SE$ nhỏ hơn độ lệch chuẩn cá thể $s$ gấp $\sqrt{n}$ lần.
  3. **Biên sai số / Sai số biên ($ME$ hoặc $E$ - Margin of Error):**
     $$ME = E = (\text{Hệ số tới hạn}) \times (\text{Sai số chuẩn}) = t_{n-1,\, \alpha/2} \cdot \frac{s}{\sqrt{n}}$$
     - Bản chất hình học: Chính là **"bán kính"** của khoảng tin cậy đối xứng $[\bar{x} - ME \;;\; \bar{x} + ME]$.
     - Ý nghĩa thống kê: Thể hiện độ lệch tối đa kỳ vọng giữa ước lượng điểm từ mẫu ($\bar{x}$) so với tham số thực của tổng thể ($\mu$) ở độ tin cậy $1-\alpha = 95\%$.
     - Quan hệ với độ dài KTC: Độ dài khoảng tin cậy luôn bằng 2 lần biên sai số ($I = \text{Cận trên} - \text{Cận dưới} = 2ME$).

- **Điều kiện áp dụng:**
  1. Các quan sát độc lập với nhau ✓ (mỗi công ty là một thực thể kinh doanh riêng biệt)
  2. Chưa biết phương sai tổng thể σ² → dùng phân phối Student t(n-1) thay cho Z
  3. Cỡ mẫu lớn n ≥ 30 (sau lọc n ≈ 480) → theo CLT, X̄ xấp xỉ phân phối chuẩn bất kể phân phối gốc → **đảm bảo điều kiện áp dụng**
  4. Với n lớn, t_{n-1, α/2} ≈ z_{α/2} = 1.96 (phân phối t hội tụ về N(0,1) khi ν ≥ 30), nên có thể dùng z thay thế, nhưng ở đây dùng t cho chính xác hơn

- **Nhận xét dữ liệu:**
  - P/E ban đầu có min = -251.53, max = 520.15 → phải lọc giá trị âm và outliers trước
  - Sau khi lọc, dữ liệu vẫn lệch phải nhẹ nhưng không ảnh hưởng đến KTC cho kỳ vọng (nhờ CLT với n ≈ 480 >> 30)

**Cell 2.2 (Code):** Tính toán

Các bước tính toán cần triển khai (tự viết hàm, không dùng hàm KTC có sẵn):
1. Lấy mảng giá trị P/E đã lọc sạch → `pe_data`
2. Tính cỡ mẫu: `n = len(pe_data)`
3. Tính trung bình mẫu: `x_bar = sum(pe_data) / n` (hoặc `np.mean`, nhưng nên viết thủ công để thể hiện hiểu công thức)
4. Tính phương sai mẫu hiệu chỉnh: `s2 = sum((x - x_bar)**2 for x in pe_data) / (n - 1)`
5. Tính độ lệch chuẩn mẫu: `s = math.sqrt(s2)`
6. Tính sai số chuẩn: `SE = s / math.sqrt(n)`
7. Tra phân vị t: `t_crit = scipy.stats.t.ppf(1 - alpha/2, df=n-1)` — *giải thích: hàm `ppf` chỉ tra bảng giá trị phân vị, tương đương việc tra bảng phân phối t trong sách giáo khoa*
8. Tính biên sai số: `ME = t_crit * SE`
9. Khoảng tin cậy: `(x_bar - ME, x_bar + ME)`
10. In ra bảng tổng hợp: n, x̄, s, t_{n-1, 0.025}, ME, cận dưới, cận trên

**Cell 2.3 (Markdown):** Diễn giải kết quả

Nội dung mẫu (điền số thực tế sau khi chạy code):

> "Với độ tin cậy 95%, mức định giá P/E trung bình của các doanh nghiệp vốn hóa lớn tại Mỹ nằm trong khoảng [A ; B]. Điều này có nghĩa nhà đầu tư trên thị trường đang chấp nhận bỏ ra trung bình từ A đến B đô-la cho mỗi 1 đô-la lợi nhuận doanh nghiệp tạo ra."

---

### Phần 3: Bài toán 3b — Khoảng tin cậy 95% cho phương sai P/E (σ²)

**Cell 3.1 (Markdown):** Lý thuyết & Thiết kế bài toán

Nội dung:

- **Mục tiêu:** Ước lượng phương sai tổng thể σ² của hệ số P/E.
- **Biến làm việc:** `Price/Earnings` (cùng dữ liệu đã lọc sạch ở phần 2).
- **Công thức:**

$$\left[\frac{(n-1)s^2}{\chi^2_{n-1,\, \alpha/2}} \;;\; \frac{(n-1)s^2}{\chi^2_{n-1,\, 1 - \alpha/2}}\right]$$

- **Cơ sở lý thuyết:** Khi tổng thể X ~ N(μ, σ²), thống kê K² = (n-1)S²/σ² ~ χ²(n-1) (Định lý 7, Chap 1). Từ đó suy ra KTC cho σ² (Định lý 8, Chap 2).

Trong đó:
  - (n-1)s²/σ² ~ χ²(n-1) khi tổng thể phân phối chuẩn
  - **Quy ước phân vị trong slide:** χ²_α(ν) thỏa mãn P(X > χ²_α(ν)) = α (phân vị **bên phải**)
    - χ²_{α/2}(n-1): Phân vị bên phải mức α/2 → **giá trị lớn** → đặt ở mẫu số cận dưới
    - χ²_{1-α/2}(n-1): Phân vị bên phải mức 1-α/2 → **giá trị nhỏ** → đặt ở mẫu số cận trên
  - **Quy ước trong scipy:** `chi2.ppf(q, df)` trả về phân vị **bên trái** (P(X ≤ x) = q)
    - Đối chiếu: `chi2.ppf(1 - α/2, df)` tương ứng χ²_{α/2} trong slide (giá trị lớn)
    - Đối chiếu: `chi2.ppf(α/2, df)` tương ứng χ²_{1-α/2} trong slide (giá trị nhỏ)

- **Điều kiện áp dụng (QUAN TRỌNG):**
  1. Các quan sát độc lập ✓
  2. **Dữ liệu gốc phải tuân theo phân phối chuẩn** — đây là điều kiện bắt buộc, không thể thay thế bằng CLT (vì CLT chỉ áp dụng cho trung bình mẫu X̄, không áp dụng cho thống kê (n-1)S²/σ²)

- **Nhận xét & Xử lý dữ liệu:**
  - P/E thường bị lệch phải (right-skewed) → **cần kiểm tra tính chuẩn bằng trực quan hóa (Histogram & Q-Q plot)**.
  - **Giải thích các công cụ trực quan hóa kiểm tra tính chuẩn:**
    - **Histogram (Biểu đồ tần số):** Chia dải giá trị thành các khoảng liên tiếp (bins) và đếm tần số xuất hiện. Khi vẽ thêm đường cong mật độ chuẩn lý thuyết chồng lên, ta quan sát được phân phối thực tế có dạng hình chuông đối xứng hay bị lệch đuôi (skewed).
    - **Q-Q Plot (Quantile-Quantile Plot):** Đồ thị so sánh trực tiếp các phân vị thực nghiệm của mẫu dữ liệu với các phân vị lý thuyết của phân phối chuẩn tắc $\mathcal{N}(0, 1)$. Nếu các điểm dữ liệu bám sát đường thẳng tham chiếu 45° thì dữ liệu tuân theo phân phối chuẩn; nếu các điểm uốn cong hoặc lệch xa đường thẳng ở hai đầu thì dữ liệu bị lệch đuôi hoặc có đuôi nặng.
  - **Tại sao chọn phép biến đổi $\ln(P/E)$?**
    - Biến P/E có chặn dưới tại 0 ($P/E > 0$) và có đuôi kéo dài sang phải $\implies$ đây là đặc trưng điển hình của **phân phối Log-chuẩn (Lognormal Distribution)**.
    - Phép biến đổi logarit $Y = \ln(P/E)$ giúp nén mạnh các giá trị cực lớn ở đuôi phải và kéo giãn vùng giá trị nhỏ gần 0, đưa phân phối của $Y$ về dạng gần đối xứng và tiệm cận phân phối Chuẩn $\mathcal{N}(\mu_Y, \sigma_Y^2)$.
  - **Tại sao không chọn cách khác (ví dụ: khớp phân phối tương ứng như Gamma, Weibull,...)?**
    1. *Giới hạn trong phạm vi môn học:* Giáo trình và slide (MI2020, MI3031) chỉ cung cấp công thức giải tích đóng (closed-form formula) cho khoảng tin cậy của phương sai dưới giả thiết phân phối Chuẩn (dựa trên thống kê $\frac{(n-1)S^2}{\sigma^2} \sim \chi^2(n-1)$) và xấp xỉ chuẩn theo CLT. Giáo trình không có công thức giải tích đóng cho KTC phương sai của phân phối Gamma hay Weibull.
    2. *Độ phức tạp vượt ngoài yêu cầu:* Để dựng KTC cho phương sai khi khớp phân phối Gamma hay Weibull, người làm phải áp dụng các phương pháp phi chuẩn cấp cao (như Bootstrap tái lấy mẫu phi tham số, hoặc ước lượng tiệm cận dựa trên ma trận thông tin Fisher / hàm hợp lý Profile Likelihood). Điều này vượt xa mục tiêu yêu cầu của đề tài và kiến thức nền tảng của môn học.
    3. *Chuẩn mực phân tích tài chính:* Phép biến đổi logarit là chuẩn mực kinh điển trong kinh tế lượng tài chính (log-returns, log-valuation) vì bảo toàn trật tự dữ liệu ($a < b \iff \ln a < \ln b$) và dễ diễn giải.
  - Nếu dùng ln(P/E): KTC thu được là cho $\text{Var}(\ln(P/E))$. Cần nêu rõ giới hạn: kết quả phương sai thu được là của biến đã biến đổi, không phải phương sai gốc của P/E.

**Cell 3.2 (Code):** Kiểm tra tính chuẩn & Trực quan hóa

Tạo figure 2×2 gồm:
1. **Histogram của P/E** (sau lọc) + đường cong mật độ chuẩn lý thuyết chồng lên → nhận xét hình dạng phân phối lệch phải
2. **Q-Q plot của P/E** → quan sát các điểm lệch khỏi đường thẳng 45° ở đuôi phải
3. **Histogram của ln(P/E)** + đường cong mật độ chuẩn → nhận xét mức độ cải thiện hình chuông
4. **Q-Q plot của ln(P/E)** → so sánh với Q-Q plot gốc, xác nhận các điểm bám sát đường thẳng hơn rõ rệt

Nhận xét: Dữ liệu gốc P/E lệch phải, sau khi lấy ln thì gần chuẩn hơn → tính toán song song cả 2 trường hợp (P/E gốc và ln(P/E)) và nêu rõ hạn chế lý thuyết.

**Cell 3.3 (Code):** Tính toán KTC cho phương sai

Các bước (tự viết):
1. Chọn biến: dùng P/E gốc (và thêm phần tính song song cho ln(P/E) để so sánh)
2. Tính n, s² (phương sai mẫu hiệu chỉnh, đã tính ở phần 2)
3. Tra phân vị chi-bình phương:
   - `chi2_upper = scipy.stats.chi2.ppf(1 - alpha/2, df=n-1)` — phân vị bên phải, giá trị lớn
   - `chi2_lower = scipy.stats.chi2.ppf(alpha/2, df=n-1)` — phân vị bên trái, giá trị nhỏ
   - *Giải thích: `ppf(q, df)` trả về giá trị x sao cho P(χ² ≤ x) = q, tương đương tra bảng*
4. Cận dưới: `lower = (n - 1) * s2 / chi2_upper`
5. Cận trên: `upper = (n - 1) * s2 / chi2_lower`
6. Tính thêm khoảng cho độ lệch chuẩn: `(sqrt(lower), sqrt(upper))`
7. In bảng kết quả: n, s², χ² hai phân vị, cận dưới, cận trên (cho cả σ² và σ)

**Cell 3.4 (Markdown):** Diễn giải kết quả

Nội dung mẫu:

> "Với độ tin cậy 95%, phương sai của hệ số P/E nằm trong khoảng [C ; D] (tương đương độ lệch chuẩn từ √C đến √D). Khoảng này phản ánh mức độ phân hóa định giá giữa các công ty trên thị trường."

> **Hạn chế:** Công thức KTC cho phương sai dựa trên giả thiết tổng thể phân phối chuẩn. Qua Histogram và Q-Q plot, dữ liệu P/E có dấu hiệu lệch phải → kết quả cần được diễn giải một cách thận trọng. Khi dùng biến đổi ln(P/E), phân phối gần chuẩn hơn nên KTC cho Var(ln(P/E)) có độ tin cậy cao hơn.

---

### Phần 4: Bài toán 3c — Khoảng tin cậy 95% cho tỷ lệ trả cổ tức (p)

**Cell 4.1 (Markdown):** Lý thuyết & Thiết kế bài toán

Nội dung:

- **Mục tiêu:** Ước lượng tỷ lệ p — xác suất một công ty vốn hóa lớn tại Mỹ có chính sách trả cổ tức.
- **Biến làm việc:** `Pay_Dividend` (biến nhị phân, tạo từ `Dividend Yield`)
  - `Pay_Dividend = 1` nếu `Dividend Yield > 0` (có trả cổ tức)
  - `Pay_Dividend = 0` nếu `Dividend Yield = 0` (không trả cổ tức)
- **Dữ liệu sử dụng:** Toàn bộ 505 công ty ban đầu (không lọc theo P/E vì biến này không liên quan đến P/E)
- **Công thức:**

$$\hat{p} \pm z_{\alpha/2} \sqrt{\frac{\hat{p}(1-\hat{p})}{n}}$$

Trong đó:
  - p̂ = k / n là tỷ lệ mẫu (k = số công ty có trả cổ tức)
  - z_{α/2} = z_{0.025} = 1.96 là phân vị chuẩn tắc
  - n = tổng số công ty trong mẫu

- **Cơ sở lý thuyết:**
  - Biến ngẫu nhiên gốc X ~ Bernoulli(p), với E(X) = p, V(X) = p(1-p)
  - Tần suất mẫu P̂ = (1/n)ΣXᵢ là ước lượng không chệch của p (E(P̂) = p), V(P̂) = p(1-p)/n
  - Khi n đủ lớn, theo xấp xỉ chuẩn cho phân phối nhị thức (Chap 1): P̂ ≈ N(p, p(1-p)/n)
  - Thống kê Z = (P̂ - p) / √(p(1-p)/n) → N(0,1) (Định lý 10, Chap 2)
  - Thay p bằng p̂ trong sai số chuẩn (vì p chưa biết) → thu được công thức KTC

- **Điều kiện áp dụng:**
  1. Các quan sát độc lập ✓ (mỗi công ty quyết định chính sách cổ tức riêng)
  2. Điều kiện xấp xỉ chuẩn theo slide (Chap 1 & Định lý 10, Chap 2): **np̂ ≥ 5 và n(1-p̂) ≥ 5**
     - Dữ liệu thực tế: n = 505, k = 419 → p̂ ≈ 0.83
     - Kiểm tra: np̂ = 505 × 0.83 = 419 >> 5 ✓
     - Kiểm tra: n(1-p̂) = 505 × 0.17 = 86 >> 5 ✓
     - → **Điều kiện xấp xỉ chuẩn thỏa mãn tuyệt đối**

**Cell 4.2 (Code):** Tính toán

Các bước (tự viết):
1. Tính trên bộ dữ liệu gốc (505 dòng, trước khi lọc P/E)
2. Tính n = tổng số quan sát = 505
3. Tính k = số công ty có `Dividend Yield > 0` = 419
4. Tính tỷ lệ mẫu: `p_hat = k / n`
5. Kiểm tra điều kiện: in ra `n * p_hat` và `n * (1 - p_hat)`, xác nhận cả hai ≥ 5
6. Tra phân vị: `z_crit = scipy.stats.norm.ppf(1 - alpha/2)` → 1.96 (*giải thích: tra bảng Z chuẩn tắc*)
7. Tính sai số chuẩn: `SE = math.sqrt(p_hat * (1 - p_hat) / n)`
8. Tính biên sai số: `ME = z_crit * SE`
9. Khoảng tin cậy: `(p_hat - ME, p_hat + ME)`
10. In bảng: n, k, p̂, z_{0.025}, SE, ME, cận dưới, cận trên (dạng tỷ lệ và %)

Thêm trực quan hóa: Vẽ biểu đồ cột (bar chart) hoặc pie chart thể hiện tỷ lệ Có/Không trả cổ tức.

**Cell 4.3 (Markdown):** Diễn giải kết quả

Nội dung mẫu:

> "Với độ tin cậy 95%, tỷ lệ các công ty vốn hóa lớn tại Mỹ có chi trả cổ tức nằm trong khoảng từ E% đến F%. Vì toàn bộ khoảng tin cậy đều lớn hơn 50%, có thể khẳng định phần lớn các doanh nghiệp lớn, trưởng thành tại Mỹ đều duy trì chính sách chia cổ tức tiền mặt cho cổ đông."

---

### Phần 5: Bài toán 3d — Khoảng tin cậy 95% cho hiệu kỳ vọng P/E giữa hai ngành (μ₁ - μ₂)

**Cell 5.1 (Markdown):** Lý thuyết & Thiết kế bài toán

Nội dung:

- **Mục tiêu:** So sánh mức định giá P/E trung bình giữa ngành **Information Technology** (μ₁) và **Health Care** (μ₂) bằng KTC cho hiệu μ₁ - μ₂.
- **Biến làm việc:**
  - `Price/Earnings` (sau lọc 0 < P/E ≤ 100)
  - `Sector` — dùng để tách 2 nhóm:
    - Nhóm 1: `Sector == 'Information Technology'` → n₁ = 66 công ty
    - Nhóm 2: `Sector == 'Health Care'` → n₂ = 58 công ty
- **Cơ sở lý thuyết:**
  - Hai mẫu ngẫu nhiên **độc lập** kích thước n₁, n₂ từ hai tổng thể có kỳ vọng μ₁, μ₂ và phương sai σ₁², σ₂² (chưa biết, không giả sử bằng nhau)
  - Phân phối mẫu của hiệu trung bình (Định lý 3, Chap 1): E(X̄₁ - X̄₂) = μ₁ - μ₂, V(X̄₁ - X̄₂) = σ₁²/n₁ + σ₂²/n₂
  - **Công thức chính thống theo slide (Định lý 13, Chap 2):** Khi σ₁² ≠ σ₂² chưa biết, dùng **xấp xỉ Welch** với phân phối t(ν):
    $$(\bar{x}_1 - \bar{x}_2) \pm t_{\alpha/2,\, \nu} \sqrt{\frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}}$$
    trong đó bậc tự do Satterthwaite:
    $$\nu = \frac{\left(\frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}\right)^2}{\frac{(s_1^2/n_1)^2}{n_1-1} + \frac{(s_2^2/n_2)^2}{n_2-1}} \quad \text{(làm tròn xuống)}$$

- **Công thức áp dụng trong bài (trường hợp đặc biệt mẫu lớn):**

$$(\bar{x}_1 - \bar{x}_2) \pm z_{\alpha/2} \sqrt{\frac{s_1^2}{n_1} + \frac{s_2^2}{n_2}}$$

Trong đó:
  - x̄₁, x̄₂ là trung bình mẫu P/E của mỗi nhóm
  - s₁², s₂² là phương sai mẫu hiệu chỉnh của mỗi nhóm
  - z_{α/2} = 1.96

- **Tại sao dùng xấp xỉ chuẩn Z thay vì t Welch (Định lý 13)?**
  - Hai mẫu **độc lập** (các công ty thuộc 2 ngành khác nhau hoàn toàn) ✓
  - Chưa biết σ₁², σ₂² và **không giả sử chúng bằng nhau** → đúng bối cảnh Định lý 13
  - Cỡ mẫu mỗi nhóm đều lớn: n₁ = 66 ≥ 30, n₂ = 58 ≥ 30
  - Theo CLT (Định lý 3, Chap 1), khi n₁, n₂ đủ lớn, thống kê chuẩn hóa hội tụ về N(0,1), đồng thời t(ν) → N(0,1) khi ν lớn
  - → Dùng z_{α/2} là **trường hợp đặc biệt** của Định lý 13 khi mẫu lớn, cho kết quả gần như tương đương nhưng **đơn giản hơn** (tránh phải tính bậc tự do Satterthwaite)

- **Nhận xét dữ liệu:**
  - Ngành IT thường có P/E cao hơn do kỳ vọng tăng trưởng lớn
  - Ngành Health Care đa dạng hơn (gồm cả dược phẩm truyền thống P/E thấp lẫn biotech P/E rất cao)

**Cell 5.2 (Code):** Trực quan hóa so sánh 2 nhóm

- **Trực quan hóa bằng Histogram so sánh:**
  - Vẽ biểu đồ tần số (Histogram) phân phối P/E của cả hai ngành trên cùng một đồ thị với màu sắc phân biệt và độ mờ (`alpha=0.6`), hoặc đặt 2 biểu đồ con (subplots) cạnh nhau với cùng thang đo trục hoành.
  - Giúp đối chiếu trực quan vị trí tập trung dữ liệu, độ phân tán và hình dạng phân bố P/E giữa ngành Information Technology và Health Care.
- Triển khai code:
  - Vẽ biểu đồ **Histogram so sánh** phân phối P/E giữa IT và Health Care
  - In thống kê mô tả ($n, \bar{x}, s$) của mỗi nhóm

**Cell 5.3 (Code):** Tính toán KTC

Các bước (tự viết):
1. Tách dữ liệu P/E theo Sector:
   - `pe_tech = df[df['Sector'] == 'Information Technology']['Price/Earnings']`
   - `pe_hc = df[df['Sector'] == 'Health Care']['Price/Earnings']`
2. Tính cho nhóm 1 (Tech):
   - `n1 = len(pe_tech)`
   - `x1_bar = sum(pe_tech) / n1`
   - `s1_sq = sum((x - x1_bar)**2 for x in pe_tech) / (n1 - 1)`
3. Tính cho nhóm 2 (HC):
   - `n2 = len(pe_hc)`
   - `x2_bar = sum(pe_hc) / n2`
   - `s2_sq = sum((x - x2_bar)**2 for x in pe_hc) / (n2 - 1)`
4. Tính hiệu trung bình: `diff = x1_bar - x2_bar`
5. Tính sai số chuẩn: `SE = math.sqrt(s1_sq/n1 + s2_sq/n2)`
6. Tra phân vị: `z_crit = 1.96`
7. Biên sai số: `ME = z_crit * SE`
8. KTC: `(diff - ME, diff + ME)`
9. In bảng: n₁, n₂, x̄₁, x̄₂, s₁², s₂², x̄₁ - x̄₂, SE, ME, cận dưới, cận trên

**Cell 5.4 (Markdown):** Diễn giải kết quả

Hai trường hợp cần xem xét:

- **Nếu cả hai cận đều dương** ([G, H] với G > 0):

> "Với độ tin cậy 95%, hệ số P/E trung bình của ngành Công nghệ cao hơn ngành Chăm sóc sức khỏe từ G đến H đơn vị. Điều này hợp lý về mặt kinh tế vì ngành Công nghệ có kỳ vọng tăng trưởng tương lai cao hơn nên được thị trường định giá đắt hơn."

- **Nếu khoảng chứa số 0** (G < 0 < H):

> "Ở độ tin cậy 95% (mức ý nghĩa α = 5%), chưa đủ bằng chứng thống kê để kết luận có sự khác biệt về mức định giá P/E trung bình giữa ngành Công nghệ và ngành Chăm sóc sức khỏe."

---

### Phần 6: Tổng kết

**Cell 6.1 (Markdown):** Bảng tổng hợp kết quả

| Bài toán | Tham số | Phương pháp | KTC 95% | Diễn giải ngắn |
|---|---|---|---|---|
| 3a | μ (P/E) | t-interval | [?, ?] | ... |
| 3b | σ² (P/E) | χ²-interval | [?, ?] | ... |
| 3c | p (tỷ lệ cổ tức) | Z-interval cho tỷ lệ | [?, ?] | ... |
| 3d | μ₁ - μ₂ (IT vs HC) | Z-interval cho hiệu kỳ vọng | [?, ?] | ... |

*(Giá trị cụ thể sẽ được điền sau khi chạy code)*

---

## Nguyên tắc viết code xuyên suốt

1. **Ưu tiên tự viết hàm tính toán:**
   - Tính trung bình, phương sai, độ lệch chuẩn, tỷ lệ mẫu **bằng công thức thủ công** (vòng lặp `sum()` hoặc list comprehension)
   - Có thể dùng `np.mean()`, `np.var(ddof=1)` để **kiểm tra chéo** kết quả, nhưng kết quả chính lấy từ hàm tự viết

2. **Chỉ dùng hàm có sẵn để tra bảng phân vị:**
   - `scipy.stats.norm.ppf()` — tra bảng Z
   - `scipy.stats.t.ppf()` — tra bảng t
   - `scipy.stats.chi2.ppf()` — tra bảng χ²
   - Mỗi lần dùng cần **chú thích** giải thích hàm ppf là gì (Percent Point Function = hàm ngược CDF, tương đương tra bảng trong sách giáo khoa)

3. **Không dùng** các hàm đóng gói sẵn như:
   - `scipy.stats.t.interval()`
   - `statsmodels.stats.proportion_confint()`
   - Hoặc bất kỳ hàm nào trả ra KTC trực tiếp mà không cần tự tính

4. **Trực quan hóa đi kèm mỗi bài toán** (histogram, Q-Q plot, bar chart) để hỗ trợ nhận xét trực quan.
