# CEREAL.ops
This repository regroups the configuration-as-code for the [CEREAL] project.
It is used to deploy CEREAL in [production] or [test] version.
It uses [Ansible] wrapped in a [suitcase] called [`cerealsible`](./cerealsible).

## Prerequisites
- Access to the [Keybase] team `/keybase/team/cepro_cereal/`
- Access to the prod & test VMs (as root)

## How it works
`./cerealsible`
- Install some dependencies on the VM (Docker, Git, cifs-utils, ...)
- Clone the git repository, copy the env variables from Keybase, start the MySQL container, build the Docker image, apply the Prisma migrations and run CEREAL's container
- Setup the daily database backup
- Configurate and run Apache in a Docker container

Use `./cerealsible --prod` to deploy on the production VM.

### Tags
- `cereal.setup`
    - Install necessary dependencies on the VM
- `cereal.run`
    - Clone the git repository, and do the necessary to start the project (MySQL container, docker image, Prisma migrations & container) : This is mostly what you want to do if you just made some changes to the project that you want to push in production.
- `cereal.backup`
    - Mount the backup share and setup the daily database backup crontab
- `cereal.apache`
    - Configurate Apache & certificates (certificate are on the Keybase) in `/data/apache/`, validate the configuration and run the `cereal-apache` container. Logs are available with `docker logs cereal-apache`

To use a tag : `./cerealsible -t cereal.setup` or `./cerealsible --prod -t cereal.setup`


[Ansible]: https://ansible.com/
[CEREAL]: https://github.com/epfl-cepro/cereal/
[Keybase]: https://keybase.io/
[production]: https://cereal.epfl.ch/
[suitcase]: https://github.com/epfl-si/ansible.suitcase/
[test]: https://cereal-test.epfl.ch/