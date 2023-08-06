# Google_Map_Distance_Automation
Google Map Distance Automation Using Google Maps Distance Matrix API- It will automate the process of calculating distances and travel durations between various locations. 

**Google_Map_Distance_Automation** Project uses the Google Maps Distance Matrix API to calculate the distance and travel duration between pairs of locations. It enables users to input multiple origin-destination pairs and obtain real-time information about the distance and time it takes to travel between them.

**Use Cases:
Logistics and Delivery: Determine the optimal route and estimated time for deliveries and shipments.
Travel Planning: Plan routes and evaluate travel times for vacations or business trips.
Real Estate: Calculate distances between properties and important landmarks for better property evaluation.
Fitness Apps: Estimate distances for walking or running routes.
**


import requests
import pandas as pd
lat_long_input=pd.read_csv('lat_long_input.csv')
lat_long_input['Orgin_geocode']=lat_long_input['oc_lat'].astype(str) + "," + lat_long_input['oc_long'].astype(str)
lat_long_input['Dest_geocode']=lat_long_input['cn_lat'].astype(str) + "," + lat_long_input['cn_long'].astype(str)

lat_long_input['Distance_KM']="-"
lat_long_input['Distance_Hour']="-"


# Buy Distance Matrix API  from Google
api_key = "ertertretretretert-sfsdfdsf"

def fetch_gmaps_distances(src, dest):
    url = "https://maps.googleapis.com/maps/api/distancematrix/json?"

    response = requests.get(url + 'origins=' + src + '&destinations=' + dest +
                   '&key=' + api_key)
    
    result = response.json()
    
    if result["status"] == "OK":
        Distance_KM = round(result['rows'][0]['elements'][0]['distance']['value']/1000,0)
    else:
        Distance_KM="NA"
    
    return Distance_KM 


def fetch_gmaps_Hour(src, dest):
    url = "https://maps.googleapis.com/maps/api/distancematrix/json?"

    response_2 = requests.get(url + 'origins=' + src + '&destinations=' + dest +
                   '&key=' + api_key)
    
    result_2 = response_2.json()
    
    if result_2["status"] == "OK":
        Distance_Hour = round(result_2["rows"][0]["elements"][0]["duration"]["value"]/3600,2)
    else:
        Distance_Hour="NA"
    
    return Distance_Hour

for i in range(lat_long_input.shape[0]):
    print(i)
    
    geo1 = lat_long_input['Orgin_geocode'][i]
    geo2 = lat_long_input['Dest_geocode'][i]    
    dist = fetch_gmaps_distances(geo1,geo2)
    dist2 = fetch_gmaps_Hour(geo1,geo2)
    lat_long_input['Distance_KM'][i]=dist
    lat_long_input['Distance_Hour'][i]=dist2
    lat_long_input.to_excel("sachin_Sharma.xlsx",index=False)


print(lat_long_input.head())
lat_long_input.to_excel("sachin_Sharma.xlsx",index=False)
