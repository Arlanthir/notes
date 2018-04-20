# Protractor

## Installing

```bash
npm install -g protractor
webdriver-manager update --ie
```

### Troubleshooting

If IE generates error:  
`SessionNotCreatedError: Unexpected error launching Internet Explorer.
Protected Mode settings are not the same for all zones.
Enable Protected Mode must be set to the same value (enabled or disabled) for all zones.`

1. Open IE
2. Go to Tools -> Internet Options -> Security
3. Set all zones (Internet, Local intranet, Trusted sites, Restricted sites) to the same protected mode,
   enabled or disabled should not matter.
