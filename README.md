# Lacework Inline Scanner GitHub Actions Container Demo
GitHub Actions demo for Lacework Inline Scanner
  
### About
This action pulls a PacMan Container and then Scan's it with the Lacework Inline CVE scanner.  
  
### Expected Inputs
It required two inputs as GitHub Secrets:
- secrets.LW_ACCOUNT_NAME
- secrets.LW_ACCESS_TOKEN
  
In your GitHub repo, go into `Settings` -> `Secrets` and create Repository Secrets as follows.  They are encrypted by default.
  
![Secrets](/images/lw_sec.png)

### Integration and Token
In the Lacework UI, se;ect `Settings` -> `Account Settings` -> `Integrations` -> `Container Registries`   
  
Create a `New` and choose a `Registry Type` of `Inline Scanner`.  You cannot use an Agent Token or API Key.
  
![Registration](/images/lw_reg.png)
  
### Lacework Scanner
This version uses a GitHub action that is published to provide the LW Scanner   
`timarenz/lw-scanner-action@main`  
  
You will need to add the following `step` to your GitHub Actions workflow:  
```yml
      - name: Scan container images for vulnerabitilies using Lacework
        uses: timarenz/lw-scanner-action@main
        env:
          LW_ACCOUNT_NAME: ${{ secrets.LW_ACCOUNT_NAME }} 
          LW_ACCESS_TOKEN: ${{ secrets.LW_ACCESS_TOKEN }}
        with:
          image_name: uzyexe/pacman
          image_tag: latest
          fail_if_critical_vulnerabilities_found: true
          fail_only_if_vulnerabilities_fixable: true
          save_build_report: true
```
  
### Lacework Vulnerability Report
Once the GitHub action flow is complete, you will see the container scan report in Lacework.  Under `Vulnerabilities` -> `Container` select the `remote_scanner` in the drop-down menu at the top.  
  
![Vuls](/images/lw_vul.png)
