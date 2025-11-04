# 💸 SinaDollar API version : 1.0.3

وب‌سرویس SinaDollar یک سرویس سریع و دقیق برای دریافت قیمت لحظه‌ای دلار آزاد به ریال و تومان 🇮🇷💰 است.
با یک درخواست ساده‌ی GET، آخرین قیمت از وب‌سایت رسمی tgju.org واکشی و به‌صورت JSON برگردانده می‌شود.
مناسب برای ربات‌ها، وب‌اپ‌ها و پروژه‌های مالی و... ⚡

---

## 🌐 آدرس وب‌سرویس

👉 https://dollar.api-sina-free.workers.dev/dollar

---

## 🔹 ورودی‌ها
```نداره```

GET :

```GET https://dollar.api-sina-free.workers.dev/dollar```

---

## 🔹 خروجی‌ها

پارامتر | نوع |	توضیح ( به ترتیب )

```creator```
```string```
شناسه توسعه‌دهنده
```source```
```string```
منبع داده (tgju.org)
```price_rial```
```string```
قیمت دلار به ریال
```price_toman```
```number```
قیمت دلار به تومان
```updated_at```
```string```
(ISODate)	زمان به‌روزرسانی

---

## 🧾 نمونه خروجی

```{
  "creator": "@Sinabani_api",
  "منبع": "tgju.org",
  "price_rial": "1076400",
  "price_toman": 107640,
  "updated_at": "2025-11-01T10:29:37.201Z"
}
```

---

## 💻 نمونه استفاده در Python

```
import requests
from datetime import datetime

API_URL = "https://dollar.api-sina-free.workers.dev/dollar"

def fetch_dollar():
    try:
        res = requests.get(API_URL, timeout=5)
        res.raise_for_status()
        data = res.json()

        
        if "price_toman" not in data:
            print(" خطا در دریافت داده")
            return

        price_toman = data["price_toman"]
        price_rial = data["price_rial"]
        updated_at = data["updated_at"]
        creator = data.get("creator", "نامشخص")
        source = data.get("source", "tgju.org")

        time_str = datetime.fromisoformat(updated_at.replace("Z", "+00:00")).strftime("%Y-%m-%d %H:%M:%S")

        print(f"💰 به تومان: {price_toman:,} تومان")
        print(f"💵 به ریال:  {int(price_rial):,} ریال")
        print(f" بروزرسانی: {time_str}")
        print(f"🌐 منبع: {source}")
        print(f"👤 توسعه‌دهنده: {creator}")

    except requests.exceptions.Timeout:
        print(" سرور پاسخ نداد (Timeout).")
    except requests.exceptions.ConnectionError:
        print(" اتصال اینترنت برقرار نیست.")
    except Exception as e:
        print(" خطا", e)

if __name__ == "__main__":
    fetch_dollar()
```

---

## 🤖 نمونه استفاده در Rubika

```
from rubpy import Client, filters
import requests
from datetime import datetime

bot = Client(name="sina_dollar_bot")

API_URL = "https://dollar.api-sina-free.workers.dev/dollar"

@bot.on_message_updates(filters.text)
async def main(message):
    text = message.text.strip()

    if text in ["دلار", "Dollar", "dollar", "د‌لار"]:
        try:
            response = requests.get(API_URL, timeout=5)
            response.raise_for_status()
            data = response.json()

            if "price_toman" not in data:
                await message.reply("خطا در دریافت اطلاعات")
                return

            price_toman = data["price_toman"]
            price_rial = data["price_rial"]
            updated_at = data["updated_at"]
            creator = data.get("creator", "@Sinabani_api")
            source = data.get("source", "tgju.org")

            time_str = datetime.fromisoformat(updated_at.replace("Z", "+00:00")).strftime("%Y/%m/%d - %H:%M:%S")

            reply_text = (
                f"💸 قیمت لحظه‌ای دلار آزاد 🇮🇷\n"
                f"💰 به تومان: {price_toman:,} تومان\n"
                f"💵 به ریال: {int(price_rial):,} ریال\n"
                f"⏰ بروزرسانی: {time_str}\n"
                f"🌐 منبع: {source}\n"
                f"👤 توسعه‌دهنده: {creator}"
            )

            await message.reply(reply_text)

        except requests.exceptions.Timeout:
            await message.reply(" سرور پاسخ نمی‌دهد، لطفاً دوباره تلاش کنید.")
        except requests.exceptions.ConnectionError:
            await message.reply(" اتصال اینترنت برقرار نیست.")
        except Exception as e:
            await message.reply(f" خطا\n{e}")

bot.run()
```
---

## 📦 اطلاعات فنی

Method: GET          
Response Type: JSON          
Host: Cloudflare Workers          
Source: tgju.org          

---

👤 Developer: @Sinabanis          
📍 Hosted on: Cloudflare Workers          
🔗 Endpoint: https://dollar.api-sina-free.workers.dev/dollar          
