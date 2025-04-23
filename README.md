curl -v -X POST https://api.line.me/v2/bot/message/push \
-H "Content-Type: application/json" \
-H "Authorization: Bearer YOUR_LINE_TOKEN" \
-d '{
  "to": "4HDNCk2kjt0P/OykTSKwMIXoS1jwjVqXPc6hCdAxNTrrX0ERv8D77uTHB182fjOnar59sJhFJ3KVsCfUGerHfBQ5pQifIGH8CFJPvhVf8Xo7WzwaS4eWrlujnLTEyfSpSsi6x+QEc9It4NMzhiRN6QdB04t89/1O/w1cDnyilFU=",
  "messages":[
    {
      "type":"text",
      "text":"✅ ทดสอบส่งข้อความจากบอท"
    }
  ]
}'
