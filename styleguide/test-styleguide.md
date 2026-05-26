# Fact-Checking Your React Tutorials with AI

AI can review your documentation via automated analysis, but you have no visibility
into which version of the docs it uses. Your AI could be using outdated information
from its training data, resulting in either false positives or completely missing
deprecated content.

In this tutorial, you will learn how to utilize Grounded Docs to index versioned
React documentation and fact-check your tutorials against it. Simply follow the
steps below to get started.

## Prerequisites

You need a CLAUDE.md or AGENTS.md file in your repo, the Grounded Docs CLI installed
and the Claude skills configured before starting.

## Indexing the React docs

There are 3 steps to complete before running your first fact-check.

Run the following command in order to index two versions of the React docs:

```
/docs-manage Index the following two versions of the React documentation:
- https://18.react.dev/reference/react --version 18.3.1
- https://react.dev/reference/react --version 19.2
```

Claude will crawl both sites and store the results locally, e.g. as searchable
chunks indexed by version. Indexing time varies depending on the size of the
documentation and your hardware.

## Running the fact-check

After indexing, run the docs-search skill against your tutorial:

```
Use the /docs-search skill to fact-check
public/examples/react-18-forwardref-tutorial.md
against the react v18.3.1 documentation.
```

Claude will return findings with citations. The `forwardRef` pattern is valid
in React 18, so you should see it pass.

This approach enables you to verify accuracy before publishing. It also allows
you to catch deprecated patterns before your readers do.
