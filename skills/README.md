# draw.io skills initial prompts.

## Download AWS Icons

[AWS Architecture Icons](https://aws.amazon.com/architecture/icons/)

## Install skill-creator skill

Install from Anthropic marketplace:

Claude Desktop -> Customize -> Skills -> + -> Browse skills

## Create Skill

Claude Desktop -> Customize -> Skills -> + -> Create skill -> Create with Claude

## Using Create with Claude

--> Attach the AWS Architecture Icon Guideline PDF and the Icon zip files <--

Let's create a skill together using your skill-creator skill. First ask me what the skill should do.

Create a skill that can generate draw.io chart. Draw.io is the application from https://www.drawio.com/ . 
This skill is to create draw.io file. When a flowchart creation is needed, using draw.io format to output.
The name of this skill is "drawio".

Please use pre-defined shapes / professional icons, especially Amazon AWS. The attached files contain almost all Amazon AWS icons . 
Another requirement: For the standard / basic / general shapes, e.g. rectangle, rounded rectangle, decision diamond, do not fill-in with color. That looks not very professional. Leave color to non basic shapes, e.g. AWS icons, or some special meaning shape, e.g. mobile phone.

## The following questions were from the 'skill-creator' skill, we can paste in as part of the prompts. 

```text
Q: types of diagrams should this skill cover? 
A: Flowcharts / process flows, System architecture diagrams, Entity-relationship (ER) diagrams

Q: What should the output be?
A: A .drawio file the user can download

Q: Should the skill also include a rendered preview (e.g. SVG inline) alongside the draw.io file?
A: No preview is needed. Please output only .drawio XML format.

Q: Should the skill use a consistent visual style/theme across diagrams?
A: Yes, a clean modern style (rounded shapes, soft colors)

Q: How should the skill handle ambiguous requests — e.g. user says 'draw a diagram of my auth flow' with no further detail?
A: Ask clarifying questions first
```

