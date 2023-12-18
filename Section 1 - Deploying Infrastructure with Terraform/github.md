
#### GitHub Provider Terraform:

### https://registry.terraform.io/providers/integrations/github/latest/docs

## Code Used:

 ```sh

terraform {
  required_providers {
    github = {
      source  = "integrations/github"
      version = "~> 5.0"
    }
  }
}

provider "github" {
  token = "<<Your PAT token here>>"
}

resource "github_repository" "firstgithubrepo" {
  name        = "CreatedviaTerraform"
  description = "This repo has been created by terraform"

  visibility = "public"

}

resource "github_repository" "secondgithubrepo" {
  name        = "CreatedviaTerraform2"
  description = "This repo has been created by terraform"

  visibility = "public"

}
 ```
