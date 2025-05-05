# Objective

In this blog, we are going to demonstrate how to leverage Conversational Agent Playbook function tool, to dynamically control website information display based on real time chat conversation intent.


# Playbook Configuration

Set up a new Function tool in Playbook with the following input/output schemas.

```yaml
# Input
type: object
required:
  - intent
properties:
  intent:
    type: string
    description: Intent for web client rendering.

# Output
null
```

# Setup Playbook

You can import the JSON file `dialogflow_app/exported_agent_test-global.json` to setup Playbook agent.



# Web Client HTML

There are two keys steps:
- Create a function to be called, `getUserMetadata`
- Register the playbook tool by mapping it to the function using `registerClientSideFunction`

```html
<html>
  <head>
   <title>Traveloka Demo</title>
    <link rel="stylesheet" type="text/css" href="/css/style.css">
  </head>
  <body>
    <img src="nature.jpg" alt="background" width="100%" height="100%">
    <link rel="stylesheet" href="https://www.gstatic.com/dialogflow-console/fast/df-messenger/prod/v1/themes/df-messenger-default.css">
    <script src="https://www.gstatic.com/dialogflow-console/fast/df-messenger/prod/v1/df-messenger.js"></script>
    <df-messenger
      project-id="aimc-410006"
      agent-id="fe0baa0c-f221-4ddd-b270-aaaabca8f2d6"
      location="us-central1"
      language-code="en"
      max-query-length="-1"
      allow-feedback="all">
    </df-messenger>

    <script>
      async function getUserMetadata(input) {
        console.log(input)
        const key = input.key
        return Promise.resolve({
          "username": "David",
          "booking_id": "1111"
        })
      }

      // Register the tool
      const toolId = "projects/aimc-410006/locations/us-central1/agents/fe0baa0c-f221-4ddd-b270-aaaabca8f2d6/tools/4b753634-a923-4b5d-bf68-108a27aa2bd0"
      const dfMessenger = document.querySelector('df-messenger');
      dfMessenger.registerClientSideFunction(toolId, "get-user-metadata", getUserMetadata)

      window.addEventListener('dfMessengerLoaded', function () {
          var dfMessenger = document.querySelector('df-messenger');

      });
    </script>
  </body>
</html>


```

# Demo

Conversational Agent
- Agent: test-global [link](https://dialogflow.cloud.google.com/v2/projects/hello-world-418507/locations/global/agents/75e7c898-526d-4789-8405-b496df0bf214)
- Project: hellow-world


# FAQ

## What is function toll?
Tools are interfaces that accept input, perform an action, and return structured results according to a predefined schema, often by leveraging external APIs to extend LLM capabilities beyond what the model alone can do.

Tool calling enhances LLMs by enabling access to real-time data, allowing for more accurate and relevant responses, and supporting complex, specialized tasks like advanced calculations or database queries.
