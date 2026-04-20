# Étape 1 : récupération de MediaWiki 
FROM nginx:alpine AS downloader
RUN wget -q https://releases.wikimedia.org/mediawiki/1.36/mediawiki-1.36.0.tar.gz -O /tmp/mediawiki.tar.gz && mkdir -p /usr/share/nginx/wiki && tar xzf /tmp/mediawiki.tar.gz -C /usr/share/nginx/wiki --strip-components=1 && rm /tmp/mediawiki.tar.gz

# Étape 2 : image finale
FROM nginx:alpine
# Installation des dépendances PHP et outils en une seule RUN, avec nettoyage
RUN apk add --no-cache php php-fpm php-mysqli php-xml php-intl php-mbstring mariadb-client wget && rm -rf /var/cache/apk/*

# Copie de la configuration Nginx personnalisée
COPY wiki.conf /etc/nginx/http.d/default.conf

# Copie de MediaWiki depuis l’étape précédente (multi-stage)
COPY --from=downloader /usr/share/nginx/wiki /usr/share/nginx/wiki

# Création d’un utilisateur non‑root
RUN adduser -D -g '' nginxuser && chown -R nginxuser:nginxuser /usr/share/nginx/wiki

# Volume pour la configuration générée (LocalSettings.php) et les logs
VOLUME ["/usr/share/nginx/wiki", "/var/log/nginx"]

# Exposition du port
EXPOSE 80

# Lancement de PHP-FPM et Nginx
CMD php-fpm && nginx -g "daemon off;"
