# Personal AI Learning & Career Agent

A personal AI learning assistant built with Claude Project to help me identify learning gaps, decide what to study next, and practice technical concepts using my own AI/ML project material.

This project was built as part of the FlyRank General AI Fluency internship. The first version focuses on one practical goal: making AI useful for learning from real project work instead of relying only on general questions and answers.

## Table of Contents

- [Overview](#overview)
- [What It Does](#what-it-does)
- [Who It Is For](#who-it-is-for)
- [How It Was Built](#how-it-was-built)
- [Architecture](#architecture)
- [Connected Data](#connected-data)
- [Setup and Access](#setup-and-access)
- [How to Use It](#how-to-use-it)
- [Example Prompts](#example-prompts)
- [Evaluation and V2 Improvements](#evaluation-and-v2-improvements)
- [What I Learned From Testing](#what-i-learned-from-testing)
- [Limitations](#limitations)
- [Future Improvements](#future-improvements)
- [Working Agent](#working-agent)
- [Demo Video](#demo-video)
- [Project Evidence](#project-evidence)
- [Author](#author)

## Overview

I built this agent to help me learn from my own projects and prepare for future technical interviews.

Instead of asking an AI assistant only general questions, I wanted a tool that could review my project documentation, identify gaps in my understanding, recommend relevant topics to study, and generate practice questions based on the work I have actually done.

The agent is currently implemented as a Claude Project. It uses one connected project README alongside general AI/ML knowledge to provide learning support.

The project is an MVP rather than a standalone software application with its own backend.

## What It Does

The agent is designed to help me:

- Review my existing project documentation.
- Identify possible gaps in my knowledge or project documentation.
- Recommend specific AI/ML topics to study next.
- Generate practice and technical interview questions.
- Connect machine learning concepts to my project workflow.
- Explain concepts in a way that supports further learning.
- Identify information that is missing from the available source material.
- Avoid inventing project-specific results that are not documented.

The goal is to support my learning, not to replace independent study or make career decisions for me.

## Who It Is For

This project is primarily designed for individual AI/ML learners who want to use their own projects as learning material.

It can be useful for learners who want to understand the concepts behind their projects, identify areas for improvement, prepare for technical interviews, and discover gaps in their documentation.

## How It Was Built

I built the first version using **Claude Project** rather than developing a separate application or backend.

I chose this approach to keep the initial version simple and focus on testing the core learning workflow before adding more integrations.

The agent's instructions emphasize using the available project material, identifying missing information, distinguishing source-based facts from general knowledge, and avoiding unsupported project-specific claims.

The project does not currently use a custom API, database, or automated file-modification system.

## Architecture

The current workflow can be represented as follows:
                 USER
                   |
                   v
    +-----------------------------+
    | Personal AI Learning &      |
    | Career Agent                |
    | (Claude Project)            |
    +-----------------------------+
                   |
          +--------+--------+
          |                 |
          v                 v
    Connected Project   General AI/ML
       README             Knowledge
          |                 |
          +--------+--------+
                   |
                   v
           Learning Support
                   |
       +-----------+-----------+
       |           |           |
       v           v           v
   Find Gaps   Study Topics   Practice
                             Questions


The connected README provides the project-specific context. General AI/ML knowledge helps explain concepts and suggest learning activities. When a requested project-specific fact is not available in the connected material, the intended behavior is to acknowledge the gap rather than invent a result.

## Connected Data

The first version uses one real project document:

**House Price Prediction — ML Project README**

The document describes aspects of the project, including:

- The machine learning workflow.
- Python and scikit-learn.
- Model training and saving.
- Flask API deployment.
- Project structure and limitations.

I deliberately started with one project document to test the basic workflow before connecting more sources.

My other AI/ML projects, completed assignments, learning notes, and career documents are not currently connected to this agent.

## Setup and Access

The agent is hosted as a Claude Project, so users do not need to install a separate application or configure a local Python environment to try the current version.

To access and use it:

1. Open the [Personal AI Learning & Career Agent](https://claude.ai/share/c655926b-1c00-47a5-af25-3b4279f7f4d5).
2. Sign in to Claude if prompted.
3. Open the shared project or conversation available through the link.
4. Try one of the example prompts in this README.
5. Review the response and check whether the answer is supported by the available project material.

**Important:** The current agent depends on the project material available through the shared Claude setup. Access to the shared link does not necessarily give another user permission to edit or duplicate the original Claude Project. If the shared link does not expose the working project or its connected source, the user may need to recreate the Claude Project, add the House Price Prediction README, and configure the agent instructions.

No separate backend deployment is required for the current MVP.

## How to Use It

The basic workflow is:

1. Open the agent.
2. Ask it to review the available project material.
3. Ask it to identify one learning gap or missing piece of information.
4. Request a specific topic to study next.
5. Generate practice or interview questions.
6. Ask it to connect a machine learning concept to the project.
7. Check whether project-specific claims are supported by the connected source.

### Example Prompts

**Identify a learning gap**

> Review my project material and identify one important learning gap. Explain why it matters.

**Recommend the next topic**

> Based on the learning gap you identified, recommend one specific topic I should study next.

**Prepare for an interview**

> Generate five technical interview questions based on my House Price Prediction project. Include a short explanation of what each question tests.

**Connect ML concepts to the project**

> Explain the machine learning concepts used in this project and connect them to the workflow described in the README.

**Find missing documentation**

> What important information about model evaluation is missing from my project material?

**Check whether a metric is available**

> Can you determine the exact test-set R² score from the connected README? If not, explain what information is missing.

For the final prompt, the agent should not invent an R² value when that value is not documented in the connected source.

## Evaluation and V2 Improvements

I evaluated the agent using six practical test cases developed from my earlier FL-06 work.

The tests focused on the following behaviors:

| Test | What it checks |
|---|---|
| Learning-gap identification | Whether the agent can identify a useful learning gap from the project material. |
| Next-topic recommendation | Whether it can suggest a relevant topic to study. |
| Practice-question generation | Whether it can create questions related to the learner's work. |
| Project connection | Whether it can connect ML concepts to the actual project. |
| Missing-information handling | Whether it acknowledges when a requested answer is not available. |
| Source separation | Whether it distinguishes project-specific information from general knowledge. |

**Reported evaluation result: 6/6 tests passed.**

This result refers to the six practical tests conducted during the initial evaluation. It is not a standardized benchmark or a statistical measure of model accuracy.

### Example: Missing Model Evaluation Metrics

One important test concerned the evaluation of the House Price Prediction model.

The connected README did not contain actual test-set metrics such as R², mean absolute error (MAE), or root mean squared error (RMSE).

When asked for the exact test-set R², the agent correctly stated that the value could not be determined from the available README. It explained that additional evaluation information would be needed rather than creating a numerical result.

This test was useful because it checked whether the agent could recognize a documentation gap instead of presenting an unsupported answer as a fact.

### V2 Evaluation Status

The six tests above document the practical evaluation of the agent. A separate, independently reproducible v2 benchmark with a larger test set, predefined scoring rubric, and recorded results for every test has not been established in this version.

The next evaluation stage would formalize those tests and compare future versions against the same criteria. I would not claim a measured improvement over v1 without that comparison.

## What I Learned From Testing

Testing showed why missing-information handling matters in a project-based learning assistant.

I wanted the agent to follow four principles:

- Use the connected project material when it contains relevant information.
- Explain concepts using general knowledge when appropriate.
- Make it clear when a requested fact is not available in the source.
- Avoid inventing project-specific metrics, results, or implementation details.

The model evaluation example demonstrated how the agent could turn missing documentation into a learning recommendation. Instead of guessing an R² score, it identified regression evaluation and validation as topics worth studying.

This behavior makes the assistant more useful as a learning tool because it can help identify what still needs to be understood, measured, or documented.

## Limitations

The current version is an MVP with several limitations:

- Only one project README is connected.
- Other AI/ML projects, notes, and assignments are not connected.
- Google Drive was part of the original plan but was not connected in this version.
- The agent has not been evaluated on a large or standardized benchmark.
- The six practical tests provide limited evidence about reliability across different projects and questions.
- Project-specific answers depend on the completeness and accuracy of the available source material.
- The connected README does not provide actual R², MAE, or RMSE results for the House Price Prediction model.
- The agent does not automatically modify project files.
- It does not send emails or apply for jobs.
- It does not make career decisions or perform external actions on my behalf.
- Shared-project access and source availability may vary depending on Claude's sharing and account permissions.

The six successful practical tests should therefore be treated as an initial evaluation, not proof that the agent will always produce correct answers.

## Future Improvements

If I continue developing the project, I would like to:

1. Connect more AI/ML projects, notes, and completed assignments.
2. Add structured learning resources and relevant technical documentation.
3. Expand the test set with more difficult and varied questions.
4. Record expected behavior and pass/fail evidence for every evaluation case.
5. Compare future versions against the same test cases.
6. Generate more targeted interview questions based on demonstrated learning gaps.
7. Track recurring learning gaps across multiple projects.
8. Improve the way the agent cites or identifies the source of project-specific claims.

These changes would help move the agent from a small personal prototype toward a more useful and consistently evaluated learning assistant.

## Transparency

This project was built as part of the FlyRank General AI Fluency internship.

Claude Project was used to build and test the assistant. The evaluation was based on practical prompts using my own project material as the connected source.

The reported 6/6 result reflects those six practical tests only. It should not be interpreted as a guarantee of correctness or as a formal benchmark score.

## Working Agent

[Open the Personal AI Learning & Career Agent in Claude](https://claude.ai/share/c655926b-1c00-47a5-af25-3b4279f7f4d5)

A Claude login may be required. The shared link may provide access to a conversation rather than full editing access to the original project.

## Demo Video

**Demo recording:** [Watch the Personal AI Learning & Career Agent demo](https://youtu.be/BkfK29Gx52M)

The video is intended to demonstrate a live end-to-end run of the agent, including:

- Reviewing the connected project material.
- Identifying a learning gap.
- Recommending a topic to study next.
- Generating interview questions.
- Handling a request for model evaluation information that is missing from the README.

The final submission recording should be between three and five minutes, include voice narration, and show the actual agent rather than slides. It should also explain one design decision and one limitation on camera.

## Project Evidence

The project evidence includes:

- A working Claude Project agent.
- A connected House Price Prediction project README.
- Six practical evaluation test cases.
- A demo video link.
- A build log documenting design decisions, testing, and limitations.

**Repository location:** `work/personal_ai_learning_agent/README.md`

## Author

**Faran Khan**

Built as part of the FlyRank AI Internship Program — General AI Fluency Track.
