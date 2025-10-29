# 📊 Database Module Documentation

# مستندات ماژول دیتابیس

## 📁 Structure / ساختار

```
src/database/
├── database.js              # Main database module
├── scripts/                 # Database utility scripts
│   ├── view_database.js     # Database structure viewer
│   ├── database_report.js   # Complete report generator
│   ├── advanced_query.js    # Advanced query tool
│   ├── interactive_db_manager.js # Interactive database manager
│   └── test_database.js    # Database testing script
└── README.md               # This documentation
```

## 🗄️ Database Schema / ساختار دیتابیس

### Tables / جداول:

1. **`bot_sessions`** - جلسات ربات

   - `id` (INTEGER PRIMARY KEY)
   - `start_time` (DATETIME)
   - `end_time` (DATETIME)
   - `start_balance_*` (REAL) - موجودی شروع
   - `end_balance_*` (REAL) - موجودی پایان
   - `pnl_*` (REAL) - سود/زیان
   - `trades_*` (INTEGER) - آمار معاملات
   - `status` (TEXT) - وضعیت جلسه

2. **`trades`** - معاملات

   - `id` (INTEGER PRIMARY KEY)
   - `session_id` (INTEGER FOREIGN KEY)
   - `trade_type` (TEXT) - 'open' یا 'close'
   - `symbol` (TEXT) - نماد معامله
   - `amount` (REAL) - مقدار
   - `price` (REAL) - قیمت
   - `order_id` (TEXT) - شناسه سفارش
   - `exchange` (TEXT) - صرافی
   - `timestamp` (DATETIME)

3. **`open_positions`** - پوزیشن‌های باز

   - `id` (INTEGER PRIMARY KEY)
   - `session_id` (INTEGER FOREIGN KEY)
   - `symbol` (TEXT) - نماد
   - `amount` (REAL) - مقدار
   - `entry_price` (REAL) - قیمت ورود
   - `order_id` (TEXT) - شناسه سفارش
   - `exchange` (TEXT) - صرافی
   - `opened_at` (DATETIME)

4. **`hourly_stats`** - آمار ساعتی

   - `id` (INTEGER PRIMARY KEY)
   - `session_id` (INTEGER FOREIGN KEY)
   - `hour_start` (DATETIME) - شروع ساعت
   - `trades_count` (INTEGER) - تعداد معاملات
   - `pnl_hourly` (REAL) - سود ساعتی
   - `balance_*` (REAL) - موجودی‌ها

5. **`daily_stats`** - آمار روزانه

   - `id` (INTEGER PRIMARY KEY)
   - `date` (DATE UNIQUE) - تاریخ
   - `sessions_count` (INTEGER) - تعداد جلسات
   - `total_trades` (INTEGER) - مجموع معاملات
   - `successful_trades` (INTEGER) - معاملات موفق
   - `failed_trades` (INTEGER) - معاملات ناموفق
   - `total_pnl` (REAL) - سود کل
   - `max_pnl` (REAL) - بیشترین سود
   - `min_pnl` (REAL) - کمترین سود

6. **`bot_events`** - رویدادهای ربات
   - `id` (INTEGER PRIMARY KEY)
   - `session_id` (INTEGER FOREIGN KEY)
   - `event_type` (TEXT) - نوع رویداد
   - `event_data` (TEXT) - داده رویداد (JSON)
   - `timestamp` (DATETIME)

## 🛠️ Usage / نحوه استفاده

### Main Module / ماژول اصلی:

```javascript
import {
  initializeDatabase,
  startBotSession,
  endBotSession,
  recordTrade,
  openPosition,
  closePosition,
  getBotStatus,
  getTradingSummary,
  closeDatabase,
} from "./src/database/database.js";
```

### Scripts / اسکریپت‌ها:

```bash
# View database structure
node src/database/scripts/view_database.js

# Generate complete report
node src/database/scripts/database_report.js

# Advanced queries
node src/database/scripts/advanced_query.js

# Interactive manager
node src/database/scripts/interactive_db_manager.js

# Test database
node src/database/scripts/test_database.js
```

## 📊 Key Functions / توابع کلیدی

### Session Management / مدیریت جلسات:

- `startBotSession(balance)` - شروع جلسه جدید
- `endBotSession(sessionId, endBalance, pnl)` - پایان جلسه
- `getCurrentSession()` - دریافت جلسه فعلی

### Trade Management / مدیریت معاملات:

- `recordTrade(sessionId, type, symbol, amount, price, orderId, exchange)` - ثبت معامله
- `openPosition(sessionId, symbol, amount, price, orderId, exchange)` - باز کردن پوزیشن
- `closePosition(orderId)` - بستن پوزیشن
- `getOpenPositions(sessionId)` - دریافت پوزیشن‌های باز

### Statistics / آمار:

- `getTradingSummary(hours)` - خلاصه معاملات
- `getDailyStats(date)` - آمار روزانه
- `recordHourlyStats(sessionId, tradesCount, pnl, balance)` - ثبت آمار ساعتی

### Reporting / گزارش‌گیری:

- `getBotStatus()` - وضعیت ربات
- `checkOpenPositionsOnStop()` - بررسی پوزیشن‌های باز هنگام توقف

## 🔧 Configuration / تنظیمات

Database file location: `data/arbitrage_bot.db`

The database is automatically created and initialized when the module is first imported.

## 📈 Performance / عملکرد

- Uses SQLite with better-sqlite3 for optimal performance
- Prepared statements for security and speed
- Foreign key constraints for data integrity
- Automatic indexing on primary keys

## 🚨 Important Notes / نکات مهم

1. Always call `initializeDatabase()` before using other functions
2. Call `closeDatabase()` when done to free resources
3. Database file is created automatically in `data/` directory
4. All timestamps are stored in ISO format
5. Foreign key constraints are enabled for data integrity
