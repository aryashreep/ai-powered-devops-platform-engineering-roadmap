# 🧪 Lab: Text Pipeline Builder

> **Goal:** Chain Linux text utilities into powerful data-processing pipelines.

## Target Objectives

- Use redirection operators to route data
- Build grep pipelines to filter logs
- Sort and deduplicate data
- Extract columns with cut and sed

---

## Hands-on Challenges

### 1. Create a Sample File
```bash
cat > /tmp/sample-data.txt << EOF
2024-01-15 10:23:45 ERROR timeout connecting to db
2024-01-15 10:23:46 INFO retry attempt 1
2024-01-15 10:23:47 ERROR connection refused
2024-01-15 10:25:00 WARN disk usage at 85%
2024-01-15 10:30:00 ERROR timeout connecting to db
2024-01-15 10:31:00 INFO health check passed
EOF
```

### 2. Filter with grep
Find all ERROR lines. Count how many there are. Find lines containing "db" or "disk".

### 3. Pipeline Chain
Build a single pipeline that:
- Reads the file
- Filters for ERROR
- Extracts just the timestamp and error message
- Sorts by timestamp
Saves output to `solution/errors-sorted.txt`

### 4. Apache Log Analysis
If you have `/var/log/apache2/access.log` (or create a sample one), find:
- Top 10 IP addresses
- Most requested URLs
- Count of 404 responses

### 5. sed Substitution
Use sed to replace all `ERROR` with `❌ ERROR` and save to a new file.

---

## Proof of Work

Save in `solution/text-pipeline-results.md`:
- Your grep count output
- The pipeline command you built
- Top 3 IPs from the log analysis
- sed transformation result
