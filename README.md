# Report-8-2


---

# 📊 گزارش پروژه: ماشین‌حساب ساده با کیپد ماتریسی (Simple Calculator with 4×4 Keypad)

## ۱. مقدمه  
در این پروژه، یک **کیپد ماتریسی 4×4** به آردوینو متصل شده است و با فشار دادن دکمه‌ها، یک ماشین‌حساب ساده ساخته شده است. این ماشین‌حساب قادر به:
- ورود دو عدد (مثلاً `12` و `5`)  
- انتخاب عملگر (`+`, `-`, `×`, `÷`)  
- محاسبه و نمایش نتیجه  
است..



## ۳. قطعات مورد استفاده  
- برد آردوینو Uno  
- کیپد ماتریسی 4×4 (با دکمه‌های `0–9`, `*`, `#`, `A–D` یا `F1–F4`)  
- برد برد و سیم‌های جامپر  

.
.

## ۵. کد برنامه (اسکچ)  

### 🔹 متغیرهای اصلی:
```cpp
float firstNumber = 0;   // عدد اول
float secondNumber = 0;  // عدد دوم
float result = 0;        // نتیجه
bool section = false;    // حالت: false = ورود عدد اول، true = ورود عدد دوم
int type = 0;            // نوع عملگر: 1=+, 2=-, 3=×, 4=÷
```

### 🔹 تعریف کیپد:
```cpp
char hexaKeys[ROWS][COLS] = {
  {'1','2','3','A'},  // A = +
  {'4','5','6','B'},  // B = -
  {'7','8','9','C'},  // C = ×
  {'*','0','#','D'}   // D = ÷
};
byte rowPins[] = {2, 3,  vier, 5};
byte colPins[] = {6, 7, 8, 9};
Keypad customKeypad = Keypad(makeKeymap(hexaKeys), rowPins, colPins, ROWS, COLS);
```

### 🔹 منطق اصلی در `loop()`:
```cpp
int button = int(customKeypad.getKey()) - 48;  // تبدیل '0'→0, '1'→1, ...
if (0 <= button && button <= 10) {  // اعداد 0 تا 9 + '*' (که 42 است، ولی در اینجا 10 محدود شده)
  if (!section) {
    firstNumber = firstNumber * 10 + button;
  } else {
    secondNumber = secondNumber * 10 + button;
  }
}
else {
  switch (button) {
    case 17: // 'A' → +
      section = true; type = 1; Serial.print("+"); break;
    case 18: // 'B' → -
      section = true; type = 2; Serial.print("-"); break;
    case 19: // 'C' → ×
      section = true; type = 3; Serial.print("*"); break;
    case 20: // 'D' → ÷
      section = true; type = 4; Serial.print("/"); break;
    case -6: // '#' → حذف (reset)
      firstNumber = secondNumber = result = 0;
      section = false; type = 0;
      Serial.println("= CLEAR");
      break;
    case -13: // '*' → محاسبه
      switch (type) {
        case 1: result = firstNumber + secondNumber; break;
        case 2: result = firstNumber - secondNumber; break;
        case 3: result = firstNumber * secondNumber; break;
        case 4: 
          if (secondNumber != 0) result = firstNumber / secondNumber;
          else result = 0; // جلوگیری از تقسیم بر صفر
          break;
      }
      Serial.print(" = "); Serial.println(result);
      // بازنشانی برای محاسبه بعدی
      firstNumber = secondNumber = 0;
      section = false;
      break;
  }
}
```

>.

##
## ۷. نتایج  
- ماشین‌حساب به درستی اعداد چند رقمی را پشتیبانی می‌کند.  
- عملگرهای `+`, `-`, `×`, `÷` به درستی انجام می‌شوند.  
- دکمه `#` برای پاک کردن و `*` برای محاسبه استفاده می‌شود.  
- نتیجه در Serial Monitor به صورت عدد اعشاری نمایش داده می‌شود.
.

---
