# Demo App using [FastAPI](https://fastapi.tiangolo.com/), GraphQL([Strawberry](https://strawberry.rocks/), [strawberry-sqlalchemy](https://github.com/strawberry-graphql/strawberry-sqlalchemy)), Docker
## System architecture
```mermaid
flowchart LR
    A[Browser] -->|GrapqhQL| B(FastAPI)
    B --> C[Strawberry]
    C --> D[strawberry-sqlalchemy]
    D --> E[SQLAlchemy]
    E --> F[(PostgreSQL)]
```


## How to run
1. Run Docker
- $ docker compose build --no-cache
- $ docker compose up
- $ docker compose start
2. DB migration
- $ task test
  - Before testing, DB migration, formatting by autoflake, black, isort, pyupgrade, and type checking by mypy are performed by [taskipy](https://github.com/taskipy/taskipy).
3. Run FastAPI
- $ uvicorn src.api.app:server --host 0.0.0.0 --reload
4. Access to GraghiQL
- http://localhost:8000/graphql

## Testing
- $ task test

## Demo with Render
1. Get token
```bash
$ curl -Ss \
-X POST \
-H "Content-Type: application/json" \
--data '{ "query": "mutation { login(input: { id: \"fceef692-010b-480f-899c-5a6e8bab23a7\", password: \"admin\" }) { tokenType accessToken severErrors { msg } } }" }' \
https://fastapi-strawberry-strawberry-sqlalchemy.onrender.com/graphql | jq .
```
```json
{
  "data": {
    "login": {
      "tokenType": "bearer",
      "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJmY2VlZjY5Mi0wMTBiLTQ4MGYtODk5Yy01YTZlOGJhYjIzYTciLCJleHAiOjE2OTU1NDg0MTh9.YFvZL07ZTFDURfdzaU_Xk096iz2nLdeJ2gBcgmL6xSA",
      "severErrors": []
    }
  }
}
```

2. Excec query
```bash
curl -Ss \
-H "Content-Type: application/json" \
-H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiJmY2VlZjY5Mi0wMTBiLTQ4MGYtODk5Yy01YTZlOGJhYjIzYTciLCJleHAiOjE2OTU1NDg0MTh9.YFvZL07ZTFDURfdzaU_Xk096iz2nLdeJ2gBcgmL6xSA" \
--data '{ "query": "query { cities { cities { cityId cityName population } } }" }' \
https://fastapi-strawberry-strawberry-sqlalchemy.onrender.com/graphql | jq .
```
```json
{
  "data": {
    "cities": {
      "cities": [
        {
          "cityId": 1,
          "cityName": "Los Angeles",
          "population": 3849000
        },
        {
          "cityId": 2,
          "cityName": "Santa Monica",
          "population": 91000
        },
        {
          "cityId": 3,
          "cityName": "Cebu",
          "population": 3000000
        }
      ]
    }
  }
}
```
