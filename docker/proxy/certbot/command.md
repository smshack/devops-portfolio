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


docker compose run --rm certbot certonly \
--webroot \
-w /var/www/certbot \
-d jenkins.smartseoapp.com \
--email 5432tat@naver.com \
--agree-tos \
--no-eff-email


docker compose run --rm certbot certonly \
--webroot \
-w /var/www/certbot \
-d sonarqube.smartseoapp.com \
--email 5432tat@naver.com \
--agree-tos \
--no-eff-email

docker compose run --rm certbot certonly \
--webroot \
-w /var/www/certbot \
-d prometheus.smartseoapp.com \
--email 5432tat@naver.com \
--agree-tos \
--no-eff-email

docker compose run --rm certbot certonly \
--webroot \
-w /var/www/certbot \
-d grafana.smartseoapp.com \
--email 5432tat@naver.com \
--agree-tos \
--no-eff-email

docker compose run --rm certbot certonly \
--webroot \
-w /var/www/certbot \
-d mattermost.smartseoapp.com \
--email 5432tat@naver.com \
--agree-tos \
--no-eff-email

docker compose run --rm certbot certonly \
--webroot \
-w /var/www/certbot \
-d kibana.smartseoapp.com \
--email 5432tat@naver.com \
--agree-tos \
--no-eff-email