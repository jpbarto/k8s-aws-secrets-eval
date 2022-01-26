Test of AWS provider for K8s secrets

Based on:
- https://aws.amazon.com/blogs/security/how-to-use-aws-secrets-configuration-provider-with-kubernetes-secrets-store-csi-driver/
- https://github.com/aws/secrets-store-csi-driver-provider-aws
- https://secrets-store-csi-driver.sigs.k8s.io/
- https://www.eksworkshop.com/beginner/194_secrets_manager/sync_native_secrets_env/

After applying the secret provider and the deployment you should be able to execute:

```
kubectl exec -it $(kubectl get pods | awk '/nginx-deployment/{print $1}' | head -1) cat /mnt/secrets-store/MySecret; echo
```

and see the value of the AWS SecretsManager managed secret output.

to enable rotation of secrets use: (note this will rotate every 60 seconds)

```
helm upgrade -n kube-system csi-secrets-store secrets-store-csi-driver/secrets-store-csi-driver --set enableSecretRotation=true --set rotationPollInterval=60s --set syncSecret.enabled=true
```

note that this does not work for environment variable mounted secrets

note that the links above and this code deploys a secrets provider which retrieves secrets from secrets manager and makes them available as files on the container.  The container can reference the mounted secrets so they are synchronized with Kubernetes secrets and are then made available to the container as environment variables.  The Environment Variables however remain static even though the values of the files are rotated.
