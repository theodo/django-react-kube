# Etapes pour déployer

- Créer le cluster, il faut chercher pour ajouter cloudsql
- Une fois cela fait connecter kubectl au cluster grâce à la commande fournie par GCP
- Déployer le namespace de prod
- `kubectl config set-context --current --namespace=production` + configurer kubectl pour l'utiliser par défaut
- Déployer les secrets et configmap:
  - `kubectl create secret generic cloudsql-oauth-credentials --from-file=credentials.json=<path-to-credentials-file>` pour le secret correspondant à l'accès au cloud sql (pour le proxy)
  - Déployer le secret contenant les infos de connexion à la base
  - Déployer le configmap contenant les variables d'env de Django
- Changer le host dans les `ALLOWED_HOST` de django et dans le `Dockerfile` du front pour qu'ils matchent le FQDN de votre prod
- Changer le lien vers la BDD dans `django-deployment.yaml`
- Pousser les différentes images nécessaires pour la prod sur le repo de container de gcp
- Changer les noms des images dans `django-deployment.yaml`, `react-deployment.yaml` et `statics-deployment.yaml`
- Apply ces trois fichiers
- Apply les services
- Ajouter votre host dans `ingress.yaml` (au même niveau de host, le format est host: hostname)
- Apply l'Ingress une fois tous les services et deployments ok
