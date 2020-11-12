---
layout: project
title: All Projects
excerpt: "A List of Projects"
comments: false
---
``` python
from bs4 import BeautifulSoup
import requests
import time

# 여기에 신청한 본인의 key를 넣어야 합니다!
key = 'XrZiG7WA9X7Su9HwTRRiYBkpPFx5PCvadzxG81Ig89vxrTjgn3UgHjCUBQSnZGcoOwXn46kh1Ebd%2BXwnl9j53g%3D%3D'

numOfRows = '1000'
pageNo = '1'
# 도시 목록을 가져온다.
queryParams = 'ServiceKey='+key+'&numOfRows=' + numOfRows + '&pageNo='+pageNo
url = 'http://openapi.tago.go.kr/openapi/service/ExpBusInfoService/getExpBusTrminlList?serviceKey=XrZiG7WA9X7Su9HwTRRiYBkpPFx5PCvadzxG81Ig89vxrTjgn3UgHjCUBQSnZGcoOwXn46kh1Ebd%2BXwnl9j53g%3D%3D&numOfRows=1000&pageNo=1'
req = requests.get(url)
print(url)
html = req.text
soup = BeautifulSoup(html, 'html.parser')
my_content = soup.select('body > items > item > terminalId')
naek = []
for content in my_content:
    naek.append(content.text)

for i in naek:
    numOfRows = '100'
    pageNo = '1'
    depTerminalId = 'NAEK010'
    arrTerminalId = i
    depPlandTime = '20201112'
    busGradeId = ''
    queryParams = 'ServiceKey='+key+'&depTerminalId='+depTerminalId + \
        '&arrTerminalId='+arrTerminalId+'&depPlandTime='+depPlandTime
    url = 'http://openapi.tago.go.kr/openapi/service/ExpBusInfoService/getStrtpntAlocFndExpbusInfo?'+queryParams
    #print(url)
    req = requests.get(url)
    #print(type(req))
    html = req.text
    #print(type(html)) #이제 string객체로 바꼈다.
    #print(html[:100]) #이렇게 내용물을 확인할 수 있다.
    soup = BeautifulSoup(html, 'html.parser')
    my_content = soup.select('body > items > item >')
    for content in my_content:
        print(content.text+' ')

```