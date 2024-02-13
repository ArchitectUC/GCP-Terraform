resource "google_compute_instance" "tf-instance-1"{
  name         = "tf-instance-1"
  machine_type = "e2-standard-2"
  zone         = "us-central1-c"

  boot_disk {
    initialize_params {
      image = "debian-11-bullseye-v20240110"
    }
  }

  network_interface {
    network    = module.vpc.network_self_link
    subnetwork = module.vpc.subnets_self_links[0]
  }

  allow_stopping_for_update = true
}

resource "google_compute_instance" "tf-instance-2"{
  name         = "tf-instance-2"
  machine_type = "e2-standard-2"
  zone         = "us-central1-c"

  boot_disk {
    initialize_params {
      image = "debian-11-bullseye-v20240110"
    }
  }

  network_interface {
    network    = module.vpc.network_self_link
    subnetwork = module.vpc.subnets_self_links[1]
  }

  allow_stopping_for_update = true
}

module "vpc" {
  source  = "terraform-google-modules/network/google"
  version = "~> 6.0.0"

  project_id   = "qwiklabs-gcp-03-c259d3c891d4"
  network_name = "tf-vpc-460994"
  routing_mode = "GLOBAL"

  subnets = [
    {
      subnet_name           = "subnet-01"
      subnet_ip             = "10.10.10.0/24"
      subnet_region         = "us-central1"
    },
    {
      subnet_name           = "subnet-02"
      subnet_ip             = "10.10.20.0/24"
      subnet_region         = "us-central1"
      subnet_private_access = "true"
      subnet_flow_logs      = "true"
      description           = "This subnet has a description"
    },
  ]
}
