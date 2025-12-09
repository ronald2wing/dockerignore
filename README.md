# dockerignore Templates

A comprehensive collection of `.dockerignore` templates for various programming languages, frameworks, and tools. This repository provides ready-to-use templates to help you optimize your Docker builds by excluding unnecessary files from the build context.

## Why Use .dockerignore?

When building Docker images, the entire build context (all files in the directory) is sent to the Docker daemon. This can be slow and inefficient, especially when your project contains:

- Development dependencies (`node_modules/`, `vendor/`)
- Build artifacts and cache directories
- Log files and temporary files
- IDE configuration and OS metadata
- Test files and coverage reports
- Sensitive environment files

Using a `.dockerignore` file helps you exclude these unnecessary files, resulting in:

- **Faster build times** - Smaller context means less data to transfer
- **Smaller image sizes** - No unnecessary files included in final layers
- **Improved security** - Sensitive files never reach the build context
- **Better caching** - More efficient layer caching by excluding volatile files

## Table of Contents

- [What is a .dockerignore file?](#what-is-a-dockerignore-file)
- [Installation](#installation)
- [Usage](#usage)
- [Best Practices](#best-practices)
- [Common Issues and Troubleshooting](#common-issues-and-troubleshooting)
- [Template Structure](#template-structure)
- [FAQ](#faq)
- [Contributing](#contributing)
- [Related Projects](#related-projects)
- [License](#license)
- [Acknowledgments](#acknowledgments)
- [Support](#support)

## What is a .dockerignore file?

A `.dockerignore` file is similar to a `.gitignore` file but is used by Docker during the build process. It specifies which files and directories should be excluded from the Docker build context, which can significantly reduce build time and image size.

## Installation

### Option 1: Manual Download

Browse the repository and copy the appropriate template to your project:

```bash
# For a Node.js project
curl -o .dockerignore https://raw.githubusercontent.com/ronald2wing/dockerignore/master/languages/Node.dockerignore
```

### Option 2: Clone the Repository

```bash
git clone https://github.com/ronald2wing/dockerignore.git
cd dockerignore
```

### Option 3: Using as a Submodule

```bash
git submodule add https://github.com/ronald2wing/dockerignore.git
```

## Usage

### Basic Usage

1. **Browse the appropriate directory** (`languages/`, `frameworks/`, or `tools/`) for the template that matches your project
2. **Copy the contents** into your project's `.dockerignore` file
3. **Customize as needed** for your specific project requirements

### Examples

#### For a Node.js project

```bash
# If you've cloned the repository
cp languages/Node.dockerignore .dockerignore

# Or using curl (without cloning)
curl -o .dockerignore https://raw.githubusercontent.com/ronald2wing/dockerignore/master/languages/Node.dockerignore
```

#### For a Python project

```bash
# If you've cloned the repository
cp languages/Python.dockerignore .dockerignore

# Or using curl (without cloning)
curl -o .dockerignore https://raw.githubusercontent.com/ronald2wing/dockerignore/master/languages/Python.dockerignore
```

#### For a React project

```bash
# If you've cloned the repository
cp frameworks/React.dockerignore .dockerignore

# Or using curl (without cloning)
curl -o .dockerignore https://raw.githubusercontent.com/ronald2wing/dockerignore/master/frameworks/React.dockerignore
```

#### For a Spring Boot project

```bash
# If you've cloned the repository
cp frameworks/SpringBoot.dockerignore .dockerignore

# Or using curl (without cloning)
curl -o .dockerignore https://raw.githubusercontent.com/ronald2wing/dockerignore/master/frameworks/SpringBoot.dockerignore
```

### Combining Templates

You can combine multiple templates by concatenating them. This is useful when your project uses multiple technologies:

```bash
# Combine global patterns with language-specific patterns
cat tools/Global.dockerignore languages/Node.dockerignore > .dockerignore

# Or combine multiple language templates
cat languages/Python.dockerignore languages/Node.dockerignore > .dockerignore

# Combine framework with language template
cat frameworks/React.dockerignore languages/Node.dockerignore > .dockerignore
```

### Advanced Usage

#### Customizing Templates

After copying a template, you can add project-specific exclusions:

```dockerignore
# .dockerignore
# Copied from languages/Node.dockerignore

# Add project-specific exclusions
secrets/
config/local.yaml
*.pem
database-backups/

# Keep specific files that would normally be excluded
!src/config/important-config.json
!package-lock.json  # For reproducible builds
```

#### Using Wildcards and Patterns

Dockerignore supports pattern matching similar to `.gitignore`:

- `*` - matches any sequence of non-separator characters
- `?` - matches any single non-separator character
- `**` - matches any number of directories (including zero)
- `!` - negates a pattern (includes files that would be excluded)
- `#` - comment line

## Best Practices

### 1. Customize for Your Project

Add project-specific exclusions for:

- Sensitive files (API keys, certificates, credentials)
- Build artifacts and temporary files
- Database dumps and backups
- Large media files

### 2. Keep It Updated

Review and update your `.dockerignore` file as your project evolves:

- When adding new dependencies or tools
- When changing build processes
- When new sensitive files are created

### 3. Test Your Builds

Ensure your `.dockerignore` file doesn't exclude necessary files by:

- Running `docker build --no-cache` to see what's being sent
- Checking that essential files are included
- Verifying build outputs are correct

### 4. Order Matters

Place more specific patterns before general ones. Docker processes patterns in order, and the first matching pattern wins.

### 5. Use Comments

Document why certain patterns are included for future maintainers:

```dockerignore
# Exclude test files - not needed in production
**/test/
**/tests/

# Exclude IDE files - developer-specific
.idea/
.vscode/
```

### 6. Review Periodically

As your project grows, review the `.dockerignore` to ensure it's still relevant:

- Remove patterns for tools no longer used
- Add patterns for new tools and dependencies
- Ensure security-sensitive files are still excluded

## Common Issues and Troubleshooting

### Issue: Required files are being excluded

**Solution**: Use the `!` operator to include specific files that would otherwise be excluded:

```dockerignore
# Exclude all .env files
.env*

# But include .env.example
!.env.example
```

### Issue: Patterns aren't working as expected

**Solution**: Remember that patterns are evaluated in order. More specific patterns should come before general ones. Also, ensure you're using the correct pattern syntax:

- Use `**/` to match directories at any level
- Use `*` for wildcards within a single directory level
- Patterns are case-sensitive on Linux/macOS

### Issue: Build context is still large

**Solution**: Check for large directories that might not be excluded. Use `docker build --no-cache` to see what's being sent to the daemon. Common culprits include:

- Large media files (images, videos)
- Database dumps
- Backup files
- Development tools and SDKs

### Issue: Docker build is slow despite .dockerignore

**Solution**: Check if you're using `.dockerignore` correctly with Docker BuildKit enabled:

```bash
# Enable BuildKit for better performance
DOCKER_BUILDKIT=1 docker build .
```

### Issue: Patterns with special characters

**Solution**: Escape special characters with backslashes or use quotes:

```dockerignore
# Exclude files with brackets
file\[1\].txt
# or
"file[1].txt"
```

## Template Structure

Each template follows a consistent structure for easy maintenance:

1. **Header**: `# Created by https://dockerignore.com/`
2. **Language/Framework-specific patterns**: Organized by category
3. **Common patterns**: Shared across many templates
4. **Comments**: Explanations for complex patterns

Templates typically exclude common categories like development environment files, build artifacts, runtime files, testing outputs, and security-sensitive files.

## FAQ

### Q: Can I use .gitignore as .dockerignore?

A: While similar, `.gitignore` and `.dockerignore` serve different purposes.
Some patterns may overlap, but `.dockerignore` should be more comprehensive
for build-time exclusions. `.dockerignore` should exclude more files like
build artifacts, dependencies, and temporary files that aren't typically
committed to git but are present during development.

### Q: Should I exclude package-lock.json or yarn.lock?

A: Generally yes, as these are used during development but the actual
dependencies are installed from package.json during `npm install` or
`yarn install` in the Dockerfile. However, if you want reproducible builds,
you might want to include lock files and use `npm ci` instead of `npm install`.

### Q: What about Docker multi-stage builds?

A: `.dockerignore` applies to the initial build context sent to the Docker daemon.
In multi-stage builds, it helps reduce the initial context size. Note that
files copied between stages using `COPY --from` are not affected by `.dockerignore`
since they come from previous build stages, not the build context.

### Q: Can I have multiple .dockerignore files?

A: No, Docker only recognizes one `.dockerignore` file in the build context root.
However, you can combine multiple templates into one file using the `cat` command
as shown in the examples.

### Q: What about .dockerignore in subdirectories?

A: Docker only looks for `.dockerignore` in the build context root (the directory
specified in the `docker build` command). Patterns in `.dockerignore` apply to
all files and directories in the build context, regardless of their depth.

### Q: Can I use environment variables in .dockerignore?

A: No, `.dockerignore` doesn't support environment variable expansion. It's a
static file that's processed before any build arguments or environment variables
are evaluated.

### Q: How do I handle files that need to be excluded in development but included in production?

A: Use conditional logic in your Dockerfile or create separate `.dockerignore`
files for different environments. You can also use build arguments to control
what gets copied.

## Contributing

We welcome contributions! To contribute:

1. Fork the repository
2. Create a new template or improve an existing one
3. Ensure the template includes the header:
   `# Created by https://dockerignore.com/`
4. Submit a pull request

### Template Guidelines

- Follow the existing format and style
- Include comprehensive patterns for the language/framework
- Add comments for clarity when needed
- Test the template with real projects
- Organize patterns logically (group related patterns together)
- Include common build artifacts, dependency directories, and IDE files

### Testing Your Template

Before submitting a template:

1. Test it with a real project of that language/framework
2. Verify it doesn't exclude necessary build files
3. Check for any security-sensitive files that should always be excluded

## Related Projects

- [GitHub gitignore templates](https://github.com/github/gitignore)
- [Docker documentation on .dockerignore](https://docs.docker.com/engine/reference/builder/#dockerignore-file)
- [Docker Best Practices](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) - Official Docker best practices
- [Awesome Docker](https://github.com/veggiemonk/awesome-docker) - A curated list of Docker resources

## License

This project is licensed under the MIT License - see the [LICENSE.md](LICENSE.md)
file for details.

## Acknowledgments

- Community contributions from developers worldwide
- The Docker community for best practices and patterns
- All contributors who have helped improve these templates

## Support

If you find this project helpful, please consider:

- Starring the repository
- Contributing new templates or improvements
- Sharing with your team or community
- Reporting issues or suggesting enhancements
