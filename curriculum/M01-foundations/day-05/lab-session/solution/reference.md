# Day 03 — Reference Solution

## Sample Pipeline
```bash
grep ERROR /tmp/sample-data.txt | \
  cut -d' ' -f1,2,4- | \
  sort
```

## Grep Count
```
grep -c ERROR /tmp/sample-data.txt
# Output: 3
```

## Log Analysis Pattern
```bash
# Top IPs
awk '{print $1}' /var/log/apache2/access.log | sort | uniq -c | sort -rn | head -10

# 404 count
grep ' 404 ' /var/log/apache2/access.log | wc -l
```

## sed Substitution
```bash
sed 's/ERROR/❌ ERROR/g' /tmp/sample-data.txt > solution/annotated-log.txt
```
