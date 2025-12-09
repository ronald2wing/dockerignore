# .dockerignore Templates

A comprehensive collection of `.dockerignore` templates for various programming languages, frameworks, and tools. This repository provides ready-to-use templates to help you optimize your Docker builds by excluding unnecessary files from the build context.

## Table of Contents

- [What is a .dockerignore file?](#what-is-a-dockerignore-file)
- [Why Use .dockerignore?](#why-use-dockerignore)
- [Installation](#installation)
- [Usage](#usage)
- [Best Practices](#best-practices)
- [Common Issues and Troubleshooting](#common-issues-and-troubleshooting)
- [Template Structure](#template-structure)
- [FAQ](#faq)
- [Contributing](#contributing)
- [Project Overview](#project-overview)
- [Related Projects](#related-projects)
- [License](#license)
- [Acknowledgments](#acknowledgments)
- [Support](#support)

## What is a .dockerignore file?

A `.dockerignore` file is similar to a `.gitignore` file but is used by Docker during the build process. It specifies which files and directories should be excluded from the Docker build context, which can significantly reduce build time and image size.

## Why Use .dockerignore?

When building Docker images, the entire build context (all files in the directory) is sent to the Docker daemon. This can be slow and inefficient, especially when your project contains:

- Build artifacts and cache directories
- Development dependencies (`node_modules/`, `vendor/`)
- IDE configuration and OS metadata
- Log files and temporary files
- Sensitive environment files
- Test files and coverage reports

Using a `.dockerignore` file helps you exclude these unnecessary files, resulting in:

- **Better caching** - More efficient layer caching by excluding volatile files
- **Faster build times** - Smaller context means less data to transfer
- **Improved security** - Sensitive files never reach the build context
- **Smaller image sizes** - No unnecessary files included in final layers

## Installation

### Quick Download

Download templates directly using curl:

```bash
# For a Node.js project
curl -o .dockerignore https://raw.githubusercontent.com/ronald2wing/dockerignore/master/languages/Node.dockerignore

# For a Python project
curl -o .dockerignore https://raw.githubusercontent.com/ronald2wing/dockerignore/master/languages/Python.dockerignore

# For a React project
curl -o .dockerignore https://raw.githubusercontent.com/ronald2wing/dockerignore/master/frameworks/React.dockerignore

# For a Spring Boot project
curl -o .dockerignore https://raw.githubusercontent.com/ronald2wing/dockerignore/master/frameworks/SpringBoot.dockerignore
```

### Alternative Methods

You can also clone the repository or use it as a submodule:

```bash
# Clone the repository
git clone https://github.com/ronald2wing/dockerignore.git

# Or use as a submodule
git submodule add https://github.com/ronald2wing/dockerignore.git
```

## Usage

### Basic Usage

1. **Choose the appropriate template** from the `languages/`, `frameworks/`, or `tools/` directories
2. **Copy the contents** into your project's `.dockerignore` file
3. **Customize as needed** for your specific project requirements

### Combining Templates

You can combine multiple templates when your project uses multiple technologies:

```bash
# Combine framework with language template
cat frameworks/React.dockerignore languages/Node.dockerignore > .dockerignore

# Combine global patterns with language-specific patterns
cat tools/Global.dockerignore languages/Node.dockerignore > .dockerignore

# Or combine multiple language templates
cat languages/Python.dockerignore languages/Node.dockerignore > .dockerignore
```

## Best Practices

### 1. Customize for Your Project

Add project-specific exclusions for:

- Build artifacts and temporary files
- Database dumps and backups
- Large media files
- Sensitive files (API keys, certificates, credentials)

### 2. Test and Validate

Ensure your `.dockerignore` file works correctly:

- Run `docker build --no-cache` to see what's being sent to the daemon
- Verify essential files are not accidentally excluded
- Check build outputs are correct

### 3. Maintain and Update

Keep your `.dockerignore` file current:

- Review periodically as your project evolves
- Add patterns for new tools and dependencies
- Remove patterns for tools no longer used
- Document patterns with comments for future maintainers

### 4. Pattern Ordering

Place more specific patterns before general ones. Docker processes patterns in order, and the first matching pattern wins.

### Advanced Usage

```dockerignore
# Exclude all .env files
.env*

# Exclude IDE files - developer-specific
.idea/
.vscode/

# Exclude test files - not needed in production
**/test/
**/tests/

# Keep specific files that would normally be excluded
!src/config/important-config.json
!package-lock.json  # For reproducible builds
```

## Common Issues and Troubleshooting

### Issue: Build context is still large

**Solution**: Check for large directories that might not be excluded. Use `docker build --no-cache` to see what's being sent to the daemon. Common culprits include:

- Backup files
- Database dumps
- Development tools and SDKs
- Large media files (images, videos)

### Issue: Docker build is slow despite .dockerignore

**Solution**: Check if you're using `.dockerignore` correctly with Docker BuildKit enabled:

```bash
# Enable BuildKit for better performance
DOCKER_BUILDKIT=1 docker build .
```

### Issue: Patterns aren't working as expected

**Solution**: Remember that patterns are evaluated in order. More specific patterns should come before general ones. Also, ensure you're using the correct pattern syntax:

- Patterns are case-sensitive on Linux/macOS
- Use `*` for wildcards within a single directory level
- Use `**/` to match directories at any level

### Issue: Patterns with special characters

**Solution**: Escape special characters with backslashes or use quotes:

```dockerignore
# Exclude files with brackets
file\[1\].txt

# or
"file[1].txt"
```

### Issue: Required files are being excluded

**Solution**: Use the `!` operator to include specific files that would otherwise be excluded:

```dockerignore
# Keep specific files that would normally be excluded
!src/config/important-config.json
!package-lock.json  # For reproducible builds
```

## Template Structure

Each template follows a consistent structure for easy maintenance:

1. **Header**: `# Created by https://dockerignore.com/`
2. **Language/Framework-specific patterns**: Organized by category
3. **Common patterns**: Shared across many templates
4. **Comments**: Explanations for complex patterns

Templates typically exclude common categories like development environment files, build artifacts, runtime files, testing outputs, and security-sensitive files.

### Using Wildcards and Patterns

Dockerignore supports pattern matching similar to `.gitignore`:

- `!` - negates a pattern (includes files that would be excluded)
- `#` - comment line
- `*` - matches any sequence of non-separator characters
- `**` - matches any number of directories (including zero)
- `?` - matches any single non-separator character

## FAQ

### Q: Can I have multiple .dockerignore files?

A: No, Docker only recognizes one `.dockerignore` file in the build context root.
However, you can combine multiple templates into one file using the `cat` command
as shown in the examples.

### Q: Can I use .gitignore as .dockerignore?

A: While similar, `.gitignore` and `.dockerignore` serve different purposes.
Some patterns may overlap, but `.dockerignore` should be more comprehensive
for build-time exclusions. `.dockerignore` should exclude more files like
build artifacts, dependencies, and temporary files that aren't typically
committed to git but are present during development.

### Q: Can I use environment variables in .dockerignore?

A: No, `.dockerignore` doesn't support environment variable expansion. It's a
static file that's processed before any build arguments or environment variables
are evaluated.

### Q: How do I handle files that need to be excluded in development but included in production?

A: Use conditional logic in your Dockerfile or create separate `.dockerignore`
files for different environments. You can also use build arguments to control
what gets copied.

### Q: Should I exclude package-lock.json or yarn.lock?

A: Generally yes, as these are used during development but the actual
dependencies are installed from package.json during `npm install` or
`yarn install` in the Dockerfile. However, if you want reproducible builds,
you might want to include lock files and use `npm ci` instead of `npm install`.

### Q: What about .dockerignore in subdirectories?

A: Docker only looks for `.dockerignore` in the build context root (the directory
specified in the `docker build` command). Patterns in `.dockerignore` apply to
all files and directories in the build context, regardless of their depth.

### Q: What about Docker multi-stage builds?

A: `.dockerignore` applies to the initial build context sent to the Docker daemon.
In multi-stage builds, it helps reduce the initial context size. Note that
files copied between stages using `COPY --from` are not affected by `.dockerignore`
since they come from previous build stages, not the build context.

## Contributing

We welcome contributions! Please see our comprehensive [CONTRIBUTING.md](CONTRIBUTING.md) guide for detailed instructions on:

- Code of conduct and community guidelines
- How to contribute new templates or improve existing ones
- Submission and review process
- Template guidelines and quality standards
- Testing requirements and methodology

For complete contribution guidelines, please refer to [CONTRIBUTING.md](CONTRIBUTING.md).

## Project Overview

**Total Templates:** 38
**Language Templates:** 9 (Elixir, Go, Java, Node.js, PHP, Python, Ruby, Rust, TypeScript)
**Framework Templates:** 19 (Angular, Django, ExpressJS, FastAPI, Flask, Gatsby, Laravel, NestJS, NextJS, NuxtJS, Phoenix, Qwik, Rails, React, Remix, SolidJS, Spring Boot, Svelte, Vue)
**Tool Templates:** 10 (AndroidStudio, Docker, Git, Global, IntelliJ, PyCharm, SublimeText, Vim, VSCode, WebStorm)
**License:** MIT License
**Repository:** <https://github.com/ronald2wing/dockerignore>

## Related Projects

- [Awesome Docker](https://github.com/veggiemonk/awesome-docker) - A curated list of Docker resources
- [Docker Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) - Official Docker best practices
- [Docker documentation on .dockerignore](https://docs.docker.com/engine/reference/builder/#dockerignore-file)
- [GitHub gitignore templates](https://github.com/github/gitignore)

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md)
file for details.

## Acknowledgments

- All contributors who have helped improve these templates
- Community contributions from developers worldwide
- The Docker community for best practices and patterns

## Support

If you find this project helpful, please consider:

- Contributing new templates or improvements
- Reporting issues or suggesting enhancements
- Sharing with your team or community
- Starring the repository
