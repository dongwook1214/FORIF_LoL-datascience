Riot API, Pandas 기초 (1)
========================
RiotWatcher 설치
----------------
###### pip install riotwatcher 로설치 할수있다
###### colab에서는 !를 붙이면 할수있다
###### ex)!pip install riotwatcher
#
#
#
Riot Watcher (1) : Data Dragon
--------------------
#### code &downarrow;&downarrow;

###### import pandas as pd
###### import matplotlib.pyplot as plt
###### from riotwatcher import LolWatcher
#
###### api_key = 'your api key'
###### watcher = LolWatcher(api_key)
#
###### versions = watcher.data_dragon.versions_for_region('kr')
###### champions_version = versions['n']['champion']
#
###### latest = watcher.data_dragon.versions_for_region('kr')['n']['champion']
###### static_champ_list = watcher.data_dragon.champions(latest, False, 'ko_KR')
###### static_champ_data = static_champ_list['data']

##### &uparrow;latest 는 최신 버전을 알려주고 static_champ_data는 챔피언 데이터를 알려주는데 static_champ_data['zoe']이런 식으로 입력하면 조이의 데이터만 읽을수있다.
#
#
#
What is Pandas?
-----------
##### pandas의 목적은 데이터를 분석하기 위해 필요한 data set을 만드는 것입니다. 이러한 과정을 ‘전처리(preprocessing)’라고 부르며, 가장 중요한 단계이기도 합니다.
#
#
#
pandas
-------
##### code &downarrow;&downarrow;
###### static_champ_data = static_champ_list['data']
###### champ_list = static_champ_data.keys()
###### champ_data = []
###### champ_stat = list(static_champ_data['Aatrox']['stats'].keys())
###### info_key = list(static_champ_data['Aatrox']['info'].keys())
#
###### my_col = ['id'] + champ_stat
#
###### for champ in champ_list:
######   &nbsp;&nbsp;temp = []
######   &nbsp;&nbsp;temp.append(static_champ_data[champ]['id'])
######   &nbsp;&nbsp;for stat in champ_stat:
######   &nbsp;&nbsp;&nbsp;&nbsp;  temp.append(static_champ_data[champ]['stats'][stat])
######   &nbsp;&nbsp;&nbsp;&nbsp; if len(static_champ_data[champ]['tags']) > 1:
######   &nbsp;&nbsp;&nbsp;&nbsp;  temp.append(static_champ_data[champ]['tags'][0])
######   &nbsp;&nbsp;&nbsp;&nbsp;  temp.append(static_champ_data[champ]['tags'][1])
######   &nbsp;&nbsp;else :
######   &nbsp;&nbsp;&nbsp;&nbsp;  temp.append(static_champ_data[champ]['tags'][0])
######   &nbsp;&nbsp;&nbsp;&nbsp;  temp.append(None)
#
######   for info in info_key:
######   &nbsp;&nbsp;  temp.append(static_champ_data[champ]['info'][info])
#
######   champ_data.append(temp)
#
###### my_col.extend(['primary_class', 'secondary_class', 'info_attack', 'nfo_defense', 'info_magic', 'info_difficulty'])
###### champ_data = pd.DataFrame(champ_data, index = champ_list, columns = my_col)
#
###### champ_data.head()
##### &uparrow; 챔피언 데이터에서 위에 몇개만 보여줌
###### champ_data.shape
##### &uparrow; (159, 27)반환 함 행,열 순서로 반환
###### champ_data.values
##### &uparrow; 판다스로 안 보여주고 그냥 알려줌
###### champ_data.columns
##### &uparrow; 열을 보여줌 ex)hp,mp,...
###### champ_data.index
##### &uparrow; 챔피언 이름 부분만 나옴

#
###### champ_hp = champ_data.sort_values("hp", ascending=False)
##### &uparrow; champ_data를 내림차순으로 정렬 ascending=True면 오름차순으로 정렬
###### champ_data.query("primary_class == 'Tank'")
##### &uparrow; primary_class == 'Tank'인거만 출력해줌
###### champ_data.groupby("primary_class").mean()
##### &uparrow; primary_class에 있는 종류인 Assassin,Fighter,Mage,Marksman,Support,Tank 이거를 행으로 해서 모든값을 평균내서 출력 평균내는거는 .mean() 이거임
