---
layout: project
title: whiteboard picture enhancer
date: 2025-08-02
description: این پروژه یک ابزار پردازش تصویر است که عکس‌های تخته‌سفید را تمیز و واضح می‌کند. با استفاده از تکنیک‌هایی مثل بهبود کنتراست، حذف پس‌زمینه، آستانه‌گذاری تطبیقی و حذف نویز، نوشته‌ها در تصویر برجسته و پس‌زمینه سفید می‌شود تا خوانایی تصویر بسیار بهتر شود. این پروژه برای آرشیو، اشتراک‌گذاری یا آماده‌سازی عکس‌های تخته‌سفید برای OCR بسیار مناسب است.
tags:
  - CV
  - python
  - whiteboard
  - تمیز_کردن_تصویر
github: SepehrDadgar/whiteboard-enhancer
live_demo: whiteboard-enhancer
---


# چجوری عکس تخته‌سفید رو تمیز کنیم با Python و OpenCV

یه کد پایتون میخوایم بنویسیم که عکس تخته‌سفید رو می‌گیره و نویز و لکه‌هاشو پاک می‌کنه، نوشته‌ها رو واضح‌تر می‌کنه، و پس‌زمینه رو سفید می‌کنه. اینطوری می‌تونی عکسای تخته‌سفید رو راحت‌تر بخونی یا برای OCR استفاده کنی.

---

## خلاصه کار کد

کد این کارا رو می‌کنه:

عکس رو باز می‌کنه و اگه خواستی بزرگش می‌کنه  

رنگ‌هاشو خاکستری می‌کنه (سیاه و سفید)  

کنتراست محلی رو بهتر می‌کنه با CLAHE  

پس‌زمینه رو صاف و یکنواخت می‌کنه  

متن رو با آستانه‌گذاری تطبیقی جدا می‌کنه  

نویز و لکه‌های ریز رو پاک می‌کنه  

قسمت‌های خیلی کوچیک رو حذف می‌کنه  

رنگ تصویر رو معکوس می‌کنه که پس‌زمینه سفید و متن سیاه بشه  

آخرش عکس تمیز رو ذخیره و نشون می‌ده

---

## کل کد تابع

```python
import cv2
import numpy as np
from matplotlib import pyplot as plt

def preprocess_whiteboard_image(image_path, save_path='enhanced_whiteboard.png', display=True, upscale=True):
    # Load image from file
    image = cv2.imread(image_path)
    if image is None:
        raise ValueError(f"Cannot load image: {image_path}")

    # Optional: upscale image to keep thin details clear
    if upscale:
        image = cv2.resize(image, None, fx=2.0, fy=2.0, interpolation=cv2.INTER_CUBIC)

    # Convert to grayscale
    gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

    # Apply CLAHE for local contrast enhancement
    clahe = cv2.createCLAHE(clipLimit=3.0, tileGridSize=(8, 8))
    enhanced = clahe.apply(gray)

    # Remove background with morphological closing
    kernel_bg = cv2.getStructuringElement(cv2.MORPH_ELLIPSE, (25, 25))
    background = cv2.morphologyEx(enhanced, cv2.MORPH_CLOSE, kernel_bg)
    contrast = cv2.divide(enhanced, background, scale=255)

    # Smooth lighting with Gaussian Blur
    blurred = cv2.GaussianBlur(contrast, (3, 3), 0)

    # Adaptive thresholding (inverse binary)
    binary = cv2.adaptiveThreshold(
        blurred, 255,
        cv2.ADAPTIVE_THRESH_GAUSSIAN_C,
        cv2.THRESH_BINARY_INV,
        blockSize=11,
        C=5
    )

    # Median filter to reduce salt-and-pepper noise
    denoised = cv2.medianBlur(binary, 3)

    # Morphological opening to remove small isolated pixels
    kernel = cv2.getStructuringElement(cv2.MORPH_RECT, (2, 2))
    denoised = cv2.morphologyEx(denoised, cv2.MORPH_OPEN, kernel)

    # Remove small connected components
    num_labels, labels, stats, _ = cv2.connectedComponentsWithStats(denoised, connectivity=8)
    min_area = 30
    cleaned = np.zeros_like(denoised)
    for i in range(1, num_labels):
        if stats[i, cv2.CC_STAT_AREA] >= min_area:
            cleaned[labels == i] = 255

    # Invert image to have white background and black text
    cleaned_inverted = cv2.bitwise_not(cleaned)

    # Save the final image
    cv2.imwrite(save_path, cleaned_inverted)

    # Display images for checking
    if display:
        plt.figure(figsize=(15, 5))
        plt.subplot(1, 3, 1), plt.imshow(gray, cmap='gray'), plt.title("Original Grayscale"), plt.axis('off')
        plt.subplot(1, 3, 2), plt.imshow(binary, cmap='gray'), plt.title("Adaptive Threshold"), plt.axis('off')
        plt.subplot(1, 3, 3), plt.imshow(cleaned_inverted, cmap='gray'), plt.title("Cleaned Output"), plt.axis('off')
        plt.tight_layout()
        plt.show()

    return cleaned_inverted
````

---

## هر قسمت کد چه کاری می‌کنه؟

### ۱. باز کردن عکس و بزرگ کردن اگه خواستی

```python
image = cv2.imread(image_path)
if upscale:
    image = cv2.resize(image, None, fx=2.0, fy=2.0, interpolation=cv2.INTER_CUBIC)
```

اینجا عکس رو از فایل می‌خونه.  
اگه تصویر کیفیتش پایین باشه، بزرگش می‌کنیم تا جزئیات متن ظریف‌تر بمونه.

---

### ۲. تبدیل عکس به سیاه‌وسفید و بهتر کردن کنتراست

```python
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)
clahe = cv2.createCLAHE(clipLimit=3.0, tileGridSize=(8, 8))
enhanced = clahe.apply(gray)
```

اول رنگ عکس رو خاکستری می‌کنیم.  
بعد CLAHE باعث میشه قسمتای تاریک و روشن عکس بهتر دیده بشن.

---

### ۳. پاک کردن پس‌زمینه با یه نوع فیلتر مورفولوژی

```python
kernel_bg = cv2.getStructuringElement(cv2.MORPH_ELLIPSE, (25, 25))
background = cv2.morphologyEx(enhanced, cv2.MORPH_CLOSE, kernel_bg)
contrast = cv2.divide(enhanced, background, scale=255)
```

اینجا پس‌زمینه رو صاف می‌کنیم، انگار لکه‌ها رو پاک می‌کنیم.  
بعد با تقسیم کردن تصویر روی پس‌زمینه، روشنایی عکس یکنواخت میشه.

---

### ۴. تبدیل عکس به سیاه‌وسفید دقیق با آستانه تطبیقی

```python
binary = cv2.adaptiveThreshold(
    blurred, 255,
    cv2.ADAPTIVE_THRESH_GAUSSIAN_C,
    cv2.THRESH_BINARY_INV,
    blockSize=11,
    C=5
)
```

توی این مرحله نوشته‌ها سفید و بقیه سیاه میشه (به دلیل THRESH_BINARY_INV)  
و متن از پس‌زمینه جدا میشه.

---

### ۵. حذف نویزهای ریز

```python
denoised = cv2.medianBlur(binary, 3)
kernel = cv2.getStructuringElement(cv2.MORPH_RECT, (2, 2))
denoised = cv2.morphologyEx(denoised, cv2.MORPH_OPEN, kernel)
```

اینجا لکه‌ها و نقطه‌های ریز که نویزن رو حذف می‌کنیم، که تصویر تمیز بشه.

---

### ۶. حذف لکه‌های خیلی کوچیک

```python
num_labels, labels, stats, _ = cv2.connectedComponentsWithStats(denoised, connectivity=8)
min_area = 30
cleaned = np.zeros_like(denoised)
for i in range(1, num_labels):
    if stats[i, cv2.CC_STAT_AREA] >= min_area:
        cleaned[labels == i] = 255
```

قطعات خیلی کوچیک‌تر از ۳۰ پیکسل رو حذف می‌کنیم چون نویزه.

---

### ۷. معکوس کردن رنگ‌ها

```python
cleaned_inverted = cv2.bitwise_not(cleaned)
```

تا پس‌زمینه سفید و متن سیاه بشه، که طبیعی‌تر دیده بشه.

---

### ۸. ذخیره و نمایش تصویر نهایی

```python
cv2.imwrite(save_path, cleaned_inverted)
```

همین‌جا عکس نهایی رو ذخیره می‌کنیم.  
با matplotlib هم نشون میدیم که بتونی مرحله به مرحله نتیجه رو ببینی.
![](assets/images/whiteboard_enhanced_example.png)
