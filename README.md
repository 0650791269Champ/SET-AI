# set_line_notify_bot/app.py
import requests
from bs4 import BeautifulSoup
import os

# ตั้งค่า LINE Messaging API
LINE_API_URL = "https://api.line.me/v2/bot/message/push"
LINE_TOKEN = os.getenv("4HDNCk2kjt0P/OykTSKwMIXoS1jwjVqXPc6hCdAxNTrrX0ERv8D77uTHB182fjOnar59sJhFJ3KVsCfUGerHfBQ5pQifIGH8CFJPvhVf8Xo7WzwaS4eWrlujnLTEyfSpSsi6x+QEc9It4NMzhiRN6QdB04t89/1O/w1cDnyilFU=")  # ใส่ Token ใน .env
LINE_USER_ID = os.getenv("U8db446d4735e422a488484c416c0c88f")  # ใส่ User ID

# ดึงข้อมูล SET Index

def get_set_index():
    url = "https://www.settrade.com/th/home"
    headers = {"User-Agent": "Mozilla/5.0"}
    response = requests.get(url, headers=headers)
    soup = BeautifulSoup(response.content, "html.parser")

    try:
        index = soup.select_one(".setindex .value" ).text.strip()
        change = soup.select_one(".setindex .change" ).text.strip()
        return index, change
    except:
        return None, None

# ส่งข้อความเข้า LINE

def send_line_message(message):
    headers = {
        "Content-Type": "application/json",
        "Authorization": f"Bearer {LINE_TOKEN}"
    }
    data = {
        "to": LINE_USER_ID,
        "messages": [{
            "type": "text",
            "text": message
        }]
    }
    requests.post(LINE_API_URL, headers=headers, json=data)

# MAIN
if __name__ == "__main__":
    index, change = get_set_index()
    if index:
        msg = f"\U0001F4C8 SET ล่าสุด\nดัชนี: {index}\nเปลี่ยนแปลง: {change}"
    else:
        msg = "\u26A0\uFE0F ไม่สามารถดึงข้อมูล SET ได้"

    send_line_message(msg)
    print("✅ ส่งข้อความเรียบร้อย")
