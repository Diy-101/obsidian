```bash
curl -X POST "http://toxiproxy:8474/proxies" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "lotos-db-proxy",
    "listen": "0.0.0.0:9006",
    "upstream": "real-db-host:5432"
  }'
```

