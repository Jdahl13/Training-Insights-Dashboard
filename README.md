# Training Insights Dashboard

Dashboard that turns raw API data from Strava and weather into daily activity insights. Built as part of a transition from sales into a more technical role.

## Tech Stack:
- Strava API
- Weather API (TBD)
- Hosting (TBD)

## Decisions & Challenges:

### Strava API vs Apple Health or Garmin
Originally considered pulling directly from Apple Health or Garmin but Apple Health is iOS native only, and using Garmins API requires an application and approval for access and is not suited for a personal side project. Chose Strava because it has an open developer API and Apple Watch/Garmin typically synch there anyways.

### Discovered Strava API is no longer free
Was told it was free but discovered as of 6/2026 access requires a paid subscription. I thought about pivoting to another option, but decided to pay the monthly fee with developing this project.

### Testing auth manually via curl before writing code

Before writing any app code, I tested each piece of the auth chain 
independently via curl: verified the access token against the `/athlete` 
endpoint, then verified the refresh token by exchanging it for a new access 
token. This confirmed the Client ID, Secret, access token, and refresh token 
all worked correctly before introducing app code as a second variable to 
debug.

## What's Next

Pull and inspect real athlete data from Stravs 'athlete/activities' endpoint.