Following the recent re-naming and re-addressing of my online resume from the url [mathieu.viales.fr](https://mathieu.viales.fr) to [resume-mathieu.viales.fr](https://resume-mathieu.viales.fr) I needed to setup a 301 redirection for SEO reasons. 

### DNS configuration

Ensure that both domains have an A record (or whatever) pointing to your ingress load balancer

### Application ingress config

The site's ingress is currently defined as follows 

```yaml
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: ssl-termination-ingress
  annotations:
    kubernetes.io/ingress.class: "nginx"
    cert-manager.io/cluster-issuer: letsencrypt-prod-custom
spec:
  tls:
    - hosts:
        - resume-mathieu.viales.fr
      secretName: resume-mathieu.viales.fr-cert
  rules:
    - host: resume-mathieu.viales.fr
      http:
        paths:
          - path: /
            backend:
              serviceName: resume-mathieu-viales
              servicePort: 80
```

Classic stuff, a cluster issuer and a secret are referenced to get the SSL certificate, a back-end is mapped on port 80 for the root path of the domain.

### Redirection ingress

To perform the redirection, configure an other ingress as follows (details after the code)

```yaml
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: dns-canonical-redirect
  annotations:
    kubernetes.io/ingress.class: "nginx"
    cert-manager.io/cluster-issuer: letsencrypt-prod-custom
    nginx.ingress.kubernetes.io/permanent-redirect: 'https://resume-mathieu.viales.fr'
spec:
  tls:
    - hosts:
        - mathieu.viales.fr
      secretName: resume-mathieu.viales.fr-cert
  rules:
    - host: mathieu.viales.fr
      http:
        paths:
          - path: /
            backend:
              serviceName: resume-mathieu-viales
              servicePort: 80
```

#### Annotations

Same as above, TLS configuration. But this time there's an nginx annotation added: `nginx.ingress.kubernetes.io/permanent-redirect`. **This annotation lets us specify an URL to use as the target for a 301 redirect**. If the protocol is specified, the target will be absolute. If the protocol is omitted, the redirection string will be considered to be a path relative to the current fully qualified URL.

A backend is specified through the "rules" node because an ingress definition requires a backend to be valid.

### Conclusion

Apply to your cluster, make sur to have an TLS cert configured for both domains, you're good

