# CoreOS Customer Cases Bug Report Generator

Generate a comprehensive weekly report of CoreOS bugs that have Customer Cases linked to them using the jira CLI tool.

## JQL Query
Use this JQL query to retrieve all active CoreOS bugs with customer cases:
```
filter = "CoreOS Bugs" AND "SFDC Cases Counter" > 0 AND issuetype = Bug and Project in (RHEL, COS, OCPBUGS) AND statusCategory != Done
```

## Implementation

### Step 1: Execute JQL Query
```bash
jira issue list -q 'filter = "CoreOS Bugs" AND "SFDC Cases Counter" > 0 AND Project in (RHEL, COS, OCPBUGS) AND statusCategory NOT IN (Done)' --plain --columns key,summary,assignee,created,status,labels
```

### Step 2: Analysis Requirements
1. Weekly New Bugs: Identify bugs created within the current week (Monday-Sunday)
2. Unassigned Bugs: Find all bugs with empty assignee fields
3. Age Categorization: Group bugs by age:
   - Recent: < 1 month old
   - Medium-term: 1-3 months old  
   - Long-term: 3+ months old
4. Assignment Summary: Count bugs by assignee
5. Statistics: Calculate oldest bug age and total counts

#### Special Attention Items:
- CVE-related bugs (flag as security concerns)
- Bugs over 100 days old without assignment
- Multiple new bugs in a single week
- Highlight the issue with rhcos-waitingonrhel label

