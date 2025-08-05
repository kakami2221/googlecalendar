# googlecalendar
from google.oauth2.credentials import Credentials
from google_auth_oauthlib.flow import InstalledAppFlow
from googleapiclient.discovery import build
import os
import datetime
import calendar
from datetime import date




SCOPES = ['https://www.googleapis.com/auth/calendar']

flow = InstalledAppFlow.from_client_secrets_file(r"C:\Users\.json", SCOPES)
creds = flow.run_local_server(port=0)
service = build('calendar', 'v3', credentials=creds)

def timead(today):
    dayof = date(today[0],today[1],today[2])
    year, month = dayof.year, dayof.month
    monthpp = calendar.monthrange(year, month)[1]

    if today[2] + 1 > monthpp:
        if today[1] == 12:
            return [today[0] + 1,1,1]
        else:
            return [today[0],today[1]+ 1,1]
    else:
        return [today[0],today[1],today[2] + 1]

n = 0
while n == 0:
    try:
        kadais = int(input('課題を何個追加しますか？：'))
        break
    except:
        print('誤入力')
        continue

djka = []
for i in range(kadais):
    while n == 0:
        try:
            lltime = list(map(int,input('課題の提出日(例：2025 7 1)：').split()))
            name = input('課題,イベントの名前：')
            djka.append([lltime,name])
            break
        except:
            print('誤入力')
            continue

for mytime in djka:
    time = date(mytime[0][0],mytime[0][1],mytime[0][2])
    timed = date(timead(mytime[0])[0],timead(mytime[0])[1],timead(mytime[0])[2])
    event = {
            'summary': mytime[1],
            'start': {  
                'date':time.isoformat()
            },
            'end': {
                'date':timed.isoformat()
            }
        }
    created_event = service.events().insert(calendarId='primary', body=event).execute()
