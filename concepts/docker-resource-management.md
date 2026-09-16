# Docker Resource Management

Docker resource management controls how much host resources a container can consume.

The main resources covered here are:

- CPU
- Memory
- Processes (PIDs)

---

## 1. CPU Resource Limit

Docker can limit how much CPU a container can use.

Example:

docker run -d \
  --name resource-demo \
  --cpus=0.5 \
  alpine \
  sh -c "while true; do :; done"

### --cpus=0.5

This gives the container a CPU limit of approximately 0.5 CPU.

The command:

sh -c "while true; do :; done"

creates an infinite CPU workload.

- sh -c → run the command using a shell
- while true → loop forever
- : → shell builtin that does nothing and returns success

The loop is used only to create CPU demand so the CPU limit can be observed.

Monitor usage:

docker stats resource-demo

Example:

CPU %     MEM USAGE / LIMIT
50.12%    432KiB / 128MiB

A CPU limit of 0.5 resulted in approximately 50% CPU usage in the lab.

---

## 2. Memory Resource Limit

Docker can limit the amount of memory a container can use.

Example:

docker run -d \
  --name memory-demo \
  --memory=128m \
  python:3.13-alpine \
  python -c "x = bytearray(200 * 1024 * 1024); import time; time.sleep(300)"

### --memory=128m

This sets the container memory cgroup limit to approximately 128 MiB.

The Python command attempts to allocate approximately 200 MiB:

bytearray(200 * 1024 * 1024)

Monitor memory usage:

docker stats memory-demo

Observed:

MEM USAGE / LIMIT
127.9MiB / 128MiB

This shows that the container reached its configured memory limit.

A memory limit does not mean the application is automatically killed the instant it reaches the displayed limit. If memory demand cannot be satisfied within the cgroup limit, OOM (Out Of Memory) behavior can occur and a process may be killed.

Check the container state:

docker inspect memory-demo \
  --format '{{.State.Status}} OOMKilled={{.State.OOMKilled}} ExitCode={{.State.ExitCode}}'

Example from the lab:

exited OOMKilled=false ExitCode=0

This means the container exited normally and was not recorded as OOM-killed.

---

## 3. PID Resource Limit

PID means Process ID.

A container can contain multiple processes.

Docker can limit how many processes a container can create:

docker run -d \
  --name pid-demo \
  --pids-limit=10 \
  alpine \
  sh -c "while true; do sleep 10; done"

### --pids-limit=10

This configures a maximum of 10 processes for the container.

Monitor the current process count:

docker stats pid-demo

The PIDS column shows the number of processes currently running inside the container.

---

## 4. Viewing Processes Inside a Container

Run:

docker exec pid-demo ps

Example:

PID   USER     TIME  COMMAND
1     root     0:00  sh -c while true; do sleep 10; done
64    root     0:00  sleep 10
65    root     0:00  ps

### Understanding the processes

PID 1:

sh -c while true; do sleep 10; done

This is the container's main process.

The shell repeatedly runs:

sleep 10

The sleep process is created, finishes after 10 seconds, and another sleep process is created.

When this command is executed:

docker exec pid-demo ps

Docker starts a temporary ps process inside the existing container.

Therefore, the PID of ps changes each time:

20 → ps
58 → ps
65 → ps
71 → ps
77 → ps

The PID number is not the same as the number of processes.

For example:

PIDS = 2

means there are two processes currently running.

---

## 5. Verify the PID Limit

Check the configured PID limit:

docker inspect pid-demo \
  --format '{{.HostConfig.PidsLimit}}'

Output:

10

This confirms that the container was created with:

--pids-limit=10

---

## 6. Monitoring vs Configuration

### docker stats

Shows current resource usage:

docker stats

It can show:

CPU %
MEM USAGE / LIMIT
MEM %
NET I/O
BLOCK I/O
PIDS

### docker inspect

Shows configured container settings, including resource limits.

Example:

docker inspect resource-demo \
  --format '{{.HostConfig.NanoCpus}} {{.HostConfig.Memory}}'

Example:

500000000 134217728

Interpretation:

500000000 NanoCPUs
→ 0.5 CPU

134217728 bytes
→ 128 MiB

For PID limit:

docker inspect pid-demo \
  --format '{{.HostConfig.PidsLimit}}'

Output:

10

---

## 7. Resource Management Mental Model

Docker Container
       |
       +---- CPU
       |      |
       |      +---- --cpus=0.5
       |
       +---- Memory
       |      |
       |      +---- --memory=128m
       |
       +---- Processes
              |
              +---- --pids-limit=10

Monitor actual usage:

docker stats

Verify configuration:

docker inspect

---

## 8. Why Resource Limits Matter

Without appropriate limits, one container can consume excessive host resources.

For example:

Host
├── Application A → high CPU usage
├── Application B → high memory usage
└── Application C → many processes

Resource limits provide boundaries:

Application A
└── CPU limit

Application B
└── Memory limit

Application C
└── PID limit

This helps with resource isolation and prevents a single workload from consuming an excessive amount of host resources.

Resource management can also contribute to better infrastructure utilization and cloud cost control, but its primary purpose is controlling container resource consumption.

---

## 9. Important Commands

CPU limit:

docker run --cpus=0.5 <image>

Memory limit:

docker run --memory=128m <image>

PID limit:

docker run --pids-limit=10 <image>

Monitor resources:

docker stats

Inspect container configuration:

docker inspect <container>

View processes inside a running container:

docker exec <container> ps

Remove a container:

docker rm -f <container>

---

## 10. Quick Revision

--cpus
→ controls CPU

--memory
→ controls memory

--pids-limit
→ controls number of processes

docker stats
→ shows current resource usage

docker inspect
→ shows configured limits/settings

docker exec
→ runs a command inside a running container

---

## 11. Core Mental Model

Resource Management
       |
       +-- CPU       → How much CPU?
       |
       +-- Memory    → How much RAM?
       |
       +-- PIDs      → How many processes?
       |
       +-- stats     → What is being used now?
       |
       +-- inspect   → What is configured?