skillbag-adapt-skill: Adapt a file or directory into a conforming SkillBag skill and keep SKILLS.md synchronized.
skillbag-create-skill: Create a new skill in a SkillBag repo and keep SKILLS.md synchronized.
skillbag-find-skills-sh: Search skills.sh first when users need a skill, present matches, and adapt selected results into SkillBag skills. #use/skillbag-adapt-skill #use/skillbag-refresh-skill-context
skillbag-log-skills: Make the agent always output the list of skill names used to generate the response. #run/always
skillbag-long-task: Split clearly long-running or context-heavy tasks into response-sized chunks and ask the user to continue.
skillbag-modify-skill: Modify an existing skill in a SkillBag repo while preserving compliance and catalog sync.
skillbag-refresh-skill-context: Re-ingest SkillBag state when SKILLBAG.md or local skill definitions change so the agent uses the latest installed rules and skills. #run/always
skillbag-supervisor: Audit used skills at the end of the task, fix issues when possible, and publish an execution report. #run/always #run/last
