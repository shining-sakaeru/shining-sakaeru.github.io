# 1. OBJECTIVE
* Migrate existing Jekyll blog content into a Docker-based Ghost CMS, generate a static version of the site using `wget`, and deploy the static output to the GitHub Pages repository (`https://github.com/shining-sakaeru/shining-sakaeru.github.io.git`).
* Ensure zero-trust iterative verification across all steps.

# 2. CONTEXT SUMMARY
* Repository: `shining-sakaeru/shining-sakaeru.github.io` (currently containing existing Jekyll static files).
* Key Tools & Stack: Docker, Docker Compose, Node.js / npm (`@tryghost/migrate`), Wget, Git.
* Constraints & Requirements: Step-by-step verification, proper handling of Docker daemon in container sandbox, and authentication handling for GitHub push (`main` branch).

# 3. APPROACH OVERVIEW
* Execute the migration and deployment pipeline sequentially across 5 defined steps, verifying each step's output before proceeding.
* Utilize Docker Compose for Ghost CMS, official migration tools for Jekyll-to-Ghost content conversion, `wget` mirroring for static site generation, and Git for deployment to GitHub Pages.

# 4. IMPLEMENTATION STEPS
* Step 1: Environment & Dependency Verification
  - Verify versions of Docker, Docker Compose, Git, Wget, Node.js, and npm.
  - Install or configure missing components as needed.
* Step 2: Ghost CMS Docker Setup & Theme Application
  - Create `~/ghost-docker` directory and set up `docker-compose.yml` for Ghost (`ghost:5-alpine`).
  - Start container with `docker-compose up -d` and verify health check at `http://localhost:2368`.
* Step 3: Jekyll to Ghost Migration
  - Install `@tryghost/migrate` via npm.
  - Locate Jekyll markdown posts (`_posts`) and execute migration to generate Ghost-compatible JSON import data.
* Step 4: Static Generation via Wget
  - Create `docs` directory inside `~/ghost-docker`.
  - Run `wget -r -nH -P docs -E -T 2 -np -k http://localhost:2368` to scrape the running Ghost site into static HTML/assets.
* Step 5: GitHub Pages Repository Push
  - Initialize Git in the `docs` directory, configure user credentials, commit static files.
  - Add remote origin (`https://github.com/shining-sakaeru/shining-sakaeru.github.io.git`), set branch to `main`, and push changes (prompting user for PAT if authentication is required).

# 5. TESTING AND VALIDATION
* Ghost container is active and responding locally at `http://localhost:2368`.
* Jekyll markdown content is successfully converted to Ghost import format.
* The `docs/` folder contains valid static HTML, CSS, JS, and asset files.
* Remote GitHub repository (`https://github.com/shining-sakaeru/shining-sakaeru.github.io.git`) is successfully updated on the `main` branch with the new static site output.
