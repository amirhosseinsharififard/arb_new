# Telegram Bot Setup Guide

# راهنمای راه‌اندازی ربات تلگرام

## تنظیمات اولیه / Initial Setup

### 1. ایجاد ربات تلگرام / Create Telegram Bot

1. به [@BotFather](https://t.me/BotFather) در تلگرام پیام دهید
2. دستور `/newbot` را ارسال کنید
3. نام ربات را انتخاب کنید (مثال: "Arbitrage Bot")
4. نام کاربری ربات را انتخاب کنید (مثال: "arbitrage_trading_bot")
5. توکن ربات را کپی کنید

### 2. دریافت Chat ID / Get Chat ID

1. به [@userinfobot](https://t.me/userinfobot) پیام دهید
2. Chat ID خود را کپی کنید

### 3. تنظیم متغیرهای محیطی / Environment Variables

فایل `.env` را در ریشه پروژه ایجاد کنید و متغیرهای زیر را اضافه کنید:

```bash
# Telegram Bot Configuration
TELEGRAM_ENABLED=1
TELEGRAM_BOT_TOKEN=your_bot_token_here
TELEGRAM_CHAT_ID=your_chat_id_here
```

### 4. مثال کامل / Complete Example

```bash
# Telegram Bot Settings
TELEGRAM_ENABLED=1
TELEGRAM_BOT_TOKEN=1234567890:ABCdefGHIjklMNOpqrsTUVwxyz
TELEGRAM_CHAT_ID=123456789

# Other bot settings
BOT_ENABLED=1
TRADING_ENABLED=1
MONITORING_ENABLED=1
LIVE_TRADING=0
```

## ویژگی‌های ربات تلگرام / Telegram Bot Features

### 📱 اطلاع‌رسانی‌های خودکار / Automatic Notifications

- **شروع ربات**: اطلاع‌رسانی هنگام شروع ربات با موجودی اولیه
- **باز شدن معامله**: اطلاع‌رسانی هنگام باز شدن هر معامله با آمار فعلی
- **بسته شدن معامله**: اطلاع‌رسانی هنگام بسته شدن هر معامله با آمار فعلی
- **توقف ربات**: خلاصه کامل روزانه با محاسبه سود/زیان
- **خطاها**: اطلاع‌رسانی خطاهای ربات
- **خلاصه ساعتی**: ارسال خلاصه هر ساعت (اگر ربات بیش از 1 ساعت اجرا شود)

### 📊 گزارش‌های روزانه / Daily Reports

ربات تلگرام خلاصه کاملی از فعالیت روزانه ارسال می‌کند شامل:

- مدت زمان فعالیت ربات
- موجودی اولیه و نهایی
- تعداد معاملات باز و بسته شده
- مجموع معاملات انجام شده
- نرخ موفقیت معاملات (درصد معاملات بسته شده نسبت به باز شده)
- محاسبه سود/زیان مطلق و درصدی
- وضعیت فعلی ربات

### 📈 آمار لحظه‌ای / Real-time Statistics

هر بار که معامله‌ای باز یا بسته می‌شود، آمار زیر نمایش داده می‌شود:

- تعداد معاملات باز شده تا آن لحظه
- تعداد معاملات بسته شده تا آن لحظه
- مجموع معاملات انجام شده
- نرخ موفقیت معاملات
- مدت زمان فعالیت ربات
- موجودی فعلی

### 💾 ذخیره وضعیت / Status Persistence

وضعیت ربات در فایل `bot_status.json` ذخیره می‌شود و شامل:

- زمان شروع و پایان
- موجودی‌های اولیه و نهایی
- آمار معاملات
- محاسبات سود/زیان

## نحوه استفاده / Usage

### اجرای ربات / Running the Bot

```bash
# اجرای ربات با پشتیبانی تلگرام
npm run funding_rate

# یا مستقیماً
node src/arbitrage_funding_rate/funding_rate_bot.js
```

### تست ربات تلگرام / Testing Telegram Bot

```bash
# اجرای تست‌های ربات تلگرام
npm run telegram_test

# یا مستقیماً
node src/arbitrage_funding_rate/telegram_test.js
```

تست‌ها شامل:

- ارسال پیام ساده
- تست اطلاع‌رسانی شروع ربات
- تست اطلاع‌رسانی‌های معامله
- تست آمار فعلی
- تست اطلاع‌رسانی خطا
- تست اطلاع‌رسانی توقف ربات

### توقف ربات / Stopping the Bot

- `Ctrl+C` برای توقف عادی
- ربات به صورت خودکار خلاصه نهایی را ارسال می‌کند

## عیب‌یابی / Troubleshooting

### مشکلات رایج / Common Issues

1. **ربات تلگرام کار نمی‌کند**:

   - بررسی کنید `TELEGRAM_ENABLED=1` باشد
   - توکن ربات و Chat ID را بررسی کنید
   - اطمینان حاصل کنید ربات به چت شما دسترسی دارد

2. **پیام‌ها ارسال نمی‌شوند**:

   - ابتدا پیام `/start` را به ربات ارسال کنید
   - Chat ID را دوباره بررسی کنید

3. **خطای اتصال**:
   - اتصال اینترنت را بررسی کنید
   - توکن ربات را دوباره از BotFather دریافت کنید

### لاگ‌ها / Logs

ربات تمام فعالیت‌های تلگرام را در کنسول نمایش می‌دهد:

```
📱 Initializing Telegram bot...
✅ Telegram bot initialized successfully
🚀 Bot started - sending Telegram notification
✅ Telegram message sent successfully
```

## امنیت / Security

- توکن ربات را هرگز در کد قرار ندهید
- از متغیرهای محیطی استفاده کنید
- فایل `.env` را در `.gitignore` قرار دهید
- Chat ID خود را محرمانه نگه دارید

## پشتیبانی / Support

در صورت بروز مشکل:

1. لاگ‌های کنسول را بررسی کنید
2. تنظیمات متغیرهای محیطی را بررسی کنید
3. اتصال اینترنت و دسترسی به تلگرام را بررسی کنید
