Notetaking Sample Structure

- Attack Path
- Credentials
- Findings
- Vulnerability Scan Research
- Service Enumeration Research
- Web Application Research
  - Aquatone or EyeWitness to screenshot all applications  
- AD Enumeration Research
- OSINT
- Administrative Information
  - Contact information like project managers or points of contact for the engagement
- Scoping Information
- Activity Log
- Payload Log
  - File hashes
 

**Tmux Logging
```shell-session
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
touch .tmux.conf
```

```shell-session
cat .tmux.conf 

# List of plugins

set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
set -g @plugin 'tmux-plugins/tmux-logging'

# Initialize TMUX plugin manager (keep at bottom)
run '~/.tmux/plugins/tpm/tpm'
```

```shell-session
tmux source ~/.tmux.conf
tmux new -s {name}
```

**Install plugin in tmux
```shell-session
[Ctrl] + [B]
[Shift] + [I]
```

**Start logging in tmux
```shell-session
[Ctrl] + [B]
[Shift] + [P]
exit
```

**Split terminals vertically
```shell-session
[Ctrl] + [B] + [Shift] + [%]
```

**Retroactive logging
```shell-session
[Ctrl] + [B]
[Alt] + [Shift] + [P]
```

**Increase history limit
```shell-session
set -g history-limit 50000
```

**Screenshot one pane
```shell-session
[Ctrl] + [B]
[Alt] [P]
```

Other Tmux plugins
- tmux-sessionlist
- tmux-pain-control
- tmux-resurrect


## Artifacts Left Behind
- Use file hashes so that client can clean up certain things
- IP address of the hist(s)/hostname(s) where the change was made
- Timestamp of the change
- Description of the change
- Location on the host(s) where the change was made
- Name of the application or service that was tampered with
- Name of the account (if you created one) and perhaps the password in case you are required to surrender it


## Folder Structure

- Admin
- Deliverables
- Evidence
    - Findings
    - Scans
        - Vulnerability scans
        - Service Enumeration
        - Web
        - AD Enumeration
    - Notes
        - AD Enumeration Research
        - Attack path
        - Findings
        - Administrative Information
        - Scoping Information
        - Activity Log
        - Payload Log
        - OSINT Data
        - Credentials
        - Web Application Research
        - Vulnerability Scan Research
        - Service ENumeration Research 
    - OSINT
    - Wireless
    - Logging output
    - Misc Files
- Retest

```shell-session
mkdir -p ACME-IPT/{Admin,Deliverables,Evidence/{Findings,Scans/{Vuln,Service,Web,'AD Enumeration'},Notes,OSINT,Wireless,'Logging output','Misc Files'},Retest}
```

## Formatting and Redaction
- Credentials
- PII

Tips:
- Draw attention to certain items in screenshots
- Crop to only display important info
- Include address bar of URL
- Black bar


Tools for Redaction:
- Greenshot


