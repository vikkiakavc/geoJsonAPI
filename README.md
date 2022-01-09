# geoJsonAPI
use of a GeoLocation lookup API modelled after the Google API to look up locations and parse the returned data.
import urllib.request , urllib.parse , urllib.error
import json
import ssl
api_key = 42
serviceurl = 'http://py4e-data.dr-chuck.net/json?'
ctx = ssl.create_default_context()
ctx.check_hostname = False
ctx.verify_mode = ssl.CERT_NONE
while True:
    address = input("enter the location:")
    if len(address) < 1:
        break
    parms = dict()
    parms['key'] = api_key
    parms['address'] = address
    URL = serviceurl + urllib.parse.urlencode(parms)
    print('retrieving', URL)
    data = urllib.request.urlopen(URL , context = ctx)
    uh = data.read().decode()
    print('retrieved' , len(uh) , 'characters')
    try:
        js = json.loads(uh)
    except:
        js = None
    print(json.dumps(js , indent = 4))

    if not js or 'status' not in js or js['status'] != 'OK':
        print('==== Failure To Retrieve ====')
        print(data)
        continue
    ans = js['results'][0]['place_id']
    print('place id', ans)
