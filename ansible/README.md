# do_blockstorage

This Ansible repo creates a 6 node GlusterFS cluster using DigitalOcean droplets, where each node is both a server and a client. 80% of the root volume is allocated as a [brick](https://gluster.readthedocs.io/en/latest/Administrator%20Guide/Setting%20Up%20Volumes/), and these six bricks are used to create a [dispersed volume](https://gluster.readthedocs.io/en/latest/Administrator%20Guide/Setting%20Up%20Volumes/?highlight=dispersed%20volumes#creating-dispersed-volumes), which can tolerate failure of up to two server nodes at a time.

Client mount points are created at `/mnt/gvol` on each node, and you end up with ~62GB usable space when running on 512MB droplets.

## Usage
### Pre-requisites
**NOTE:** Export your env vars *after* running `pipenv` / entering your virtualenv!

1. [pipenv](https://github.com/kennethreitz/pipenv) (optional but strongly recommended. `requirements.txt` is provided for those who'd rather setup their own virtualenv.)
2. `export DO_API_TOKEN=`[DigitalOcean personal access token with write scope](https://www.digitalocean.com/community/tutorials/how-to-use-the-digitalocean-api-v2#how-to-generate-a-personal-access-token)
3. `export SSH_PUB_KEY=/path/to/your.pub` If your preferred SSH key isn't available at `~/.ssh/id_rsa.pub`, override it with this env var. This key will be uploaded to your DigitalOcean account and used to SSH into instances.

### Running
1. `pipenv shell` to enter a virtualenv with all our required dependencies
2. Export any vars you need from pre-requisites above. `DO_API_TOKEN` is required.
3. `ansible-playbook site.yml` to setup the GlusterFS cluster.

## Tags
For use with Ansible [`--tags` or `--skip-tags`](https://docs.ansible.com/ansible/playbooks_tags.html):

| tag name | notes |
|-----|-------|
| `provision` | Creates Droplets. May want to skip for speed of execution when Droplets already exist |
| `tag` | Tags Droplets. May want to skip for speed of execution when Droplets are already tagged. |
| `do-agent` | Installs the [DigitalOcean Agent](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-the-digitalocean-agent-for-monitoring) for monitoring disk and memory usage, and process stats by CPU and memory. |
