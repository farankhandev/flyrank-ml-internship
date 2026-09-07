# Personal AI Learning & Career Agent

A personal AI assistant built with Claude Project to help me identify learning gaps, decide what to study next, and practice concepts based on my actual projects and learning material.

The first version was built as part of the FlyRank General AI Fluency internship.

## What it does

The agent is designed to support my AI/ML learning and career development.

It can:

* Review my project and learning material
* Identify areas where information or understanding may be missing
* Suggest specific topics to study next
* Generate practice and interview-style questions
* Connect general AI/ML concepts to my actual project work
* Clearly distinguish between information found in my project material and general knowledge
* Admit when the available material does not contain enough information to answer a question

The goal is not to replace learning or make decisions for me. It acts as a learning and career support tool.

## Who it is for

This version is primarily designed for an individual AI/ML learner who wants an assistant that understands their own projects and can turn those materials into targeted learning recommendations and practice.

## How it was built

The first version was built using **Claude Project** rather than developing a separate application or backend.

This kept the MVP small and allowed me to test the core workflow before adding more integrations.

### Architecture

                    ┌─────────────────────────┐
                    │   Personal AI Learning   │
                    │    & Career Assistant    │
                    └────────────┬────────────┘
                                 │
                    ┌────────────▼────────────┐
                    │      Claude Project      │
                    │   Agent instructions +  │
                    │       reasoning          │
                    └────────────┬────────────┘
                                 │
                    ┌────────────▼────────────┐
                    │   Connected project     │
                    │        material         │
                    │                         │
                    │  ML Project README      │
                    └────────────┬────────────┘
                                 │
             ┌───────────────────┼──────────────────┐
             │                   │                  │
             ▼                   ▼                  ▼
       Find gaps          Recommend learning    Generate practice
             │                   │                  │
             └───────────────────┼──────────────────┘
                                 ▼
                         Human learning /
                         career decision

## Connected data

The first version uses one real project document as its connected source:

**ML Project README**

The README contains information about my House Price Prediction project, including:

* Machine learning workflow
* scikit-learn
* Model training
* Model saving
* Flask API deployment
* Project limitations

I deliberately started with one real project file instead of connecting everything at once. The goal was to prove that the basic learning-assistant workflow worked before expanding the knowledge sources.

## How to use it

1. Open the agent.
2. Ask it to review the available project material.
3. Ask it to identify a learning gap or missing information.
4. Ask what topic should be studied next.
5. Ask it to generate practice or interview questions.
6. Ask it to connect a concept to the project material.
7. When information is missing, ask the agent to explain whether the answer can actually be determined from the available source.

### Example prompts

Review my project material and identify one important learning gap.

What should I study next based on the weaknesses you found?

Generate interview questions based on the areas I need to improve.

Connect the machine learning concepts in my project to the workflow described in the README.

What information is missing from my project material?

Can you determine the exact test-set R² from the available README? If not, explain why.

## Evaluation and testing

I tested the first version using six cases based on the FL-06 agent specification:

1. Identify a learning gap
2. Recommend the next learning topic
3. Generate practice questions
4. Connect ML concepts to my project
5. Check whether the agent admits when information is missing
6. Check whether the agent separates project-specific information from general knowledge

The agent successfully completed all six tests.

One useful example was the model evaluation gap. The agent noticed that model evaluation was mentioned in the project material, but actual metrics such as R², MAE, and RMSE were not provided. It recommended studying regression evaluation and validation rather than pretending those values were available.

I also asked the agent for the exact test-set R². Because the README did not contain that value, the agent correctly said that the exact result could not be determined from the available information and explained what additional information would be required.

The main evaluation result was therefore not a single numerical score. The important behavior I was testing was whether the agent could use the connected project material, identify missing information, and avoid making unsupported claims.

## What I changed during testing

During testing, I focused on making the assistant more reliable when working with incomplete project information.

The main behavior I wanted was:

* Use my project material when it contains the answer.
* Say when information is missing.
* Do not invent project-specific results.
* Clearly separate source-based information from general ML knowledge.

This became especially important when asking for exact model metrics that were not included in the connected README.

## Current limitations

This is an MVP, so it has several limitations.

* Only one real project README is connected in the first version.
* My other AI/ML notes and completed assignments are not connected yet.
* Google Drive was included in the original plan but was not connected in this version.
* The agent does not automatically update my files.
* The agent does not send emails or apply for jobs.
* It does not make external or irreversible decisions on my behalf.
* Its project-specific knowledge is limited to the material provided to the Claude Project.
* The current evaluation is based on a small set of practical tests rather than a large benchmark.

## What I would improve next

The next version could connect more of my actual learning material, including:

* AI/ML notes
* Completed FlyRank assignments
* Additional project documentation
* Career and resume material

I would also expand the evaluation set to test the agent across more learning gaps and different types of project questions.

## Transparency

This project was built as part of the FlyRank General AI Fluency internship.

Claude Project was used to build and test the assistant. The agent's responses were evaluated through practical prompts using my own project material as the connected source.

## Working agent

The working Personal AI Learning & Career Agent is available here:

https://claude.ai/share/c655926b-1c00-47a5-af25-3b4279f7f4d5

A Claude login may be required to access the shared agent.

## Project evidence

The FL-07 build process included:

* A working Claude Project agent
* A connected ML project README
* Six practical evaluation tests
* A raw end-to-end screen recording
* A build log documenting the decisions, testing, and limitations

## Author

**Faran Khan**

AI Engineer / Machine Learning Engineering Intern

Built as part of the FlyRank AI Internship Program.
