# مثال کامل استفاده از ربات تلگرام

# Complete Telegram Bot Usage Example

## تنظیمات اولیه / Initial Setup

### 1. ایجاد فایل .env

```bash
# فایل .env در ریشه پروژه
TELEGRAM_ENABLED=1
TELEGRAM_BOT_TOKEN=1234567890:ABCdefGHIjklMNOpqrsTUVwxyz
TELEGRAM_CHAT_ID=123456789

# تنظیمات ربات آربیتراژ
BOT_ENABLED=1
TRADING_ENABLED=1
MONITORING_ENABLED=1
LIVE_TRADING=0

# تنظیمات صرافی‌ها
ASTER_API_KEY=your_aster_api_key
ASTER_API_SECRET_KEY=your_aster_secret_key
HYPERLIQUID_API_KEY=your_hyperliquid_api_key
HYPERLIQUID_API_SECRET_KEY=your_hyperliquid_secret_key
```

### 2. تست ربات تلگرام

```bash
# اجرای تست‌ها
npm run telegram_test
```

### 3. اجرای ربات اصلی

```bash
# اجرای ربات با پشتیبانی تلگرام
npm run funding_rate
```

## نمونه پیام‌های دریافتی / Sample Messages

### پیام شروع ربات / Bot Start Message

```
🚀 ربات آربیتراژ شروع شد!

💰 موجودی:
• Aster: 100.00 USDT
• Hyperliquid: 50.00 USDC
• مجموع: 150.00 USD

⏰ زمان شروع: 14:30:25
📅 تاریخ: 1403/01/15

🤖 ربات آماده مانیتورینگ و معامله است...
```

### پیام باز شدن معامله / Trade Open Message

```
🟢 معامله باز شد:
• نماد: METUSDT
• مقدار: 15
• قیمت: 1.2345
• شناسه سفارش: ORDER_123456

📊 آمار فعلی:
• باز شده: 1
• بسته شده: 0
• مجموع: 1
• نرخ موفقیت: 0.0%
```

### پیام بسته شدن معامله / Trade Close Message

```
🔴 معامله بسته شد:
• نماد: METUSDT
• مقدار: 15
• قیمت: 1.2456
• شناسه سفارش: ORDER_789012

📊 آمار فعلی:
• باز شده: 1
• بسته شده: 1
• مجموع: 2
• نرخ موفقیت: 100.0%
```

### خلاصه ساعتی / Hourly Summary

```
⏰ خلاصه 1 ساعته ربات آربیتراژ:

⏱️ مدت فعالیت: 1 ساعت و 15 دقیقه
📅 تاریخ: 1403/01/15

💰 موجودی:
• Aster: 102.50 USDT
• Hyperliquid: 48.75 USDC
• مجموع: 151.25 USD

📈 آمار معاملات:
• معاملات باز شده: 3
• معاملات بسته شده: 2
• مجموع معاملات: 5
• نرخ موفقیت: 66.7%

🔄 ربات همچنان در حال اجرا است...
```

### خلاصه نهایی / Final Summary

```
📊 خلاصه روزانه ربات آربیتراژ:

⏰ مدت فعالیت: 480 دقیقه
📅 تاریخ: 1403/01/15

💰 موجودی:
• Aster: 100.00 USDT
• Hyperliquid: 50.00 USDC
• مجموع: 150.00 USD

📈 آمار معاملات:
• معاملات باز شده: 5
• معاملات بسته شده: 4
• مجموع معاملات: 9
• نرخ موفقیت: 80.0%

💰 موجودی:
• Aster: 105.25 USDT
• Hyperliquid: 47.80 USDC
• مجموع: 153.05 USD

📈 سود/زیان:
• مطلق: +3.05 USD
• درصدی: +2.03%

🔄 وضعیت: متوقف شده
```

## ویژگی‌های پیشرفته / Advanced Features

### 1. آمار لحظه‌ای / Real-time Statistics

هر معامله شامل آمار لحظه‌ای است:

- تعداد معاملات باز شده
- تعداد معاملات بسته شده
- مجموع معاملات
- نرخ موفقیت

### 2. محاسبه سود/زیان / P&L Calculation

- سود/زیان مطلق (USD)
- سود/زیان درصدی
- مقایسه موجودی اولیه و نهایی

### 3. ذخیره وضعیت / Status Persistence

وضعیت در فایل `bot_status.json` ذخیره می‌شود:

```json
{
  "startTime": "2024-01-15T14:30:25.000Z",
  "endTime": "2024-01-15T22:30:25.000Z",
  "startBalance": {
    "aster": 100.0,
    "hyperliquid": 50.0,
    "total": 150.0
  },
  "endBalance": {
    "aster": 105.25,
    "hyperliquid": 47.8,
    "total": 153.05
  },
  "trades": {
    "opened": 5,
    "closed": 4,
    "total": 9
  },
  "pnl": {
    "absolute": 3.05,
    "percentage": 2.03
  },
  "status": "stopped",
  "lastUpdate": "2024-01-15T22:30:25.000Z"
}
```

### 4. مدیریت خطا / Error Handling

خطاها به صورت خودکار گزارش می‌شوند:

```
❌ خطا در ربات آربیتراژ:

🔍 جزئیات خطا:
API connection timeout

⏰ زمان: 15:45:30
📅 تاریخ: 1403/01/15

🔄 ربات همچنان در حال تلاش است...
```

## نکات مهم / Important Notes

1. **امنیت**: توکن ربات و Chat ID را محرمانه نگه دارید
2. **پایداری**: ربات در صورت قطع اتصال، وضعیت را ذخیره می‌کند
3. **مانیتورینگ**: تمام فعالیت‌ها در کنسول و تلگرام ثبت می‌شوند
4. **تست**: قبل از استفاده واقعی، حتماً تست‌ها را اجرا کنید
5. **پشتیبان‌گیری**: فایل `bot_status.json` را به‌طور منظم پشتیبان‌گیری کنید
