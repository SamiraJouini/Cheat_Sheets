### DataScientest Linux/Bash Exam Summary

#### **Mandatory Bash Section**
1. **Setup API**  
   - Download & extract API archive:  
     ```bash
     wget --no-cache https://dst-de.s3.eu-west-3.amazonaws.com/bash_fr/api.tar
     tar -xvf api.tar
     ```
   - Run API:  
     ```bash
     chmod +x api && ./api &  # Runs on port 5000
     ```

2. **Install cURL**  
```bash
   sudo apt-get update && sudo apt-get install curl
```

3. **Script Requirements**

- Create folder `exam_NAME/exam_bash` and script `exam.sh` with:
```#!/bin/bash
echo "$(date)" >> sales.txt
# Add loop to fetch GPU sales via curl (e.g., curl "http://0.0.0.0:5000/rtx3060")
```

- **Constraints**: Use a function + loop (e.g., `for`/`while`).
- Output format (`sales.txt`):
```
[Timestamp]
rtx3060:[sales]
rtx3070:[sales]
...
```

4. **Cron Job**
- Schedule script to run **every minute from 7 AM to 9 PM, March/June/November, Mon-Fri**:
```#!/bin/bash
* 7-21 * 3,6,11 1-5 /path/to/exam.sh
```
- Save cron entry to `cron.txt`.
---
**Optional JQ Section**
1. **Setup**
- Download JSON file:
```#!/bin/bash
wget https://dst-de.s3.eu-west-3.amazonaws.com/bash_fr/people.json
```
- Create `exam_jq.sh` and `res_jq.txt` with formatted responses. 

2. **Questions**
- **Q1**: Count attributes per document + display first 12 lines.
- **Q2**: Count `"unknown"` values for `birth_year` using `tail`.
- **Q3**: Format creation dates as `YYYY-MM-DD` (first 10 lines).
- **Q4**: Find pairs of characters with identical birth times.
- **Q5**: List first movie ID + character name (first 10 lines).
- **Bonus**: Clean data (´people_6.json` to `people_9.txt`):
    - Delete invalid `height` entries.
    - Convert `height` to number.
    - Filter heights between 156-171.
    - Output shortest character’s details.
---
#### **Final Submission**
1. **Folder Structure**
```
exam_NAME/
├── exam_bash/
│   ├── exam.sh
│   ├── sales.txt
│   └── cron.txt
└── exam_jq/
    ├── exam_jq.sh
    ├── res_jq.txt
    └── bonus/ (people_6.json, people_9.txt, etc.)
```
2. **Create Archive**
```#!/bin/bash
tar -cvf exam_NAME.tar exam_NAME
```
3. **Transfer via SCP**
```#!/bin/bash
scp -i "data_enginering_machine.pem" ubuntu@YOUR_IP:~/exam_NAME.tar .
```
---
- Key Tools: `curl`, `cron`, `jq`, `tar`, `scp`.
- Constraints: Scripts must be executable (`chmod +x`).
- Submission: Upload `.tar` archive to the platform.
