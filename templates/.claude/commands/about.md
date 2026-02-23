Generate the "About This Project" and "Project Context" sections for this project.

**Usage:**
- `/about` — infers content from project files
- `/about <hint>` — uses the hint to supplement inference

## Instructions

1. Read the following to understand the project (skip any that do not exist):
   - `README.md`
   - Any package manifest at the root: `package.json`, `pyproject.toml`, `Gemfile`, `Cargo.toml`, `go.mod`, `composer.json`
   - Any version or tooling config: `.ruby-version`, `.node-version`, `.python-version`, `.tool-versions`
   - Any deployment config: `Dockerfile`, `docker-compose.yml`, `Procfile`, `fly.toml`, `render.yaml`, `vercel.json`
   - The top-level directory listing

2. If `$ARGUMENTS` is provided, treat it as additional context about the project.

3. Draft two to three sentences for "About This Project" that answer:
   - What the project is
   - Who uses it
   - What problem it solves

   Include important technical details where they help characterise the project. Be factual and specific. Avoid marketing language and filler phrases.

4. Draft values for the "Project Context" section:
   - **Stack:** language and framework
   - **Language version:** from manifest or version file
   - **Framework version:** from manifest or lockfile
   - **Key dependencies:** notable libraries (not an exhaustive list)
   - **Test types:** testing frameworks and test types in use (e.g. unit, integration, request/API, e2e) — infer from manifests and test config files (`.rspec`, `jest.config.*`, `vitest.config.*`, `pytest.ini`, `cypress.config.*`)
   - **Architecture notes:** monolith, API-only, microservices, etc.
   - **Deployment:** hosting or deployment method

   Leave any field as its current placeholder if the information cannot be determined from project files.

5. Present both proposed sections and ask for confirmation before writing anything.

6. On confirmation, write the content to `.claude/project.md`:
   - If the file does not exist, create it with both sections.
   - If the file exists, replace the "About This Project" placeholder comment if it is still present, and fill in any "Project Context" fields that are still placeholders. Do not overwrite fields that have already been filled in.

   The "About This Project" placeholder is:
   ```
   <!-- Replace this comment with a short description of what this project is,
        who uses it, and what problem it solves. Keep it to two or three sentences. -->
   ```

7. Report whether the file was created or updated, and which fields were changed.
