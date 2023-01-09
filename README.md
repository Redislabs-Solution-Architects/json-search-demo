# Redis JSON and Search Webinar 
This is the publicly available repo for RediSearch and JSON webinar with query problems and answers

**Prerequisites:**
- Azure Subscription
  - https://portal.azure.com/

**Expected Infrastructure:**
- Redis Cluster w/Search + JSON

**Included Code:**
- code.sh - script for demo automation
- --import
  - senators.py - loads senator data in JSON format (depends on senators.json in data dir)
  - import.py - leverages the open beer db (https://github.com/redis-developer/redis-datasets/tree/master/redisearch/openbeerdb/beerloader/data)


  

  
**Query Exercises**
- Attempt these queries (solutions below)
- RediSearch
    - Get the beers that have IPA in a field
    - Get the beers that are “North American Ales”
    - Get the beers with “Amreica” as the style (note the misspelling)
    - Get the beers that have a category of Lager
    - Get the beers that have a style of Lager but omit Amber
    - Get the beers that have an abv between 4 and 8
    - Find your favorite beer
    - Find all the beers of your favorite category
    - Find all the beers of your favorite category at your favorite brewery
    - Find all of the beers where the ABV is greater than 4.5
    - Find all the beers that have a style of Lager but omit Amber AND have an ABV between 6 and 10


- RedisJSON
    - Male senators over 50 years old
    - Female senators born in 1947
    - Senators with the name that sounds like Frank (Not sure if we want to show phonic search)
    - Total Count of Senators in the East Coast
    - Senators born after 1947 and name not starting with F
  
**Lastly**
- Tear Down cluster assets!

Solutions
---------------------------------------------------

**RediSearch**
- Get the beers that have IPA in a field:
FT.SEARCH "idx:beers" IPA
- Get the beers that are “North American Ales”:
FT.SEARCH "idx:categories" “North American Ales”
- Get the beers with “Amreica” as the style (note the misspelling):
FT.SEARCH "idx:styles" Amreica
- Get the beers that have a category of Lager:
FT.SEARCH "idx:categories" Lager
- Get the beers that have a style of Lager but omit Amber:
FT.SEARCH "idx:styles" "Lager -Amber"
- Get the beers that have an abv between 4 and 8:
FT.SEARCH idx:beers "@abv:[4 8]"
- Find your favorite beer
FT.Search idx:beers Pils
- Find all the beers of your favorite category
FT.Search idx:styles Pilsner
- Find all the beers of your favorite category at your favorite brewery
FT.SEARCH "idx:beers" "@category:Lager @brewery:Boulevard Brewing Company"
- Find all of the beers where the ABV is greater than 4.5
FT.SEARCH idx:beers "@abv:[(4 inf]"
- Find all the beers that have a style of Lager but omit Amber AND have an ABV between 6 and 10
FT.SEARCH idx:beers "@abv:[6 10] @style:Lager -Amber"

**RedisJSON**
- Find all male senators over 50 years old
FT.SEARCH idx:senators “@gender:(male) 

- Find all female senators born in 1947
FT.SEARCH idx:senators “@gender:(female) 

- Find the Total Count of Senators in the East Coast
 FT.AGGREGATE idx:senators "@state:(ME) | @state:(NH) | @state:(MA) | @state:(RI) | @state:(CT) | @state:(NY) | @state:(NJ) | @address:(DE) | @state:(MD) | @state:(VA) | @state:(NC) | @state:(SC) | @state:(GA)| @state:(FL)" GROUPBY 1 @state REDUCE count 0

- Find all senators born after 1947 and name not starting with F
FT.SEARCH idx:senators “@name:(-F*)




