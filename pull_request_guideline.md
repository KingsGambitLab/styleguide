## Pull Request Guideline

##### How to create an effective pull request which is easier to review, test and get merged quickly? (Applicable for any new feature/product)

There are two phases in the PR lifecycle: Architecture & Implementation.

#### Phase-1: Architecture

In this phase, everyone will **focus on code architecture and design**. Author won’t be implementing anything concrete here. Once the schema & design has been finalized, they will commit the skeleton first.

* Skeleton: Class, Functions,Services which are needed. No need to go into implementation details. This phase is purely code architecture.
* For Backend: Author will create empty classes, function signature and routes only.
* For Frontend: Define components, functions signature.

This phase **has to be reviewed by Frontend or Backend leads** (depends on where the changes have been done). Only if they approve, the author can go into the next phase. Architecture changes are harder to fix and give lots of pain over a long time so we need to be very vigilant here. **Any changes suggested here must be made before moving to the next phase.**

#### Phase-2: Implementation
In this phase, the author can **move into the implementation details**. The author can now implement each function one by one and do commit as frequently as possible.

This phase **should be reviewed by the codeowner or the team members** who have worked on this piece of code earlier. Domain knowledge is required here.

#### [Important] Coding conventions
Coding conventions should be followed in each phase. The author needs to be very careful about that.

In InterviewBit, we follow [Ruby Coding Conventions](https://github.com/KingsGambitLab/styleguide/blob/master/ruby_coding_conventions.md) for our **Backend**.
For **Frontend**, all rules and conventions for frontend can be found on `/docs` when running the server locally. More information for frontend conventions might be found here: [Frontend Coding Conventions](https://github.com/KingsGambitLab/styleguide/frontend_coding_conventions.rb).


### Example: A Todo app based on React

#### Phase-1: Architecture

* Commit empty components like
* Top level App Component
* Todo List Component
* Todo List Item Component
* Any API needed to fetch data

#### Phase-2: Implementation

* Implement API to fetch data from servers
* Actual UI implementation of different components.
