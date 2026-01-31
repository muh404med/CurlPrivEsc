# CurlPrivEsc

```bash
TOKEN=''
URLFILE='urls.txt'

# Colors
GREEN='[0;32m'
YELLOW='[1;33m'
RED='[0;31m'
BLUE='[0;34m'
NC='[0m' # No Color

while read -r u; do
  for m in GET POST PUT DELETE; do

    if [[ "$m" == "GET" || "$m" == "DELETE" ]]; then
      code=$(curl -s -o /dev/null -w "%{http_code}" \n        -H "Authorization: token $TOKEN" \n        -X "$m" "$u")
    else
      code=$(curl -s -o /dev/null -w "%{http_code}" \n        -H "Authorization: token $TOKEN" \n        -H "Content-Type: application/json" \n        -X "$m" -d '{}' "$u")
    fi

    # Choose color based on status
    if [[ $code =~ ^2 ]]; then
      COLOR=$GREEN
    elif [[ $code =~ ^3 ]]; then
      COLOR=$YELLOW
    else
      COLOR=$RED
    fi

    printf "${BLUE}%-6s${NC} ${COLOR}%s${NC} %s
" "$m" "$code" "$u"

  done
done < "$URLFILE"
```
