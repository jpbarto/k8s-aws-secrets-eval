Test of AWS provider for K8s secrets

Based on:
- https://aws.amazon.com/blogs/security/how-to-use-aws-secrets-configuration-provider-with-kubernetes-secrets-store-csi-driver/
- https://github.com/aws/secrets-store-csi-driver-provider-aws
- https://secrets-store-csi-driver.sigs.k8s.io/

After applying the secret provider and the deployment you should be able to execute:

```
kubectl exec -it $(kubectl get pods | awk '/nginx-deployment/{print $1}' | head -1) cat /mnt/secrets-store/MySecret; echo
```

and see the value of the AWS SecretsManager managed secret output.
