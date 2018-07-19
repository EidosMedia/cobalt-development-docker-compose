# Cobalt Development docker-compose

This compose project allows you to start a development server connect to a remote environment, to read production-like data.

Be sure to use the same version of cobalt as the one on the remot, e.g:

```yaml
image: dockerhub.eidosmedia.com/cobalt:2.8.0-SNAPSHOT
```

In order to attach to the correct remote server, you need to edit the `conf/cobalt/serviceDiscoveryRegistry.xml` file, replacing the right URLs of the server for each service you need to attach to:

```xml
<ServiceInfo id="cobalt-repo" 
    domain="${common:common.defaults.domain}" 
    zone="${common:common.defaults.zone}" 
    type="repository"
    uri="[http|https]://[hostname]:[port]/repository.rest"
    repository="${common:repository.name}" />
<ServiceInfo id="cobalt-directory"
    domain="${common:common.defaults.domain}"
    zone="${common:common.global.zone}"
    type="directory"
    uri="[http|https]://[hostname]:[port]/directory"
    realm="${common:common.defaults.realm}" />
```

In the docker-compose.yml you need also to configure the same repository name of the remote server:

```yaml
environment:
    - repository.name=repo-local
```

You need to configure the compose to start only the services you need to extend, for example:

```yaml
environment:
    - COBALT_SERVICES=site,directory
```

You can develop and then deploy your extensions by mounting the jars in the corresponding folder.

For directory service extensions:

```yaml
volumes:
    - /path/to/directory/service/extension.jar:/cobalt-dist/extra/lib/directory/extension.jar
```

For site service extensions:

```yaml
volumes:
    - /path/to/site/service/extension.jar:/cobalt-dist/extra/lib/site/extension.jar
```

