# Initialization
```
uv init
uv add fastapi
```

```main.py
from fastapi import FastAPI

from router import router as process_router

  

app = FastAPI()

app.include_router(process_router)
```

```router.py
from fastapi import APIRouter

import endpoint

router = APIRouter()

router.include_router(endpoint.router, prefix="/events", tags=["events"])
```

```endpoint.py
import json

from http import HTTPStatus

from fastapi import APIRouter

from pydantic import BaseModel

from starlette.responses import Response

import endpoint

  

router = APIRouter()

  

class EventSchema(BaseModel):

    """Event Schema"""

    event_id: str

    event_type: str

    event_data: dict

  

@router.get("/health_check")

async def health():

    return {"result": "healthy"}

  

@router.post("/", dependencies={})

async def handle_event(data: EventSchema,

)->Response:

    print(data)

    return Response(

        content=json.dumps({"message": "Data received!"}),

        status_code=HTTPStatus.ACCEPTED,

    )
```

# Run
```
uvicorn main:app --reload --port 9999
```