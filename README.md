- 👋 Hi, I’m @Youdiie
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me ...

--->
import requests
import json
import datetime

vilage_weather_url = "http://apis.data.go.kr/1360000/VilageFcstInfoService/getVilageFcst?"

service_key = "XcHf%2BrlwS0Dmzke%2BLoY7exS7jPdGY32ldH%2BBQ%2BgLvHbOhssIUjzG3xWuGfTNSPCP2p3yHBHiX%2F4UkXCAMQkYwQ%3D%3D"

today = datetime.datetime.today()
base_date = today.strftime("%Y%m%d") # "20200214" == 기준 날짜
base_time = str(input("몇시 : ")) #시간

nx = "60"
ny = "128"

payload = "serviceKey=" + service_key + "&" +\
    "dataType=json" + "&" +\
    "base_date=" + base_date + "&" +\
    "base_time=" + base_time + "&" +\
    "nx=" + nx + "&" +\
    "ny=" + ny

# 값 요청
res = requests.get(vilage_weather_url + payload)

items = res.json().get('response').get('body').get('items')

# 필요한 딕셔너리 빼오기
data = dict()
data['date'] = base_date

weather_data = dict()
for item in items['item']:
    # 기온
    if item['category'] == "T3H": #item에서 기온정보(T3H) 가져오기
        weather_data['tmp'] = item['fcstValue'] #weather_data 딕셔너리에 넣기위해
        weather_tmp = weather_data['tmp'] 

    #습도
    if item['category'] == "REH":
        weather_data['hum'] = item['fcstValue']
        weather_humidity = weather_data['hum'] 

    
    # 기상상태
    if item['category'] == 'PTY':
        
        weather_code = item['fcstValue']
        
        if weather_code == '1':
            weather_state = '비'
        elif weather_code == '2':
            weather_state = '비/눈'
        elif weather_code == '3':
            weather_state = '눈'
        elif weather_code == '4':
            weather_state = '소나기'
        else:
            weather_state = '맑음'
        
        weather_data['code'] = weather_code
        weather_data['state'] = weather_state

#weather_state -> 기상상태
#weather_tmp -> 온도

print(data)
print(weather_data)
print("({0},1,1)".format(weather_humidity))
#온도바
print("({0},1,1)".format(weather_tmp))
#기상 상태
print(weather_state)
