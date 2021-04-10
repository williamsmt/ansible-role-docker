# ansible-role-docker
![Build Test](https://github.com/williamsmt/ansible-role-docker/workflows/Build%20Test/badge.svg)

An application role to configure a virtual linux host with the Docker Engine and docker-compose utility. Generally, this role installs Docker, any dependencies and configures a local user to run Docker containers.

Requirements
------------

N/A

Role Variables
--------------

| Variable | Required | Default | Choices | Comments |
|----------|----------|---------|---------|----------|
| docker_packages | yes | docker-ce, docker-ce-cli, containerd.io | any available Docker package |      |
| docker_download_root | yes | https://download.docker.com/linux | any available repo | Leave this at default for most runs |
| docker_channel | yes | stable | stable,edge | Controls mirror used |
| docker_users | no | null | array of usernames | Users in this array are assigned to Docker group |
| docker_group | yes | docker |  | Leave this set as-is |
| install_docker_compose | yes | false | true,false |  |
| docker_bridge_subnet | no | null | any valid cidr | Only use is default docker bridge overlaps with existing network |
| docker_config_dir | yes | /etc/docker | any valid filepath | Leave this at default |
| docker_config_file | yes | daemon.json | any valid filename | Leave this at default |
| docker_compose_download_root | yes | https://github.com/docker/compose/releases/download | docker github | Leave this at default |
| docker_compose_version | yes | 1.27.4 | any valid version number | Floating version number with ~ latest |
| docker_compose_binary_location | yes | /usr/local/bin/docker-compose | any valid file path | Consider leaving this at default |
| docker_container_depend_dir | no | /opt/docker-files | any valid filepath | Leave this at default |

Dependencies
------------

No dependent roles required. Many of the tasks are currently conditioned for Debian flavor support, but can be easily modified to support other linux distributions. Please submit a PR for any enhancements you choose to include in the role.

Sample requirements.yml file for custom playbook:

    roles:
      - src: https://github.com/williamsmt/ansible-role-docker.git
        version: 21.4.1
        name: ansible-role-docker

To install this role using a requirements.yml file in the playbook directory:

`ansible-galaxy role install -r requirements.yml -p ./roles`

Example Playbook
----------------

Sample playbook passing common parameters for both image build (with Packer) and day 2 maintenance:

    - name: Build docker host
        hosts: all
        vars:
          docker_users:
            - localadmin
          docker_channel: stable
          install_docker_compose: true

        roles:
          - ./roles/ansible-role-docker

Run the playbook as:

`ansible-playbook -i {ip_address_of_host_or_inventory_file}, playbook.yml`

License
-------

Released under the [Apache-2.0 license](LICENSE)

Author Information
------------------

https://github.com/williamsmt

This is not an officially supported Google product