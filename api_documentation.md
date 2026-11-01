# Module 4 Project - Part 1: API Exploration 

**Noelia Quiroga**
**October 30th, 2026**

## File that documents each API used:
1- Base URL:
    "https://jsonplaceholder.typicode.com"
    "https://pokeapi.co/api/v2"
    "https://api.restcountries.com/countries/v5/names.common/peru"

2- Authentication Method:
    Authentication for API 1 and API 2 :NONE
    API 3: REST Contries - Yes it required authentication for version 5.
    ###Authentication
    **Type: Bearer Token/API Key
    **Requirement: It required account registration to generate a access token
    **Header Format: 'Authorization: 'Bearer <token>'

3- Endpoints
#Endpoint 1.: Get All users
    Method: GET
    Path: /users
    Description: Searches and returns all users details

#Example response shape
[
    {
        "id": 1,
        "name": "Leanne Graham",
        "username": "Bret",
        "email": "Sincere@april.biz",
        "address": {
            "street": "Kulas Light",
            "suite": "Apt. 556",
            "city": "Gwenborough",
            "zipcode": "92998-3874",
            "geo": {
                "lat": "-37.3159",
                "lng": "81.1496"
            }
        },
        "phone": "1-770-736-8031 x56442",
        "website": "hildegard.org",
        "company": {
            "name": "Romaguera-Crona",
            "catchPhrase": "Multi-layered client-server neural-net",
            "bs": "harness real-time e-markets"
        }
    },
]      

#Endpoint 2.: Get all post by specific user
    Method: GET
    Path: /posts?userId=2
    Description: Retrieves a list of post filtered by specific user using a query       paramenter
    Example response shape:
    [
    {
        "userId": 2,
        "id": 11,
        "title": "et ea vero quia laudantium autem",
        "body": "delectus reiciendis molestiae occaecati non minima eveniet qui voluptatibus\naccusamus in eum beatae sit\nvel qui neque voluptates ut commodi qui incidunt\nut animi commodi"
    },
    {
        "userId": 3,
        "id": 12,
        "title": "in quibusdam tempore odit est dolorem",
        "body": "itaque id aut magnam\npraesentium quia et ea odit et ea voluptas et\nsapiente quia nihil amet occaecati quia id voluptatem\nincidunt ea est distinctio odio"
    },
    ]

#Endpoint 3.: Post a new post
    Method: POST
    Path: /posts
    Description: Adds a new post to the posts collection
    Example Response shape:(201 created)
        {
    "id": 101,
    "title": "string",
    "body": "string",
    "userId": 1
    }

#Endpoint 4.: Get a specific pokemon
    Method: GET
    Path: /pokemon/1
    Description: Search and retrieves details of the specific pokemon
    Example Response shape:
    {
    "id": 1,
    "name": "bulbasaur",
    "base_experience": 64,
    "height": 7,
    "is_default": true,
    "order": 1,
    "weight": 69,
    "abilities": [
        {
            "is_hidden": false,
            "slot": 1,
            "ability": {
                "name": "overgrow",
                "url": "https://pokeapi.co/api/v2/ability/65/"
            }
        },
        {
            "is_hidden": true,
            "slot": 3,
            "ability": {
                "name": "chlorophyll",
                "url": "https://pokeapi.co/api/v2/ability/34/"
            }
        }
    
],
    "past_abilities": [
        {
            "generation": {
                "name": "generation-iv",
                "url": "https://pokeapi.co/api/v2/generation/4/"
            },
            "abilities": [
                {
                    "is_hidden": true,
                    "slot": 3,
                    "ability": null
                }
            ]
        }
    ],
    "forms": [
        {
            "name": "bulbasaur",
            "url": "https://pokeapi.co/api/v2/pokemon-form/1/"
        }
    ],
    "game_indices": [
        {
            "game_index": 153,
            "version": {
                "name": "red",
                "url": "https://pokeapi.co/api/v2/version/1/"
            }
        },
        {
            "game_index": 153,
            "version": {
                "name": "blue",
                "url": "https://pokeapi.co/api/v2/version/2/"
            }
        },
        {
            "game_index": 153,
            "version": {
                "name": "yellow",
                "url": "https://pokeapi.co/api/v2/version/3/"
            }
        },
        {
            "game_index": 1,
            "version": {
                "name": "gold",
                "url": "https://pokeapi.co/api/v2/version/4/"
            }
        },
        {
            "game_index": 1,
            "version": {
                "name": "silver",
                "url": "https://pokeapi.co/api/v2/version/5/"
            }
        }
    ]
    }

4 - Rate Limits
    Rate limits vary accross the tested APIs:
        *API 1: JSONPlaceholder endpoints (/user, /posts, / nested user posts) returns active rate limiting headers in the response
            x-ratelimit-limit: total requests allowed within the tracking window
            x-ratelimit-remaining: the number remaining requests before slows down occurs
            x-ratelimit-reset: indicates when the limit window resets

        *API 2 - Pokemon & API 3 Rest Countries: neither returned X-Ratelimit headers in   the HTTP response.

5 - One thing that surprised me:
    1 - I encountered breaking changes with REST Countries: v3.1 has been deprecated and now requires transioning to the v5 API. This reqires registering for an account generaing an API key and passing it via the Authorization header to resolve the unauthorized error.

    2 - A key obsercation was teh complexity of the respones schema for REST countries APIs rather then returning flat attributes, the data is inside nested layers: response.json()['data']['objects], and it contains further nested dictionaries such as name['official'].  I inspected dictionairy keys via .keys(). to identify the path
    





