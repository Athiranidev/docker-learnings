# Lab 08 - Docker Networking Basics

## Objective

Learn the basics of Docker networking, inspect the default bridge network, and identify the IP address assigned to a container.

---

## Step 1 - List Docker Networks

Command:

```bash
docker network ls
```

Output:

```text
NETWORK ID     NAME      DRIVER    SCOPE
6c3acd2ece01   bridge    bridge    local
ec9369e5f69c   host      host      local
c61cdbc17fcf   none      null      local
```

### Observation

Docker provides three default networks:

- `bridge` - Default network
- `host` - Uses the host network
- `none` - No network connectivity

---

## Step 2 - Inspect the Bridge Network

Command:

```bash
docker network inspect bridge
```

Important information:

```text
Driver: bridge
Subnet: 172.17.0.0/16
Gateway: 172.17.0.1
```

Initially, the `Containers` section was empty because no containers were connected to the default bridge network.

---

## Step 3 - Create a Network Demo Container

Command:

```bash
docker run -dit --name network-demo ubuntu bash
```

### Options

```text
-d → Detached/background mode
-i → Interactive input
-t → Allocate a terminal
```

The container was created and kept running in the background.

---

## Step 4 - Inspect the Bridge Network Again

Command:

```bash
docker network inspect bridge
```

The container appeared under the `Containers` section.

Important information:

```text
Name: network-demo
IPv4Address: 172.17.0.2/16
```

Docker automatically assigned the container an IP address from the bridge network subnet.

---

## Step 5 - Enter the Container

Command:

```bash
docker exec -it network-demo bash
```

This opened an interactive Bash shell inside the running container.

---

## Step 6 - Check the Container IP

Inside the container:

```bash
hostname -I
```

Output:

```text
172.17.0.2
```

The IP matched the IP shown by Docker network inspection.

---

## Step 7 - Check Network Interface

Inside the container:

```bash
ip addr
```

Result:

```text
bash: ip: command not found
```

### Observation

The minimal Ubuntu image does not contain the `ip` command by default.

This was not a Docker networking failure.

---

## Lab Result

The container was successfully connected to the default Docker bridge network.

```text
Docker bridge network
        |
        |-- Subnet: 172.17.0.0/16
        |
        |-- Gateway: 172.17.0.1
        |
        └-- network-demo
                |
                └-- IP: 172.17.0.2
```

---

## Commands Used

```bash
docker network ls
```

```bash
docker network inspect bridge
```

```bash
docker run -dit --name network-demo ubuntu bash
```

```bash
docker exec -it network-demo bash
```

```bash
hostname -I
```

---

## Key Learnings

- Docker automatically connects containers to a network.
- The default network is `bridge`.
- Containers receive IP addresses from the network's subnet.
- The bridge network in this lab used `172.17.0.0/16`.
- The gateway was `172.17.0.1`.
- The `network-demo` container received `172.17.0.2`.
- Container-to-container communication will be explored in the next lab.