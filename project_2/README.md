# Project 2 : Anonymous Chatting System

## Original Prompt

[Check my conversations](https://claude.ai/share/fcbf28f1-63d1-41e3-918d-d046cc2fdbea)

## Prompts for Pencil

```text
Design the user interface for this front-end specifications.
It should work for both mobile and desktop.
```

Attach files:

- 01-system-architecture.md
- 07-frontend-design.md

Choose Styles.

## Prompts for Front-End

```text
please implement @documents/07-frontend-design.md in @webui/ . The design file is in @webui/ui_design.pen. The API document can be found @documents/ . Please be aware that this front-end will be hosted on github and to be loaded via github pages. The root of GitHub Pages will be @webui/ 
```

### Additional Prompts

1. What is the configuration to make webui can be loaded from GitHub pages? How about the GitHub Actions? 
2. Work with Claude Code with images if still not working.

## Prompts for Back-End

```text
Implement three Lambda functions based on the specifications in @documents/ . AWS credential has been loaded. Perform the configuration and Lambda loading process if you can. List all actions for human if that cannot be done by you. The folder structure shall be follow the design in @documents/01-system-architecture.md 
```

## Prompts after Deploymet

```text
[attach image of the result] This is the deployment. Please update the setting in the Front-End.
```
