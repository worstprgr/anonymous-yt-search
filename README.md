# Anonymous YT Search
I wanted to search YouTube without an account or API key via Google Cloud, while getting the search results as a JSON response.  
So I researched an anonymous alternative, and boiled down the request header & body to the mandatory items.

## 1. Get the 'Guest' API Key
**GET-Request:** `https://www.youtube.com/?themeRefresh=1`  
Search the response for: `"INNERTUBE_API_KEY":"<guest API key>"`  

You don't need a JS engine for that!

## 2. Search 
**POST-Request:** `https://www.youtube.com/youtubei/v1/search?key=<guest API key>&prettyPrint=false`  

### Header
```text
Content-Type: application/json  
Content-Length: <set length>  
Host: <set host>  
User-Agent: <set user agent>  
```

### Body
```json
{
    "context": {
        "client": {
            "userAgent": "<your user agent like in the head request>",
            "clientName": "WEB",
            "clientVersion": "2.20240123.06.00",
            "acceptHeader": "text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7"
        }
    },
    "query": "<your search query>"
}
```

# 3. Response
The response is using JSON and here's an example for accessing some items:
```python
response: dict = {...}
search_results = (response['contents']['twoColumnSearchResultsRenderer']['primaryContents']
                  ['sectionListRenderer']['contents'][0]['itemSectionRenderer']['contents'])

# First 5 search results
for x in range(5):
    video_renderer = search_results[x]['videoRenderer']
    video_id = video_renderer['videoId']
    video_title = video_renderer['title']['runs'][0]['text']
    print(f'Title: {video_title}\t\tID: {video_id}')
```

`video_id` is the ID inside the video url. So you can basically build:  
```python
f'https://www.youtube.com/watch?v={video_id}'
```
