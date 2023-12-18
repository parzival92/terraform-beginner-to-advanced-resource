### Terraform Import:

 ```sh
 terraform plan -generate-config-out=generated.tf
 ```

### Destroy

 ```sh
terraform destroy
terraform destroy -target aws_instance.myec2
```

### Target Flag

```sh
terraform destroy -target github_repository.secondgithubrepo
terraform apply -target github_repository.secondgithubrepo

```