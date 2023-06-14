### Custom 명령어 저장위치
 - ~/bin 에 저장하면 모든 곳에서 호출가능

### Script
```
#!/bin/bash

# Usage: klogs[api|admin|rm] [blue|green]
#
# Options:
# -h, --help    Show this help message

# ------------------------------------------------------------------
# 도움말 호출

display_help() {
    echo "klogs \$SERVICE \$TARGET"
    echo -e "\nUsage:\n [api|aapi|admin|rm] [blue|green] \n"
}

if [ "$1" == "-h" ] || [ "$1" == "--help" ]; then
    display_help
    exit 1
fi
# ------------------------------------------------------------------
# 1. command 별 모듈 설정해주기

if [ "$1" == "api" ]; then
    SERVICE="service-api"
    CONTEXT="instantplay-api-us2"
elif [ "$1" == "aapi" ]; then
    SERVICE="service-admin-api"
    CONTEXT="instantplay-api-us2"
elif [ "$1" == "admin" ]; then
    SERVICE="service-admin"
    CONTEXT="instantplay-api-us2"
elif [ "$1" == "rm" ]; then
    SERVICE="resource-manager"
    CONTEXT="instantplay-rm-va"
fi

TARGET=$2
# ------------------------------------------------------------------
# 2. Conext, Version 지정

CONTEXT=$(kubectl config get-contexts -o name | grep $CONTEXT)

echo $CONTEXT
kubectl config use-context $CONTEXT

kubectl config get-contexts
kubectl get pod

echo Set Prerequired ==========================
echo Selected context : $CONTEXT
echo Selected env : $TARGET
sleep 2
echo Start logs=================================

if [ "$1" == "api" ] || [ "$1" == "admin" ]; then
    POD_NAMES=$(kubectl get pods -o json | jq '.items[].metadata.name' | grep "$SERVICE-$TARGET" |sed "s#\"##g")
elif [ "$1" == "admin" ]; then
    POD_NAMES=$(kubectl get pods -o json | jq '.items[].metadata.name' | grep "$SERVICE-[a-z0-9][a-z0-9][a-z0-9][a-z0-9][a-z0-9]" |sed "s#\"##g")
elif [ "$1" == "aapi" ]; then
    POD_NAMES=$(kubectl get pods -o json | jq '.items[].metadata.name' | grep "$SERVICE-[a-z0-9][a-z0-9][a-z0-9][a-z0-9][a-z0-9]" |sed "s#\"##g")
fi
# ------------------------------------------------------------------
# 3. POD 돌면서 kubectl log 호출
echo $POD_NAMES

for POD_NAME in $POD_NAMES; do
  echo -e "\033[31mLogs for $POD_NAME:\033[0m"
  sleep 2
  kubectl logs $POD_NAME -f
done

```


