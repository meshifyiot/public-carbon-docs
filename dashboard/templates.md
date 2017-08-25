## Node Type Templates

### Gotchas

- The name of the template in the api must match the name of the outside tag of the templates `<tag-name>`
- You can only have 1 list template
- You can have N detail templates

### Basics

You have 2 types of templates available to you: list templates and detail templates.

- List templates are named `nodetypename-list`.
- Detail templates are named `nodetypename-templatename`.

### Syntax

Our front-end development framework is Riot.js. All Templates have the full set of Riot.js lifecycle methods,
syntax, and features. More information can be found here: [riotjs.com](riotjs.com)

### Context

Access to:

- (Lodash)[https://lodash.com/docs/4.17.4]
- (Tachyons)[http://tachyons.io/docs/]
- Riotjs

A few opts have been passed into the tag from the parent. These are:

- opts.node (contains node api model properties, as well as below:)
```
alarms: [1, 2, 3, 4, 5],
channels: {
    temp: {
        value: 100,
        timestamp: "2017-01-01T00:00:01Z"
    },
    bat: {
        value: 50, 
        timestamp: "2017-03-02T20:05:01Z"
    }
}
```

### Available Functions

n/a

### Available Tags (Charting, Sets, etc.)

- [<mf-charts>](/dashboard/chart.md)