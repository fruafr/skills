# incus-doc

Offline authoritative [Incus](https://linuxcontainers.org/incus/docs/main/) CLI help and documentation.

It combines:
- a copy of the official online incus documentation in [./references/docs](./references/docs)
- a copy of cli help messages in [./references/cli](./references/cli)
- a copy of the man pages in [./references/man](./references/man)

It seeks to reduce hallucinations by routing all claims through verified reference files. 

Extracted for incus v. 6.0 as at 2026-07-06.

## Content

- [./SKILL.md](./SKILL.md): the skill file
- [./references](./references): contains the documentation used by the skill

# License

This skill is released under the open source [Apache-2.0 License](https://www.apache.org/licenses/LICENSE-2.0).

Incus is released under the open source [Apache-2.0 License](https://www.apache.org/licenses/LICENSE-2.0).

## Disclaimer

This skill is provided for demonstration and educational purposes only. Given the probabilistic nature of the LLM technology, the implementation and behavior of your harness may differ from what is shown in this skill.

Always test the skill thoroughly in your own environment before relying on it for critical tasks.

## Development

- [./references/cli](./references/cli): contains one txt file for each command / sub-command with help available (e.g. `incus --help`).
    - Entry point: [./references/cli-help.md](./references/cli-help.md)
    - List extracted: [./references/incus-cli.txt](./references/incus-cli.txt)
- [./references/docs](./references/docs): contains 
    - Entry point: [./references/docs/index.md](./references/docs/index.md)
    - List extracted: [./references/incus-doc-links.txt](./references/incus-doc-links.txt)
- [./references/man](./references/man): contains one txt file for each command / sub-command with man page available (e.g. `man incus`).
    - Entry point: [./references/cli-man.md](./references/cli-man.md)
    - List extracted: [./references/incus-man.txt](./references/incus-man.txt)

### Other files
- Date of extraction: [./references/extraction-date.txt](./references/extraction-date.txt)
- Incus version: [./references/incus-version.txt](./references/incus-version.txt)

## Author

David HEURTEVENT <david@heurtevent.org> for frua.fr

## What is incus ?

Incus is a truly free open-source command-line container and virtual machine manager. 

It provides a unified experience for running and managing :
- full Linux systems inside containers (shares the host kernel).
- full Linux or non-Linux systems in virtual machines (UEFI or not). 
- OCI container images (similar to Docker or Podman).

Incus supports container and virtual-machine images for a large number of Linux distributions (official Ubuntu images and images provided by the community).

It scales from one instance on a single machine to a cluster in a full data center rack, making it suitable for running workloads both for development and in production.

Incus allows you to easily set up a system that feels like a small private cloud. You can run any type of workload in an efficient way while keeping your resources optimized.

## Official sources of documentation
- [Incus - Documentation (on linuxcontainers)](https://linuxcontainers.org/incus/docs/main/)
- [Incus - Github](https://github.com/lxc/incus)
  - [doc/explanation](https://github.com/lxc/incus/tree/main/doc/explanation)
  - [doc/howto](https://github.com/lxc/incus/tree/main/doc/howto)
  - [doc/reference](https://github.com/lxc/incus/tree/main/doc/reference)
