helm repo add ory https://k8s.ory.sh/helm/charts
helm repo update

helm install \
    --set 'hydra.config.secrets.system={'$(LC_ALL=C tr -dc 'A-Za-z0-9' < /dev/urandom | base64 | head -c 32)'}' \
    --set 'hydra.config.dsn=memory' \
    --set 'hydra.config.urls.self.issuer=http://public.hydra.localhost/' \
    --set 'hydra.config.urls.login=http://example-idp.localhost/login' \
    --set 'hydra.config.urls.consent=http://example-idp.localhost/consent' \
    --set 'hydra.config.urls.logout=http://example-idp.localhost/logout' \
    --set 'ingress.public.enabled=true' \
    --set 'ingress.admin.enabled=true' \--set 'image.tag=latest-sqlite' \
    --set 'hydra.config.secrets.cookie=$(LC_ALL=C tr -dc 'A-Za-z0-9' < /dev/urandom | head -c 32 | base64)' \
    --name hydra
    --namespace ory-idp
    --create-namespace
    ory/hydra

helm upgrade --install hydra ory/hydra --namespace ory-idp --create-namespace --set 'hydra.config.secrets.system={'$(LC_ALL=C tr -dc 'A-Za-z0-9' < /dev/urandom | base64 | head -c 32)'}'     --set 'hydra.config.dsn=memory'     --set 'hydra.config.urls.self.issuer=http://public.hydra.localhost/'     --set 'hydra.config.urls.login=http://example-idp.localhost/login'     --set 'hydra.config.urls.consent=http://example-idp.localhost/consent'     --set 'hydra.config.urls.logout=http://example-idp.localhost/logout'     --set 'ingress.public.enabled=true'     --set 'ingress.admin.enabled=true' \--set 'image.tag=latest-sqlite'     --set 'hydra.config.secrets.cookie=$(LC_ALL=C tr -dc 'A-Za-z0-9' < /dev/urandom | head -c 32 | base64)'