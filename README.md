# Django GitOps repository

Argo CD deploys the Django application from `charts/django-app`.

## Runtime image

Jenkins updates these values after pushing an image to Amazon ECR:

```yaml
image:
  repository: <account>.dkr.ecr.eu-central-1.amazonaws.com/django-app
  tag: <jenkins-build>-<git-commit>
```

The Deployment renders them as:

```yaml
image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
```

## RDS connection

Terraform creates the Secret `django-app-secret` in the `django-app` namespace.
It contains `DB_HOST`, `DB_PORT`, `DB_NAME`, `DB_USER`, `DB_PASSWORD`, and
`SECRET_KEY`. The chart references the existing Secret and does not store
credentials in Git:

```yaml
existingSecret: django-app-secret
```
