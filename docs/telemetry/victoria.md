# Victoria
#### VIsual Collection Tracking Of Real-time Information Analysis

[![Streamlit App](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://polic-mbent.streamlit.app) [![View on GitHub](https://img.shields.io/badge/View%20on%20GitHub-000000?logo=github&logoColor=green)](https://github.com/SamVia/policumbent-data-visualizer)

## About:
This is a webapp made in Streamlit. It is used to display data received in realtime from the bike sensors or past races.
## Before working on the project:
You should have some basics in Python, know the bare minimum of databases and if you know HTML and CSS it's a plus. You will be working on Streamlit, firebase and sqlite3.
### 0. Streamlit:
The main component of this porject is streamlit, you should be confortable with the basis of it.
Useful material can be found here: [Streamlit Docs](https://docs.streamlit.io/)
Arguments I suggest to look up are: behaviour of a page, multipages app, how widgets work, caching of data and basic components to build on.
Also there is a blog where you could ask questions or find other useful concepts ([here](https://discuss.streamlit.io/)).
### 1. Prepare environment:
The project is built on Python 3.11.x, it could work on previous versions but it is higly discouraged.

Make sure you have streamlit latest version installed (either your computer on virtual environment): 
```bash
pip install streamlit
pip install --upgrade streamlit
```
You can use the requirements.txt file to install all the other libraries needed for the project. Make sure if you are planning to run new libraries that require installation on the streamlit cloud, they need to be added also to the requirements.

### 2. Streamlit Cloud:
The app can be run for free on the Streamit platform. On the other hand, it needs to be kept awake by either visiting the site or committing on Github (also empty commits are fine).
Every time you commit to Github the app will be updated, reloading every resource, be mindful that every temporary file not present in the repo will be erased.
Similar to Github there are "secrets", you can access the secrets via the console on the webapp.
#### 2.1 Streamlit Secrets:
To ensure safety in the application secrets are implemented. Constants like passwords, api keys and such are stored inside.

Streamlit supports toml formatting, you can write or use an already made script to convert possible keys from json to toml ([Script](https://github.com/SamVia/test_python/blob/main/key-to-toml.py)).
### 3. Databases:
* **Firebase**:
    The app uses a real time database hosted on Google Firebase. Using the free plan there is a limit on maximum read/write/delete daily. More info are present on the page of the database. Every interaction to see data on the database counts as a read for every instance (cycling through the whole database counts as many reads instead of one).
* **SqliteDB**:
    To prevent an overload of reads to see past data a local database hosted on streamlit is being used. Data is formatted in the same way. To prevent data erasure after updating the app, a system where the database is being uploaded to Github has been implemented.
  - to commit/pull the database to Github there are several steps. First the database is being hosted on another repo with just the db file. To update the database it needs to be encoded in base64 since the only working library that interfaces python with github requires it. To be able to read the database after pulling you would need to decode from base64.
 
## Testing:
For testing purposes a site that pushes data to the real-time database has been created. When the data stream starts, from the original app in the Live page, click on "check if race has started button".

[![Streamlit App](https://static.streamlit.io/badges/streamlit_badge_black_white.svg)](https://testpython.streamlit.app/)
## Data Formatting:
Data is going to be saved in the database on firebase for extraction in this mode:

>Start Collection: _yyyy_mm_dd
>>Document: timestamp (hh:mm:ss.ms)
>>>Fields: id int [0:2999]
>>>        timestamp string [hh:mm:ss.ms]
>>>        velocity float [x.x] in km/h
>>>        day int

* possible additions:
    - latitude, longitude float or following geojson formatting. For mapping purposes 
    - air quality in the vehicle
## Updating History:
The local database is updated after the race/test finishes, in other words when no new data is being insterted in the database.
To commit the changes you need to head to the update resources part ad insert the *very secure credentials* (user: policumbent, password: admin)
## Mapping:
future releases will work on it. Possibly using folium. 
