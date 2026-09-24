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

### Missing OAuth scope for activity data

After successfully testing my access and refresh tokens, requests to 
`/athlete/activities` failed with a `missing activity:read_permission` 
error. The original authorization only requested the `read` scope, not 
`activity:read`. Resolved by re-running the browser authorization step with 
an expanded scope (`read,activity:read`) and `approval_prompt=force` to 
ensure Strava re-prompted for the new permission, then exchanging the fresh 
authorization code for a new token pair. 

When re-running the browser authorization, I was initially thown off by the localhost error I was getting, but after review I realized that was to be expected. 

### Formatting raw API responses for readability

Raw JSON from curl responses is hard to read as a single unbroken line in 
the terminal. Piped output through Python's built-in `json.tool` module to 
pretty-print it, saved the result to a JSON file, and opened it in VS Code 
for easier inspection of field names, nesting, and null values before 
writing any parsing logic.

### Reframing to CDC-benchmarked activity scoring

Originally planned generic fitness insights (weekly mileage trends, pace 
over time). Reframed to track weekly active minutes against the CDC's 
150-minute weekly activity recommendation, with a simple scoring system: 
2 points/minute for running, 1 point/minute for walking or hiking. The 2 points/minute is based on CDC's guidelines of 75 min of vigorous activity. Instead of scoring individually, the 2 points/min allows tracking vs the 150 min benchmark only. This ties the dashboard to a concrete external benchmark and my own 
return-to-running goals, rather than generic stats with no reference point.

### Tech Stack considerations

I was thinking of going to node.js/express/react since its the more commonly used, but I decided to use python+flask since I have previous python experience from my software engineering coursework. The core skills this project is meant to 
demonstrate API integration, OAuth, data transformation are language-agnostic, so prioritizing familiarity over stack popularity lets me focus on relearning coding fluency rather than a framework and language simultaneously.
## What's Next

Inspect real athlete data from Stravs 'athlete/activities' endpoint. Come up with 3 things to track against