## Question  
Your application generates large logs in `/var/log/myapp/` and there's no log rotation setup.

**Task:**  
Write a shell script that compresses logs older than 7 days and deletes logs older than 30 days. Also, run it daily via cron.

### 📝 Short Explanation  
This question tests your ability to manage disk space with log compression and retention — a common task in DevOps. You're expected to automate it safely and consistently using a cron job.

## ✅ Answer  

### 🖥️ Shell Script: `log_cleanup.sh`

```bash
#!/bin/bash

# Directory where logs are stored
LOG_DIR="/var/log/myapp"
LOG_FILE="/var/log/myapp/log_rotation.log"

# Ensure the log directory exists
if [ ! -d "$LOG_DIR" ]; then
    echo "[$(date)] ERROR: Log directory $LOG_DIR does not exist!" >> "$LOG_FILE"
    exit 1
fi

# Compress logs older than 7 days (but newer than 30)
find "$LOG_DIR" -type f -name "*.log" -mtime +7 -mtime -30 ! -name "*.gz" -exec gzip {} \; -exec echo "[$(date)] Compressed: {}" >> "$LOG_FILE" \;
# or you can use
# find "$LOG_DIR" -type f \( -name "*.log" -o -name "*.tar" \) -mtime +7 -mtime -30 ! -name "*.gz" -exec gzip {} \; -exec echo "[$(date)] Compressed: {}" >> "$LOG_FILE" \;

# but generally you will either have .gz files or .tar not both

# Delete compressed logs older than 30 days
find "$LOG_DIR" -type f -name "*.gz" -mtime +30 -exec rm -f {} \; -exec echo "[$(date)] Deleted: {}" >> "$LOG_FILE" \;

# Optional: Delete uncompressed logs older than 30 days
find "$LOG_DIR" -type f -name "*.log" -mtime +30 -exec rm -f {} \; -exec echo "[$(date)] Deleted (uncompressed): {}" >> "$LOG_FILE" \;


# Done
echo "[$(date)] Log rotation completed successfully." >> "$LOG_FILE"

```

**Save the script**

```bash
chmod +x script.sh
crontab -e #chose your editor
0 3 * * * /path/to/script.sh # optional 0 3 * * * /path/to/script.sh >> /path/logfile.log 2>&1
```
**Save and exit**
# Explanation of optional cmd 
What the command actually says:
- '2' : Refers to Standard Error.
- '>' : Means "redirect to."
- '&1' : Refers to the location of Standard Output (the & tells the shell it’s a file descriptor, not a file named "1").

Translation: "Take all the errors (2) and send them to the same place the normal output (1) is going."
