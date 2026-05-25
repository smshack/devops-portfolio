docker compose run --rm certbot certonly \
--webroot \
-w /var/www/certbot \
-d keycloak.smartseoapp.com \
--email 5432tat@naver.com \
--agree-tos \
--no-eff-email

docker compose run --rm certbot certonly \
--webroot \
-w /var/www/certbot \
-d gitlab.smartseoapp.com \
--email 5432tat@naver.com \
--agree-tos \
--no-eff-email