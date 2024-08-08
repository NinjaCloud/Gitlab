```
provider "google" {
  credentials = file("<path-to-your-google-cloud-credentials-json>")
  project     = "<your-gcp-project-id>"
  region      = "<your-gcp-region>"
}

resource "google_compute_instance" "default" {
  name         = "terraform-instance"
  machine_type = "f1-micro"
  zone         = "us-central1-a"

  boot_disk {
    initialize_params {
      image = "debian-cloud/debian-9"
    }
  }

  network_interface {
    network = "default"
    access_config {}
  }
}

terraform {
  backend "s3" {
    bucket         = "your-s3-bucket-name"
    key            = "path/to/terraform.tfstate"
    region         = "your-aws-region"
    access_key     = "your-access-key-id"
    secret_key     = "your-secret-access-key"
  }
}
```
