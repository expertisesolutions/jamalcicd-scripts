# CI/CD Scripts

Scripts for Jenkins server and local tests for [Linux p4tc](https://github.com/p4tc-dev/linux-p4tc-pub).


## Local Tests
### Installation

Docker scripts for local tests are found in the [docker/local-tests](https://github.com/expertisesolutions/jamalcicd-scripts/tree/master/docker/local-tests) folder. Just clone this repository and copy this folder wherever you want.

### Requirements

```bash
Docker
Docker-compose
```

### Environment

Before execute the tests, it is necessary to configure the local environment.
The environment file can be found in the [docker/local-tests/.env](https://github.com/expertisesolutions/jamalcicd-scripts/blob/master/docker/local-tests/.env).

This environment file is used to configure the docker entrypoint to execute the [vm.sh file](https://github.com/p4tc-dev/linux-p4tc-pub/blob/master-next/tools/testing/selftests/tc-testing/vm.sh) correctly.

To maintain coherent tests and always updated with the newest version of the [linux-p4tc](https://github.com/p4tc-dev/linux-p4tc-pub) and [iproute2](https://github.com/p4tc-dev/iproute2-p4tc-pub), the docker scripts will use the local repositories as base to build and execute the tests. 

A .env file example is shown below:

```bash
#LOCAL PATH
LINUX_PATH="/home/user/linux-p4tc"
IPROUTE2_PATH="/home/user/iproute2-p4tc-pub"

#DOCKER PATH
SCRIPT="./tdc.py -c p4tc"
CONFIG="-f /home/linux/config-debug-p4tc-x86"
IMAGE="-i /home/linux/arch/x86/boot/bzImage"
ARCH=""
ROOT=""
CPU=""
MEMORY=""
JOBS=""
DRY=""
CONFIG_GEN=""
```
> !Local Path: The following variables use your local path.
- LINUX_PATH: Your local linux p4tc repository path.
- IPROUTE2_PATH: Your local iproute2 repository path.
> !Docker Path: The following variables use the path that is mapped inside docker to reference the files. Your LINUX_PATH is mapped to /home/linux and IPROUTE2_PATH is mapped to /home/iproute2.
- SCRIPT: Command that will run to execute the tests.
- CONFIG: Kernel configuration file to use. 
**Usage: -f /home/linux/<path to a config file inside linux p4tc folder>**
- IMAGE: Path to a precompiled bzImage to use. **Usage: -i /home/linux/<path to an image file inside linux p4tc folder>**
- ARCH: Architecture to use. **Usage: -a <arch name>**
- ROOT: Root filesystem to use. **Usage: -r <root path>**
- CPU: Number of vCPUs to use. **Usage: -c <number of cpus>**
- MEMORY: Size of vRAM to use. **Usage: -m <memory size>**
- JOBS: Number of compilation jobs. **Usage: -j <number of jobs>**
- DRY: Dry run and prints the QEMU command line. **Usage: -d**
- CONFIG_GEN: Generate a default kernel config. **Usage: -g**

### Execution

There are two modes to execute the local tests.

- Default: Run the script inside the docker and execute all the related tests.
- Interactive shell: Run docker and attach to the container, allowing to execute commands on the kernel.

#### Default

On the default mode, the SCRIPT defined on .env file will be executed inside the docker container.
With docker-compose installed, you can run the scripts with the follow command:
```bash
docker-compose up
```

#### Interactive shell

On the interactive shell mode, the SCRIPT will not be executed. Instead, a shell will be attached to the compiled kernel, allowing to execute commands directly.

With docker-compose installed, you can run the scripts with the follow command:
```bash
docker compose down --remove-orphans
docker-compose  up -d shell
docker attach p4tc_shell
docker compose down --remove-orphans
```
Or run the p4tc_shell.sh script:
```bash
./p4tc_shell.sh
```