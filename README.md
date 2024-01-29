# System Metrics Bash Script

## Objectives
The purpose of this Bash script is to monitor and gather information about the performance and status of a computer system through various system metrics. This script provides valuable insights into the functioning of the system.

## Case Statement
The script begins with a user inquiry about which system metric they want to check. Users can choose from the following options:
1. Memory
2. Disk
3. CPU
4. Processes

The user's choice triggers one of the functions below.

## Functions

### 1. check_disk
The purpose of this function is to check whether disk space utilization surpasses a predefined threshold. It utilizes the `df` command and compares the current disk usage with the predefined threshold. If the disk usage exceeds the limit, it logs a message.

```bash
check_disk() {
    disk_threshold=90
    disk_usage=$(df -H | awk '$NF=="/" {print int($5)}')

    if [ "$disk_usage" -gt "$disk_threshold" ]; then
        log "High disk usage: $disk_usage%"
    else
        log "Disk usage within normal range: $disk_usage%"
    fi
}
```

### 2. check_cpu
This function evaluates the current CPU usage and checks if it exceeds a predefined threshold. It uses the `top` command to extract CPU usage information and compares it with the threshold.

```bash
check_cpu() {
    cpu_threshold=90
    cpu_usage=$(top -l 1 | awk '/CPU usage:/ {print int($3)}')

    if [ "$cpu_usage" -gt "$cpu_threshold" ]; then
        log "High CPU usage: $cpu_usage%"
    else
        log "CPU usage within normal range: $cpu_usage%"
    fi
}
```

### 3. processes_number
This function monitors the total number of processes running on the system and checks if it surpasses a predefined threshold.

```bash
processes_number() {
    process_threshold=300
    process_usage=$(top -l 1 | awk '/Processes:/ {print int($2)}')

    if [ "$process_usage" -gt "$process_threshold" ]; then
        log "Too many processes running: $process_usage"
    else
        log "Few processes running: $process_usage"
    fi
}
```

### 4. check_memory
This function examines the system's memory usage and checks if it exceeds a predefined threshold.

```bash
check_memory() {
    memory_threshold=50
    memory_usage=$(top -l 1 | grep -E 'PhysMem:' | awk '{print int($2)}')

    if [ "$memory_usage" -gt "$memory_threshold" ]; then
        log "Memory is almost full: $memory_usage%"
    else
        log "Memory usage within normal range: $memory_usage%"
    fi
}
```

### 5. Log function
The log function appends a log message with the current date to the "metrics.txt" file.

```bash
log() {
    echo "$(date +"%Y-%m-%d %H:%M:%S") - $1" >> metrics.txt
}
```

## Usage
To use the script, run it and select the desired system metric to check. The script will provide information about the chosen metric and log the result in the "metrics.txt" file with the current date.
