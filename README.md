# Gruppeopgave

## Answer to Ex. 3.3.2
import requests
import json

construct_link('FOD', ['Tid=*', 'BARNKON=P']) #får et link med vores data

def function(x):
    ronja = requests.get(x) #laver en fil
    ronja = ronja.json() #unpack JSON payload

#trying to store the JSON file

    with open('ronja.json', 'w') as f:
        response_json_str = json.dumps(ronja) #skaber en string response_json_str ud fra vores girls.json fil
        f.write(response_json_str)
    return ronja

data = function('https://api.statbank.dk/v1/data/FOD/JSONSTAT?lang=en&Tid=*&BARNKON=P') #sætter linket fra construct link ind i functionen

## Answer to Ex. 6.1.4
df_weather['country'] = df_weather['station'].str[:3]
print(df_weather)

## Answer to Ex. 6.1.5
def vejr(aar):
    url = 'https://www1.ncdc.noaa.gov/pub/data/ghcn/daily/by_year/%i.csv.gz' %aar 

    df_weather = pd.read_csv(url,
                             compression='gzip',
                             header=None).iloc[:,:4]

    df_weather.columns = ['station', 'datetime', 'obs_type', 'obs_value']
    df_weather['obs_value'] = df_weather['obs_value'] / 10
    df_select = df_weather[(df_weather.station == 'ITE00100550') & (df_weather.obs_type == 'TMAX')].copy()
    df_select['TMAX_F'] = 32 + 1.8 * df_select['obs_value']
    df_sorted = df_select.reset_index(drop=True).sort_values(by=['obs_value'])
    return df_sorted

vejr(1867)
