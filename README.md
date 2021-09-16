# lw_container_scanner_demo
GitHub Actions demo for Lacework Scanner
  
### About
This action pulls a PacMan Container and then Scan's it with the Lacework Inline CVE scanner.  
  
### Expected Inputs
It required two inputs as GitHub Secrets:
- secrets.LW_ACCOUNT_NAME
- secrets.LW_ACCESS_TOKEN
  
### Lacework Scanner
This version uses a GitHub action that is published to provide the LW Scanner   
`timarenz/lw-scanner-action@main`  
  
### Lacework Vulnerability Report
Once the GitHub action flow is complete, you will see the container scan report in Lacework.  Under container vulnerabilities select the `lw_scanner` tab.  
  
![Vuls](/images/lw_vul.png)
