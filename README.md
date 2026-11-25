# blast-radius
Blast Radius is an open-source tool that maps the dependencies in your Salesforce metadata and shows what a change will affect before you deploy it. Point it at an SFDX project and ask, for example, "where is Account.Industry used?" It runs locally or in your CI, so your metadata never leaves your machine.


## Sample data

Blast Radius is developed and tested against the public [apex-recipes](https://github.com/trailheadapps/apex-recipes) sample app. Clone it **outside** this repo, for example next to it:

```
cd ~/Desktop
git clone https://github.com/trailheadapps/apex-recipes.git
```

The metadata to scan is in `apex-recipes/force-app/`. Only public sample apps or your own Developer Edition org metadata should be used here, never client or employer metadata.
