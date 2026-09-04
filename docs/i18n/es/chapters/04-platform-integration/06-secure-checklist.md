# 4.6 Lista de comprobación de integración segura

Antes de dar una integración por lista, comprueba lo siguiente:

- La configuración del backend es correcta y no contiene ningún secreto en texto plano.
- La autenticación usa OIDC, una service connection o un token con alcance limitado, no una clave de nube de larga duración en el repositorio. **Sin esto:** una clave filtrada sigue funcionando hasta que alguien lo nota y la rota a mano, y normalmente concede más permisos de los que el pipeline necesita realmente.
- El state se almacena en un backend remoto con bloqueo o en un workspace de la plataforma, nunca en el control de versiones. **Sin bloqueo:** dos ejecuciones que escriben el state a la vez pueden corromperlo o aplicar en silencio la mitad de un plan obsoleto.
- `terraform plan` se ejecuta automáticamente en los pull requests y su salida es visible para quienes revisan.
- Los archivos de plan guardados y los artefactos de plan se tratan como sensibles: con acceso restringido, retención corta y nunca públicos.
- `terraform apply` se ejecuta solo después del merge, tras una aprobación obligatoria o un entorno protegido.
- Los secretos de CI/CD se almacenan en el gestor de secretos de la plataforma y se enmascaran en los logs.
- Las versiones de providers y módulos están fijadas, y el archivo de bloqueo de dependencias `.terraform.lock.hcl` está versionado. **Sin el archivo de bloqueo:** la misma configuración puede resolver una versión de provider distinta en la siguiente ejecución y aplicar cambios que nadie escribió.
- Las operaciones de destroy están deshabilitadas o requieren una aprobación aparte y explícita.
- Las comprobaciones de políticas (Sentinel, OPA o un linter) se ejecutan antes del apply cuando corresponde.
- El acceso se revisa cuando una persona, un token, un runner o un servicio cambia de rol.

La integración tiene éxito cuando hace que el cambio de infraestructura sea más trazable y repetible sin facilitar el uso indebido de las credenciales o de los cambios en producción.

## Referencias

- [State: sensitive data](https://developer.hashicorp.com/terraform/language/state/sensitive-data)
- [Terraform recommended practices](https://developer.hashicorp.com/terraform/cloud-docs/recommended-practices)
- [Dependency lock file](https://developer.hashicorp.com/terraform/language/files/dependency-lock)
- [Policy enforcement](https://developer.hashicorp.com/terraform/cloud-docs/policy-enforcement)
- [Injecting secrets into CI/CD (OIDC)](https://developer.hashicorp.com/terraform/tutorials/cloud/dynamic-credentials)
