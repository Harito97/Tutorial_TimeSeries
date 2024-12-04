# Compare

ARIMA(2, 1, 2) - AIC-33504.754

ARIMA(2, 1, 2)x(2, 1, 2, 7) - AIC: -33859.15146003641

---

# ARIMA(2, 1, 2)

SARIMAX Results
==============================================================================
Dep. Variable:
Adj Close
Model:ARIMA(2, 1, 2)
Date:
No. Observations:
23686
Log Likelihood16757.377
Tue, 19 Nov 2024AIC-33504.754
Time:19:09:22BIC-33464.391
Sample:01-04-1960HQIC-33491.656
- 11-08-2024
Covariance Type:
opg
==============================================================================
coef
std err
z
P>|z|
[0.025
0.975]
------------------------------------------------------------------------------
ar.L1-1.01060.546-1.8510.064-2.0800.059
ar.L2-0.18800.188-1.0020.316-0.5560.180
ma.L11.09820.5462.0110.0440.0282.168
ma.L20.25180.2341.0750.282-0.2070.711
sigma20.00571.73e-05326.4510.0000.0060.006
==============================================================================
Ljung-Box (L1) (Q):54.07Jarque-Bera (JB):
1908074.63
Prob(Q):0.00Prob(JB):0.00
Heteroskedasticity (H):0.15Skew:-0.56
Prob(H) (two-sided):0.00Kurtosis:46.96
==============================================================================

---

Dựa trên kết quả ước lượng của mô hình ARIMA(2, 1, 2), chúng ta có thể phân tích mô hình và thực hiện các kiểm định chẩn đoán.

---

### **Phương trình mô hình ARIMA(2, 1, 2)**

Mô hình ARIMA(2, 1, 2) được viết dưới dạng:

$$
\Delta Y_t = \phi_1 \Delta Y_{t-1} + \phi_2 \Delta Y_{t-2} + \epsilon_t + \theta_1 \epsilon_{t-1} + \theta_2 \epsilon_{t-2}
$$

Ở đây:
- $\Delta Y_t = Y_t - Y_{t-1}$: Là sai phân bậc 1 của chuỗi $Y_t$.
- $\phi_1, \phi_2$: Hệ số của phần tự hồi quy (AR) bậc 1 và 2.
- $\theta_1, \theta_2$: Hệ số của phần trung bình trượt (MA) bậc 1 và 2.
- $\epsilon_t$: Nhiễu trắng (white noise) với phương sai $\sigma^2$.

Thay các hệ số từ kết quả ước lượng, phương trình trở thành:

$$
\Delta Y_t = -1.0106 \Delta Y_{t-1} - 0.1880 \Delta Y_{t-2} + 1.0982 \epsilon_{t-1} + 0.2518 \epsilon_{t-2} + \epsilon_t
$$

---

### **Chẩn đoán kiểm định**

#### 1. **Kiểm tra ý nghĩa thống kê của các hệ số**
- **Hệ số AR và MA**:
  - $\phi_1 = -1.0106$, p-value = 0.064 (không ý nghĩa thống kê ở mức 5%).
  - $\phi_2 = -0.1880$, p-value = 0.316 (không ý nghĩa thống kê).
  - $\theta_1 = 1.0982$, p-value = 0.044 (ý nghĩa thống kê ở mức 5%).
  - $\theta_2 = 0.2518$, p-value = 0.282 (không ý nghĩa thống kê).

  => Chỉ $ \theta_1 $ là ý nghĩa thống kê, cần xem xét lại cấu trúc mô hình.

- **Phương sai nhiễu trắng $\sigma^2 = 0.0057$**: Ý nghĩa thống kê với p-value = 0.000.

#### 2. **Kiểm định phần dư**

##### **a. Kiểm tra tự tương quan phần dư (Ljung-Box Test)**
- $Q$-statistic = 54.07, p-value = 0.00.
  - Kết luận: Có tự tương quan còn sót lại trong phần dư => Mô hình chưa phù hợp hoàn toàn.

##### **b. Kiểm tra phân phối phần dư (Jarque-Bera Test)**
- JB = 1908074.63, p-value = 0.00.
  - Kết luận: Phần dư không tuân theo phân phối chuẩn. Đây là một vấn đề cần được khắc phục, đặc biệt nếu phần dư có ảnh hưởng lớn đến dự báo.

##### **c. Kiểm tra phương sai phần dư (Heteroskedasticity Test)**
- $H = 0.15$, p-value = 0.00.
  - Kết luận: Phần dư không đồng nhất phương sai (heteroskedasticity). Điều này cho thấy có thể cần điều chỉnh mô hình hoặc sử dụng các phương pháp khác như GARCH.

#### 3. **Kiểm tra tính ổn định của mô hình**
- Các hệ số AR ($\phi_1, \phi_2$) cần đảm bảo rơi vào miền nghịch đảo để mô hình ổn định. Do $\phi_1 = -1.0106$, gần biên ổn định (-1 và 1), điều này có thể dẫn đến tính bất ổn định.

---

### **Kết luận và gợi ý cải thiện**
1. **Kết luận**:
   - Mô hình ARIMA(2, 1, 2) có một số hạn chế, bao gồm tự tương quan phần dư và phương sai không đồng nhất.
   - Một số hệ số AR và MA không ý nghĩa thống kê.

2. **Gợi ý cải thiện**:
   - Kiểm tra lại bậc của ARIMA bằng cách sử dụng ACF/PACF hoặc các tiêu chí chọn mô hình khác như AIC, BIC.
   - Xem xét chuyển đổi logarit hoặc Box-Cox để xử lý phương sai không đồng nhất.
   - Sử dụng mô hình SARIMA hoặc các phương pháp bổ sung như ARIMA-GARCH nếu có sự thay đổi trong phương sai.
   - Sử dụng phần dư của mô hình hiện tại để kiểm định và thử nghiệm các bậc khác của AR và MA (ví dụ: ARIMA(1,1,1) hoặc ARIMA(3,1,3)) để cải thiện sự phù hợp.

---

# SARIMA(2, 1, 2)(2, 1, 2, 7)

ARIMA(2, 1, 2)x(2, 1, 2, 7)12 - AIC: -33859.15146003641

==============================================================================
Dep. Variable:
Adj Close
Model:SARIMAX(2, 1, 2)x(2, 1, 2, 7)
Date:
No. Observations:
23686
Log Likelihood16938.576
Tue, 19 Nov 2024AIC-33859.151
Time:19:19:44BIC-33786.507
Sample:01-04-1960HQIC-33835.577
- 11-08-2024
Covariance Type:
opg
==============================================================================
coef
std err
z
P>|z|
[0.025
0.975]
------------------------------------------------------------------------------
ar.L10.44220.6620.6670.504-0.8561.741
ar.L2-0.04180.120-0.3490.727-0.2760.193

---

### **Phương trình của mô hình SARIMA(2, 1, 2)(2, 1, 2, 7):**

Mô hình SARIMA bao gồm thành phần hồi quy tự hồi quy (AR), trung bình trượt (MA), và các thành phần theo mùa với chu kỳ 7 (weekly).

Phương trình dạng sai phân:

$$
\Phi(B^7)(1 - B)(1 - B^7)Y_t = \Theta(B^7)(1 - \phi_1 B - \phi_2 B^2)\epsilon_t + (1 + \theta_1 B + \theta_2 B^2)
$$

Hệ số từ kết quả:

$$
\text{AR terms: } \phi_1 = 0.4422, \, \phi_2 = -0.0418 \quad (\text{Không ý nghĩa thống kê, } p > 0.05)
$$

---

### **Kết luận kiểm định:**

1. **Kiểm tra ý nghĩa của hệ số:**
   - Các hệ số $ \phi_1, \phi_2 $ không có ý nghĩa thống kê ($p > 0.05$), cần xem xét lại cấu trúc mô hình.

2. **Kiểm tra AIC và BIC:**
   - AIC = -33859.151, BIC = -33786.507. AIC thấp cho thấy mô hình phù hợp tương đối tốt, nhưng có thể cải thiện thêm bằng cách tối ưu hóa bậc.

3. **Kiểm định phần dư:**
   - Phần dư cần kiểm tra thêm (Ljung-Box và Jarque-Bera) để đảm bảo không còn tự tương quan hoặc phương sai thay đổi.

---

### **Đề xuất:**
- Xem xét lại việc giảm bậc của mô hình (ví dụ: SARIMA(1, 1, 1)(1, 1, 1, 7)).
- Sử dụng kiểm định ACF/PACF để phân tích tự tương quan phần dư.
- Nếu phần dư không đạt chuẩn, cân nhắc sử dụng mô hình bổ sung như SARIMA-GARCH.
