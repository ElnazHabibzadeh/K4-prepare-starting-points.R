r
## (K-4) Prepare starting points, search bounds and penalty value
## for MLE optimization

## K-4.1 theta - نقطه شروع برای θ
# اگر startTheta مشخص نشده باشد، از فرمول تجربی استفاده می‌شود
x1 <- rep(n / (100 * k), k)  # θ0 = n / (100k) برای هر بعد

## K-4.2 p - تنظیم حدود و نقطه شروع برای توان p
# گسترش حدود برای p (اگر optimizeP = TRUE)
LowerTheta <- c(LowerTheta, rep(1, k) * 0.01)   # حد پایین p: 0.01
UpperTheta <- c(UpperTheta, rep(1, k) * 2)      # حد بالای p: 2
x3 <- rep(1, k) * 1.9                           # نقطه شروع p0 = 1.9

# ترکیب θ و p به عنوان بردار اولیه
x0 <- c(x1, x3)

## K-4.3 lambda (nugget) - نقطه شروع و حدود برای λ
# نقطه شروع: میانگین حدود لگاریتمی
x2 <- (fit$lambdaUpper + fit$lambdaLower) / 2
x0 <- c(x0, x2)  # اضافه کردن λ به بردار اولیه

# گسترش حدود برای λ
LowerTheta <- c(LowerTheta, fit$lambdaLower)
UpperTheta <- c(UpperTheta, fit$lambdaUpper)

# تبدیل x0 به ماتریس تک‌سطری برای استفاده در optimizer
x0 <- matrix(x0, nrow = 1)

# تنظیم بودجه بهینه‌سازی (تعداد ارزیابی‌ها)
opts <- list(funEvals = fit$budgetAlgTheta * ncol(x0))

## K-4.4 Penalty value - مقدار جریمه برای جلوگیری از پارامترهای نامعتبر
penval <- n * log(var(y)) + 1e4
