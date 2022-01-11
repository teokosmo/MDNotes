# Bundle size

## webpack-bundle-analyzer

Install the webpack-bundle-analyzer CLI globally
```
npm i -g webpack-bundle-analyzer
```
Build the app
```
ng build --prod --stats-json
```
Analyze the stats.json 
```
webpack-bundle-analyzer ./dist/app/stats.json
```
