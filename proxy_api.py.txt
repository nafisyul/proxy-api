from fastapi import FastAPI, Request
import httpx

app = FastAPI()

@app.get("/proxy")
async def proxy(url: str):
    try:
        async with httpx.AsyncClient() as client:
            response = await client.get(url)
        return {
            "status": response.status_code,
            "headers": dict(response.headers),
            "body": response.text
        }
    except Exception as e:
        return {"error": str(e)}