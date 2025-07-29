# restaurant-finder

# Top-K Nearby Restaurants Finder with Spring Boot and Elasticsearch

# Use java17

# install elastic search using docker and run it thru below cmd:

docker run -d --name elasticsearch \
  -p 9200:9200 -p 9300:9300 \
  -e "discovery.type=single-node" \
  -v esdata:/usr/share/elasticsearch/data \
  docker.elastic.co/elasticsearch/elasticsearch:8.18.4

# ES query

# GET INDEX MAPPINGS

curl -X GET "http://localhost:9200/restaurants/_mapping?pretty"

# GET NEARBY 
curl -X GET "http://localhost:9200/restaurants/_search?pretty" -H 'Content-Type: application/json' -d'
{
  "size": 3,
  "query": {
    "geo_distance": {
      "distance": "200m",
      "location": {
        "lat": 28.6130,
        "lon": 77.2090
      }
    }
  },
  "sort": [
    { "rating": "desc" }
  ]
}'



# Get documents Count

curl -X GET "http://localhost:9200/restaurants/_count" -H "Content-Type: application/json"

# INSERT documents

curl -X POST "http://localhost:9200/restaurants/_bulk" -H "Content-Type: application/x-ndjson" -d '
{ "index": { } }
{ "name": "Spicy Biryani", "rating": 4.5, "reviewCount": 1200, "location": { "lat": 28.6130, "lon": 77.2090 } }
{ "index": { } }
{ "name": "Tandoori Flames", "rating": 4.2, "reviewCount": 980, "location": { "lat": 28.6145, "lon": 77.2080 } }
{ "index": { } }
{ "name": "Curry Kingdom", "rating": 4.8, "reviewCount": 1500, "location": { "lat": 28.6150, "lon": 77.2105 } }
{ "index": { } }
{ "name": "Pizza Delight", "rating": 4.1, "reviewCount": 720, "location": { "lat": 28.6120, "lon": 77.2075 } }
{ "index": { } }
{ "name": "Sushi Zen", "rating": 4.7, "reviewCount": 830, "location": { "lat": 28.6115, "lon": 77.2088 } }
'

# REST API request & response

## API GET

curl --location 'localhost:8080/restaurants/nearby?lat=28.6130&lon=77.2090&limit=2'

[
      {
            "reviewCount": 750,
            "distance_meters": 431.11593948889066,
            "name": "Momo Station",
            "rating": 4.3,
            "location": {
                  "lat": 28.6165,
                  "lon": 77.2071
            }
      },
      {
            "reviewCount": 800,
            "distance_meters": 395.1382063236317,
            "name": "Healthy Bites",
            "rating": 4.0,
            "location": {
                  "lat": 28.6095,
                  "lon": 77.2083
            }
      }
]
