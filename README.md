# app.py
import requests
from bs4 import BeautifulSoup
import os

# ✅ ค่า LINE API
LINE_API_URL = "https://api.line.me/v2/bot/message/push"
LINE_TOKEN = os.getenv("4HDNCk2kjt0P/OykTSKwMIXoS1jwjVqXPc6hCdAxNTrrX0ERv8D77uTHB182fjOnar59sJhFJ3KVsCfUGerHfBQ5pQifIGH8CFJPvhVf8Xo7WzwaS4eWrlujnLTEyfSpSsi6x+QEc9It4NMzhiRN6QdB04t89/1O/w1cDnyilFU=")
LINE_USER_ID = os.getenv("U8db446d4735e422a488484c416c0c88f")

def get_set_index():
    url = "https://www.settrade.com/th/home"
    headers = {"User-Agent": "Mozilla/5.0"}
    response = requests.get(url, headers=headers)
    soup = BeautifulSoup(response.content, "html.parser")

    try:
        index = soup.select_one(".setindex .value").text.strip()
        change = soup.select_one(".setindex .change").text.strip()
        return index, change
    except Exception as e:
        print("⚠️ Error parsing SET data:", e)
        return None, None

def send_line_message(message):
    if not LINE_TOKEN or not LINE_USER_ID:
        print("⚠️ LINE_TOKEN หรือ LINE_USER_ID ยังไม่ได้ตั้งค่าใน .env")
        return

    headers = {
        "Content-Type": "application/json",
        "Authorization": f"Bearer {LINE_TOKEN}"
    }

    data = {
        "to": LINE_USER_ID,
        "messages": [
            {
                "type": "text",
                "text": message
            }
        ]
    }

    response = requests.post(LINE_API_URL, headers=headers, json=data)
    if response.status_code == 200:
        print("✅ ส่งข้อความสำเร็จ")
    else:
        print(f"❌ ส่งไม่สำเร็จ: {response.status_code} | {response.text}")

if __name__ == "__main__":
    index, change = get_set_index()
    if index:
        msg = f"📈 SET ล่าสุด\nดัชนี: {index}\nเปลี่ยนแปลง: {change}"
    else:
        msg = "⚠️ ไม่สามารถดึงข้อมูล SET ได้"

    send_line_message(msg)
