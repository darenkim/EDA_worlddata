EDA project_worlddata
===================


안녕하세요? 패스트캠퍼스 데이터사이언스 15기 김경한입니다. 저의 첫 프로젝트였던 데이터탐색(EDA) 프로젝트를 소개합니다.

주제: OECD자료를 통한 인도네시아의 영유아사망율 요인 데이터탐색

> **Note:**
 > - 인도의 영유아 사망율을 다른 나라와 비교하여 분석합니다.
 > - 인도의  부정적 수치를 확인 후 영유아 사망율에 상관관계를 분석합니다.


**사용한 libraries**
> **libraries**

> - numpy - Used for fast matrix operations.
> - pandas - Used for performing Data Analysis.
> - seaborn - Used for statistical data visualization.
> - matplotlib - Used to create plots and Korean character
> - folium - Used for the map visualization.
> - glob -  Used for to read files


**사용한 Dataset**
> - OECD DATA
> (Air_pollution_exposure,  Alcohol consumption, childvaccin, Doctors, hospital_bed, Infant mortality rates)

------------------------------------------------------------------------------------

#### <i class="icon-folder-open"></i> **glob 이용하여 파일 한번에 불러오기**
```
dfs=[]
file_list = glob('./EDA_data/1. health/*.csv')
for each_file in file_list:
    df = pd.read_csv(each_file)
    dfs.append(df)
    
df = pd.concat(dfs)
```
-------------------------------------------

#### <i class="icon-folder-open"></i> ****2017년 유아사망율을 지도화해보기**
```
df_infant= df_2017[df_2017["INDICATOR"] =='INFANTMORTALITY']
df_infant = df_infant.sort_values(by ='Value', ascending = False)
df_infant['lat'] = [28.35, -30.5595, -0.7893,23.6345, -14.235, 38.9637, 40.1824, 9.7489, -35.6751 ,42.25748406,
                   61.52401,53.9333,48.669, 56.8796, 51.9194,46.2276, 55.3781, 56.2639, 52.1326, 50.8333, 39.0742,
                   47.1625, 46.8182, -35.4735, 51.165691, 49.8153, 31.046051, 53.1424, 55.1694, 47.5162, 35.90775700000000,
                   31.046051, 49.8175, 39.3999, 40.463667, 41.87194, 60.128161, 58.5953,60.472,46.1512, 61.92411, 36.204824 ]
df_infant['long'] = [77.12, 22.9375, 113.9213,-102.5528, -51.9253, 35.2433, 116.4142, -83.7534, -71.543, -78.02750466,
                    105.318756, -116.5765, 19.699, 24.6032, 19.1451, 2.2137, -3.4360, 9.5018, 5.2913, 4.469936, 21.8243,
                    19.5033, 8.2275, 149.0124, 10.451526, 6.1296, 34.851612, -7.6921, 23.8813, 14.5501, 127.766922,
                    34.851612, 15.473, -8.2245, -3.74922, 12.56738, 18.643501, 25.0136, 8.4689, 46.1512, 25.748151, 138.252924] 

# 영유아 사망율이 큰 국가의 folium circle을 크게 만들고자 사망율에 비례하여 radius를 설정해 주었습니다. 

from IPython.display import display
m = folium.Map()
for n in df_infant.index:
    folium.CircleMarker([df_infant['lat'][n], df_infant['long'][n]], radius = int(df_infant['Value'][n])
    , color='red', fill="True", fill_color='red', popup=df_infant['LOCATION'][n] + ":" + str(df_infant['Value'][n])).add_to(m)
display(m)

```
![enter image description here](https://user-images.githubusercontent.com/70153665/102324292-ffa3d900-3fc4-11eb-98ff-0c32de9842bb.png)



---------------------------------
#### <i class="icon-folder-open"></i> ****OECD국가중 인도는 영유아 사망율이 가장높다**

![enter image description here](https://user-images.githubusercontent.com/70153665/102324674-82c52f00-3fc5-11eb-8e69-99ed8454a6b8.png)





#### <i class="icon-pencil"></i> **인도의 건강자료만 모아보기**
```
# 왜 인도의 영유아사망율이 높은지 분석해보고자 인도를 중심으로 다른 나라와 여러요소들을 비교하여 인도의 취약부분을 살펴보겠습니다.

df_ind = df_2017[df_2017["LOCATION"] == 'IND']
df_ind = df_ind.sort_values('Value', ascending= False)
df_ind['Value'] = round(df_ind['Value'], 2)
df_ind.reset_index(drop=True, inplace = True)


# 인도의 2017년도 지표는 다음과 같습니다.. 수치 하나하나를 파악하기 보다는 다른 나라와 비교해보도록 하겠습니다.
# 인도는 어느정도의 위치를 차지하고 있을까요?

plt.figure(figsize=(15,4))
ind = sns.barplot(x= 'INDICATOR', y='Value', data = df_ind)
for i in range(df_ind.shape[0]):
    ind.text(x=i, y = df_ind['Value'][i], s=df_ind['Value'][i],fontsize=13, horizontalalignment='center')
plt.ylim(0,120)
plt.title("인도");
```
![enter image description here](https://user-images.githubusercontent.com/70153665/102324960-ea7b7a00-3fc5-11eb-910a-52d81f965e78.png)


#### <i class="icon-pencil"></i> **인도의 보건자료 세계국가와 비교해보기**
![enter image description here](https://user-images.githubusercontent.com/70153665/102325978-295dff80-3fc7-11eb-950c-861de8fadb92.png)

* 인도의 공기오염도는 OECD국가와 비교해봤을때 상당히 높습니다.


--------------------------------------

![enter image description here](https://user-images.githubusercontent.com/70153665/102326193-6e823180-3fc7-11eb-932c-26f6da1aa574.png)

* 1000명당 의사수는 가장 하위권입니다.


------------------------------------------
![enter image description here](https://user-images.githubusercontent.com/70153665/102326344-a5584780-3fc7-11eb-849e-53c803e4e278.png)

* 1000명당 병상수또한 가장 하위권에 속해있습니다.

------------------------------------
![enter image description here](https://user-images.githubusercontent.com/70153665/102326594-ee100080-3fc7-11eb-85c3-e60bfb786faa.png)

* 어린이 백신접종율은 높은 편이지만 여전히 하위권에 속해있습니다.

#### <i class="icon-pencil"></i> **인도 자료만 연도별로 뽑아 보고 상관관계를 분석해 보기**

![enter image description here](https://user-images.githubusercontent.com/70153665/102328657-afc81080-3fca-11eb-9882-6815a7bd6b20.png)
![enter image description here](https://user-images.githubusercontent.com/70153665/102328668-b2c30100-3fca-11eb-8223-7292e04d568f.png)
> * 눈에 띄는 특이한 점은 없었지만 2000년 즈음에 알코올 소비량과 영유아 백신접종율이 급격하게 떨어지는 것을 볼 수 있습니다.
> * 인도는 1998년 핵실험을 진행해서 미국과 일본의 제재를 받게 되었는데, 해외 원조와 무역에 제재를 받게 되어 그런 것이 아닐까 생각합니다.
> * 데이터는 거짓말을 하지 않는다...


#### <i class="icon-pencil"></i> **히트맵 그려보기**

![enter image description here](https://user-images.githubusercontent.com/70153665/102329906-45b06b00-3fcc-11eb-8c57-5c46f3a28510.png)

> **히트맵을 통한 시사점**

> * 의사수, 대기오염 노출은 지속적으로 증가하고 있으며 영유아 사망율은 감소하고 있습니다.(강한 상관관계)
> * 인도의 병상수는 증감이 지속적으로 나타나 상관관계가 확실히 나오지 않습니다.

>**- 인도의 유아사망율 중에 가장 높은 이유가 폐렴(18%)이라고 합니다.**
>**- 대기오염의 노출도와 상당히 관련이 있는 것으로 생각됩니다.** 
>> *인도의 인구증가수는 G7 국가와 비교하여 높은 상황이지만, 보건쪽은 앞서 살펴본 바와 같이 다른 국가와 비교하여 하위권을 맴돌고 있습니다.* 
>> *늘어나는 인구에 비하여 열악한 보건환경과 심각한 공기오염이 영유아사망율을 높인다고 생각됩니다.*


----------


**그냥 끝내기 아쉽네요.**
-------------------

재미있는 자료가 있어 공유합니다.
 이 글을 보시는 여러분(한국인)은 어떤 고기를 가장 좋아하시나요? 회식으로 또는 친구들과 어느 곳으로 모임을 가지시나요?

**소고기, 돼지고기, 닭고기, 양고기에 대한 세계소비량을 살펴보았습니다.**

![enter image description here](https://user-images.githubusercontent.com/70153665/102331250-f1a68600-3fcd-11eb-87ed-850170eba87c.png)

* 총 고기 소비량(소,돼지,닭,양 고기 소비를 합침)순으로 순서를 지정하였습니다.
* 소고기 소비량은 아르헨티나임을 확인할 수 있습니다.(소고기 가격이 상당히 싼 것을 신문기사,TV를 통해 확인 할 수 있습니다.)
* 한국은 소고기 소비량도 적은 편이 아니지만 눈에 띄는 점은 돼지고기 소비량이 월등히 높은 것 입니다.(저도 돼지고기를 훨씬 많이 먹습니다. 삼겹살..)
* 이스라엘 또한 성경적으로 돼지고기를 먹으면 안되는 구절때문에 총 고기소비량은 많지만 돼지고기는 다른 나라에 비해 상당히 적은 것을 알 수 있습니다. 

![enter image description here](https://user-images.githubusercontent.com/70153665/102331604-67125680-3fce-11eb-94bf-f7dad05d20c4.png)

* 고기 소비량이 상대적으로 적은 국가를 살펴보면 이슬람, 힌두교 등 종교적인 이유로 고기소비량이 적은것을 알 수있습니다.

* 데이터는 거짓말을 하지 않는다.
