> GitLab-runner

GitLab Runner is an application that works with GitLab CI/CD to run jobs in a pipeline.

You can choose to install the GitLab Runner application on infrastructure that you own or manage. If you do, you should install GitLab Runner on a machine that’s separate from the one that hosts the GitLab instance for security and performance reasons. When you use separate machines, you can have different operating systems and tools, like Kubernetes or Docker, on each.

GitLab Runner is open-source and written in Go. It can be run as a single binary; no language-specific requirements are needed.

You can install GitLab Runner on several different supported operating systems. Other operating systems may also work, as long as you can compile a Go binary on them.

GitLab Runner can also run inside a Docker container or be deployed into a Kubernetes cluster.
