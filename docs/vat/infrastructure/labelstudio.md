## Labelstudio Infrastructure

```docker-compose.yml
version: '3.8'
services:
  label-studio:
    image: test:latest
    environment:
#      - LABEL_STUDIO_DISABLE_SIGNUP_WITHOUT_LINK=true
      - LABEL_STUDIO_USERNAME=email@example.com
#      - LABEL_STUDIO_PASSWORD=123
    ports:
      - "8080:8080"
    volumes:
      - ./label-studio-data:/label-studio/data
```

docker compose up