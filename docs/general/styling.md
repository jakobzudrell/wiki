---
title: Styling
summary: A few words on styling guidelines for this wiki
authors:
    - Jakob Zudrell
date: 2023-11-07
---
# Styling
To keep things consistent and looking good, we need to define a few rules.

# Themes
The site is built to support a dark and a light theme. Therefore all resources on the page should be able to be displayed in both formats.
This is generally no problem, as CSS styling takes care of font and background colors. However the situation is different for diagrams and images.

In a best case scenario, diagrams are inserted using SVG and providing two different sources; one for the light theme, one for the dark theme.

# Colors
## Dark Theme (default)
```
Background: #1C1B1F
Foreground (typesetting): #E6E1E5

Accent color: #D0BCFF
Code background color: rgba(208, 188, 255, 0.05)
```

## Light Theme
```
Background: #FFFBFE
Foreground (typesetting): #1C1B1F

Accent color: #6750A4
Code background color: rgba(103, 80, 164, 0.05)
```