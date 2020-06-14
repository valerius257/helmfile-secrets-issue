# Info

Example to show the issue with helmfile secretes when importing other helmfiles.

## Steps to reproduce

```shell
gpg --imprt example.asc
```

Check that it is working in simble example:

```shell
cd working/
helmfile -i template --skip-deps
```

```
Decrypting secret helmfile-secrets-issue/working/secrets.yaml
Decrypting helmfile-secrets-issue/working/secrets.yaml

Fetching incubator/raw
Templating release=secrets-example, chart=/tmp/053699211/secrets-example/0.2.3/incubator/raw/raw

# Source: raw/templates/resources.yaml
apiVersion: v1
data:
  secret: mysupersecretstring
kind: Secret
metadata:
  labels:
    app: raw
    chart: raw-0.2.3
    heritage: Helm
    release: secrets-example
  name: workaround
type: Opaque
```

Reproduce the issue

```
cd notworking/
helmfile -i template --skip-deps
```

```
Decrypting secret helmfile-secrets-issue/notworking/secrets.yaml
Decrypting helmfile-secrets-issue/notworking/secrets.yaml

in ./helmfile.yaml: in .helmfiles[0]: in default/helmfile.yaml: error during helmfile.yaml.part.0 parsing: template: stringTemplate:23:32: executing "stringTemplate" at <.Values.foo.bar>: map has no entry for key "foo"
```
