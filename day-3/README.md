# ApplicationSets

ApplicationSets its a way to automatically generate argocd templates (Applications) based on generators (a list of something) to easly create apps based on various sources. For example: "generate a set of Applications looking for helm charts inside a folder called "apps".

ApplicationSets use goTemplate to generalize the template creation based on values from the generators.

The pro of using ApplicationSets is to simplify Application creations, by only worrying to configure the helm/repo correctly, and leaving the Application spec configuration to be auto-generated, not needing to maintain multiple Applications that are configure the same way.

## Generators

The generators are where the ApplicationSet will looking for to generate the Applications, the most common used generators are:

- git
- gitfile
- pull request
