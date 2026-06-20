# mpcal-calendar-sync

## Deployment (Deployer for Git webhook)

Auto-deploys to production (`myhub21.com` on Cloudron) via the **Deployer for Git (Pro)** plugin, which watches `github.com/fullrealtyservices/mpcal-calendar-sync` on the **`production`** branch.

```bash
git push origin main                 # dev
git push origin main:production      # deploy (a server hook requires main be pushed first)
```

Pushing `production` fires a GitHub webhook to `/wp-json/dfg/v1/package_update?secret=…&type=plugin&package=mpcal-calendar-sync`, which pulls the production branch and installs it live — no manual file copy.

Notes:
- Push auth: use the gh CLI credential helper (`gh auth setup-git`); the macOS `osxkeychain` helper can fail with `Device not configured`.
- The deployer's cache flush does **not** reset PHP opcache, so a *modified* file's change may not appear until the app is restarted (`cloudron restart --app <id>`) + `wp cache flush`. New files load fine.
