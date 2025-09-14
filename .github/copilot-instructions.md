# Copilot coding agent onboarding instructions

Purpose
- Provide a compact, trusted source-of-truth for an agent that is seeing this repository for the first time.
- Minimize exploratory searches, avoid common CI/build/test failures, and speed up making safe, reviewable PRs.

High-level summary
- This is a very small single-module Java project used to experiment with Copilot Review and PR behaviors.
- Languages / runtimes:
  - Java 17 (project toolchain)
  - Gradle build system (Gradle build file present)
  - Spring Boot (reactive WebFlux starter) used as a dependency
  - A small JavaScript script (test.js) included for simple local checks
- Size & shape: small repository (only a few root files). Single Gradle module.

What you should trust in these instructions
- Follow these steps unless you detect a clear mismatch (e.g., files moved, a workflow added). Only search the repo when the instructions are missing necessary information or fail in practice.

Build & validation (bootstrap → build → test → run → quick lint)
Preconditions (always check)
1. Ensure JDK 17 is available and JAVA_HOME points to it. Build toolchain in build.gradle targets Java 17.
   - Always verify: java -version (should show Java 17)
   - If JAVA_HOME is not set, set it before running Gradle-related commands.
2. Network access is required to download dependencies unless a Gradle cache is available.

Recommended command sequence (reproducible and conservative)
1. If a Gradle wrapper exists (./gradlew), prefer it:
   - ./gradlew --version  (confirm wrapper works)
   - Always run: ./gradlew clean build
2. If no wrapper is present (no ./gradlew), create one or use system gradle:
   - gradle wrapper
   - ./gradlew clean build
   - Or use: gradle clean build
3. To run tests:
   - ./gradlew test
   - Tests are configured to use JUnit platform (see build.gradle)
4. To run the application locally:
   - ./gradlew bootRun
   - If you only want to build and not run tests: ./gradlew build -x test
5. JavaScript exercise (small helper file):
   - node test.js
   - This is optional and independent of the Java build.

Notes about commands and common failure modes
- Missing wrapper: if ./gradlew is not present, generating a wrapper (gradle wrapper) requires Gradle installed locally. Use the wrapper when possible to lock Gradle version.
- JAVA_HOME mismatch: builds will fail if Gradle cannot find a compatible JDK. Ensure Java 17 is used.
- Network/timeouts: first build downloads dependencies from Maven Central; can fail behind proxies or flaky networks. Retry builds; increase Gradle timeout if CI times out.
- If Gradle fails with memory errors, try: ./gradlew build -Dorg.gradle.jvmargs="-Xmx1g"
- If Spring Boot plugin version or dependencies cause incompatibility in your environment, prefer switching to the Gradle wrapper or matching your Gradle version to the plugin's supported versions.

CI / pre-merge checks
- I did not detect a preconfigured GitHub Actions workflow in the repository root. Before assuming CI runs automatically, check .github/workflows.
- The de facto pre-merge validations you should replicate locally before opening a PR:
  1. ./gradlew clean build (green)
  2. ./gradlew test (green)
  3. If applicable, lint/format checks (not present by default in this repo—add if you require them)
- When preparing a PR, include a brief summary of local validation steps you ran in the PR description (e.g., "I ran ./gradlew clean build and ./gradlew test on JDK 17").

Project layout & key files (root)
- Files at repository root (priority order)
  - README.md
  - build.gradle
  - test.js
  - LICENSE
  - .github/  (check for workflows)
- Important file roles:
  - build.gradle — central build configuration, Java toolchain set to Java 17, Spring Boot plugin and dependencies.
  - test.js — a tiny JS script for quick local checks (not part of the Gradle build).
  - README.md — short description and purpose.

Contents (for quick reference)
- README.md
  - # Copilot-agent-review
  - This repository is intended for testing various GitHub features related to Copilot Review in pull requests, as well as experimenting with additional PR configurations.
  - Purpose: Explore and validate Copilot Review functionality, test PR settings and automation workflows, experiment with configuration changes.

- build.gradle (highlights)
  - plugins { id 'java'; id 'org.springframework.boot' version '3.3.3'; id 'io.spring.dependency-management' version '1.1.5' }
  - java toolchain -> languageVersion = JavaLanguageVersion.of(17)
  - dependencies include:
    - implementation 'org.springframework.boot:spring-boot-starter-webflux'
    - implementation 'com.fasterxml.jackson.core:jackson-databind'
    - implementation 'org.springframework.boot:spring-boot-starter-validation'
    - testImplementation 'org.springframework.boot:spring-boot-starter-test'
    - testImplementation 'io.projectreactor:reactor-test'
  - tests configured with JUnit Platform (tasks.named('test') { useJUnitPlatform() })

- test.js
  - function add(a, b) { return a + b; }
  - console.log("Hello, world!");
  - console.log("2 + 3 =", add(2, 3));

Where to make changes (common areas)
- For Java code: add sources under src/main/java and tests under src/test/java (standard Gradle layout). If these directories are not present yet, create them for new code.
- For build rules: modify build.gradle.
- For PR automation: add or update GitHub Actions under .github/workflows/*.yml.

Agent workflow guidance (how you should operate)
1. Trust this file for environment and build steps. Do not randomly run wide repo searches if the requested change is small and the instructions provide the necessary build/test commands.
2. Always start by validating your environment:
   - Confirm Java 17 and the ability to run ./gradlew clean build (or gradle clean build).
3. Run a minimal local validation sequence before making any changes:
   - ./gradlew clean build
   - ./gradlew test
   - node test.js (if working with the JS helper)
4. If you must change build files or add dependencies, run a full build and tests again locally.
5. If build fails with an unfamiliar error, search only the minimal set of files needed (build.gradle, .github/workflows, any newly changed files).
6. Document in the PR description: JDK version, commands run, and any non-obvious workaround used.

If you see unexpected files or a GitHub Actions workflow
- Re-check .github/workflows; if a workflow exists, replicate its steps locally where reasonable.
- If a workflow targets a different JDK or Gradle version, prefer matching the workflow for CI parity.

Extra recommendations for maintainers (helpful but optional)
- Add a Gradle wrapper to the repo (./gradlew + checked-in wrapper files) to ensure consistent Gradle versions across contributors and CI.
- Add a CI workflow (.github/workflows/ci.yml) that runs: Set up JDK 17, ./gradlew clean build, ./gradlew test — so agents and contributors have deterministic CI feedback.
- Consider adding a formatter/linter config (spotless/checkstyle) in build.gradle to avoid style-only PR rejections.

Final note
- Use this file as the authoritative onboarding guide. Only perform additional repository searches when these steps fail or when a requested change cannot be completed using the information here.
