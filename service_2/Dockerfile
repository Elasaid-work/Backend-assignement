#Stage 1: Build 

FROM python:3.13-alpine AS build

WORKDIR /app

RUN pip install uv

COPY pyproject.toml uv.lock ./

RUN uv sync

COPY . .



#Stage 2: Runtime 

FROM python:3.13-alpine AS runtime

WORKDIR /app
RUN apk add --no-cache curl

COPY --from=build /usr/local/lib/python3.13/site-packages /usr/local/lib/python>
COPY --from=build /usr/local/bin /usr/local/bin
COPY --from=build /app /app

EXPOSE 8002

CMD ["uv", "run", "app.py"]
