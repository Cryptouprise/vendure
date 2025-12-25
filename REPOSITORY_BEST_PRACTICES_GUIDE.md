# How Vendure's Repository Practices Can Help Your Projects

This guide analyzes the Vendure repository structure, practices, and conventions that you can apply to your own projects.

## Table of Contents
- [Repository Structure](#repository-structure)
- [Contribution Workflow](#contribution-workflow)
- [Development Practices](#development-practices)
- [Code Quality & Standards](#code-quality--standards)
- [Testing Strategy](#testing-strategy)
- [Documentation](#documentation)
- [Release Management](#release-management)
- [Community & Support](#community--support)

## Repository Structure

### Monorepo Architecture
Vendure uses **Lerna** for managing a monorepo with multiple packages. This approach is beneficial for:
- **Maintaining related packages together**: All packages share the same version and are released together
- **Simplified dependency management**: Internal packages can reference each other easily
- **Consistent tooling**: Shared linting, testing, and build configurations across packages
- **Atomic changes**: Changes affecting multiple packages can be done in a single PR

**How this helps your repos:**
- If you have multiple related projects (e.g., frontend, backend, shared libraries), consider consolidating them into a monorepo
- Use tools like Lerna, Nx, or Turborepo to manage the complexity
- Share configurations (ESLint, Prettier, TypeScript) across packages

### Key Configuration Files

The repository includes comprehensive configuration files:
- **`.editorconfig`**: Ensures consistent coding styles across different editors
- **`.eslintrc.js`**: Code quality and style enforcement
- **`.prettierrc`**: Automatic code formatting
- **`.gitignore`**: Prevents committing build artifacts and dependencies
- **`tsconfig.json`**: TypeScript configuration with strict typing

**How this helps your repos:**
- Set up these configuration files early in your project
- They prevent common issues and maintain consistency across contributors
- Use pre-commit hooks to enforce these standards automatically

## Contribution Workflow

### Clear Contribution Guidelines

Vendure's `CONTRIBUTING.md` provides:
1. **Step-by-step setup instructions**: From forking to running locally
2. **Development workflow**: How to test changes and create PRs
3. **Branch strategy**: Clear guidance on which branch to use (master, minor, major)
4. **Commit message format**: Conventional commits for automatic changelog generation

**How this helps your repos:**
```markdown
## Benefits of a clear CONTRIBUTING.md:
- Reduces onboarding time for new contributors
- Decreases back-and-forth in PRs due to formatting issues
- Creates a professional impression that attracts quality contributors
- Automates parts of the release process
```

### Fork-Based Contribution Model

Vendure uses a fork-based workflow:
- Contributors fork the repository
- Make changes in their fork
- Submit PRs back to the main repository

**How this helps your repos:**
- Protects the main repository from accidental changes
- Allows contributors to experiment freely
- Clear separation between official and contributed code
- Standard GitHub workflow that most developers understand

### Conventional Commits

Format: `type(scope): Message in present tense`

Types include: feat, fix, docs, perf, style, refactor, test, chore

**How this helps your repos:**
- Automatic changelog generation
- Semantic versioning can be derived from commit types
- Clear commit history that's easy to understand
- Better git blame and bisect experiences
- Tools like `standard-version` can automate releases

## Development Practices

### Docker Compose for Dependencies

Vendure includes a `docker-compose.yml` that provides:
- Multiple database options (MySQL, MariaDB, PostgreSQL)
- Elasticsearch for search functionality
- Consistent development environment across machines

**How this helps your repos:**
```yaml
Benefits:
- New developers can start quickly (just run docker-compose up)
- No need to install databases/services locally
- Consistent versions across all developers
- Easy to test against different database versions
- Can be used in CI/CD pipelines
```

### Development Server Package

The `packages/dev-server` provides:
- Pre-configured development environment
- Sample data population scripts
- Hot reloading for rapid development

**How this helps your repos:**
- Create a dedicated development/testing package
- Include seed data for consistent testing
- Reduces friction when onboarding new developers

### Watch Mode for Development

Vendure uses watch mode compilation:
```bash
npm run watch    # In package directory
npm run dev      # In dev-server
```

**How this helps your repos:**
- Faster development iteration
- Immediate feedback on code changes
- Multiple packages can be watched simultaneously
- Combine with hot reloading for best developer experience

## Code Quality & Standards

### TypeScript Throughout

Vendure is 100% TypeScript:
- Type safety across the entire codebase
- Better IDE support and autocomplete
- Catches errors at compile time
- Self-documenting code through types

**How this helps your repos:**
- Migrate JavaScript projects to TypeScript incrementally
- Use strict mode (`"strict": true`) for maximum safety
- Share types between frontend and backend
- Generate API clients from TypeScript definitions

### Linting and Formatting

Vendure uses:
- **ESLint**: Code quality and consistency rules
- **Prettier**: Automatic formatting
- **lint-staged**: Only lint files that changed

**How this helps your repos:**
```json
{
  "husky": {
    "hooks": {
      "pre-commit": "lint-staged"
    }
  },
  "lint-staged": {
    "*.ts": ["eslint --fix", "prettier --write"]
  }
}
```
- Prevents bad code from being committed
- Reduces code review comments about formatting
- Keeps codebase consistent even with many contributors

### Code Generation

Vendure uses `graphql-code-generator`:
- Generates TypeScript types from GraphQL schemas
- Ensures type safety between client and server
- Reduces manual typing work
- Keeps types in sync automatically

**How this helps your repos:**
- Use code generation for repetitive tasks
- Generate types, API clients, database models
- Reduces human error in manual typing
- Tools: graphql-codegen, TypeORM, Prisma, openapi-generator

## Testing Strategy

### Multiple Test Types

1. **Unit Tests**: Co-located with source files (`.spec.ts`)
2. **End-to-End Tests**: Integration tests in `/e2e/` directories
3. **Testing Package**: Dedicated `@vendure/testing` package

**How this helps your repos:**
```
Benefits of this structure:
- Unit tests are easy to find and maintain
- E2E tests validate entire workflows
- Testing utilities are shared and reusable
- Clear separation of test types
```

### Test Commands

```bash
npm run test    # Unit tests
npm run e2e     # E2E tests
```

**How this helps your repos:**
- Provide simple, memorable commands
- Run tests in CI/CD pipelines
- Document test commands in README
- Include test coverage reporting

## Documentation

### Multi-Level Documentation

1. **README.md**: Quick overview and getting started
2. **CONTRIBUTING.md**: Detailed contribution guidelines
3. **Full docs site**: Using Docusaurus (https://docs.vendure.io)
4. **JSDoc comments**: Generated API reference documentation

**How this helps your repos:**
```
Documentation hierarchy:
README.md           → First impression, quick start
CONTRIBUTING.md     → How to contribute
docs/guides/        → Step-by-step tutorials
docs/reference/     → Auto-generated API docs
```

### Auto-Generated Documentation

Vendure generates reference documentation from JSDoc comments:
```typescript
/**
 * @description
 * Sets the value of the new API thing.
 *
 * @since 1.2.0
 */
myNewApi: number;
```

**How this helps your repos:**
- Documentation stays in sync with code
- `@since` tags help users understand when features were added
- Tools: TypeDoc, JSDoc, Docusaurus, VitePress
- Reduces documentation maintenance burden

## Release Management

### Semantic Versioning

Vendure follows SemVer strictly:
- **master branch**: Patch releases (bug fixes)
- **minor branch**: Minor releases (new features)
- **major branch**: Major releases (breaking changes)

**How this helps your repos:**
- Users know what to expect from updates
- Clear communication about breaking changes
- Easier to maintain multiple versions
- Standard that most developers understand

### Lerna for Releases

All packages are released together at the same version:
```bash
npm run publish-release
```

**How this helps your repos:**
- Simplified version management
- No confusion about compatible versions
- Automated changelog generation
- Consistent release process

### Changelog Management

Vendure maintains detailed changelogs:
- `CHANGELOG.md`: Current version changes
- `CHANGELOG_v1.md`, `CHANGELOG_v2.md`: Historical versions
- `CHANGELOG_NEXT.md`: Upcoming changes

**How this helps your repos:**
- Users can see what changed between versions
- Helps with debugging after updates
- Marketing material for new features
- Shows active development

## Community & Support

### Contributor License Agreement (CLA)

Vendure requires contributors to sign a CLA:
- Protects both the project and contributors
- Clarifies intellectual property rights
- Automated through a bot

**How this helps your repos:**
- Important for commercial projects
- Prevents legal issues down the line
- Many enterprises require CLAs for open source
- Tools: CLA Assistant, CLAHub

### Multiple Support Channels

1. **Discord**: Active community chat
2. **GitHub Issues**: Bug reports and feature requests
3. **Documentation**: Comprehensive guides
4. **Twitter**: Updates and announcements

**How this helps your repos:**
- Multiple channels suit different user preferences
- Discord for quick questions and community building
- GitHub for trackable issues and discussions
- Documentation for self-service support

### Issue Templates

Vendure uses GitHub issue templates:
- Bug report template
- Feature request template
- Structured information gathering

**How this helps your repos:**
```markdown
Benefits:
- Gets all necessary information upfront
- Reduces back-and-forth
- Makes triage easier
- Professional appearance
```

## Applying These Practices to Your Repos

### For Small Projects (1-5 contributors)

Start with:
1. ✅ README.md with setup instructions
2. ✅ .gitignore file
3. ✅ Prettier for formatting
4. ✅ Basic CI/CD (GitHub Actions)
5. ✅ Simple CONTRIBUTING.md

### For Medium Projects (5-20 contributors)

Add:
1. ✅ ESLint for code quality
2. ✅ Pre-commit hooks (husky + lint-staged)
3. ✅ Docker Compose for dependencies
4. ✅ Issue templates
5. ✅ Conventional commits
6. ✅ Automated testing in CI

### For Large Projects (20+ contributors)

Include:
1. ✅ Comprehensive CONTRIBUTING.md
2. ✅ Monorepo structure (if multiple packages)
3. ✅ Documentation site (Docusaurus, VitePress)
4. ✅ CLA for contributors
5. ✅ Multiple support channels
6. ✅ Semantic versioning with automated releases
7. ✅ Dedicated testing infrastructure
8. ✅ Code generation tools

## Quick Implementation Checklist

Copy and adapt these practices:

### Week 1: Foundations
- [ ] Add .editorconfig, .prettierrc, .gitignore
- [ ] Create basic README.md with setup instructions
- [ ] Set up ESLint and Prettier
- [ ] Add pre-commit hooks

### Week 2: Contribution Process
- [ ] Write CONTRIBUTING.md
- [ ] Create issue templates
- [ ] Document commit message format
- [ ] Set up GitHub Actions for CI

### Week 3: Development Environment
- [ ] Add docker-compose.yml for dependencies
- [ ] Create development/testing environment
- [ ] Add seed data scripts
- [ ] Document development workflow

### Week 4: Documentation & Release
- [ ] Set up changelog management
- [ ] Implement semantic versioning
- [ ] Add JSDoc comments to code
- [ ] Consider documentation site for larger projects

## Key Takeaways

1. **Start simple, add complexity as needed**: Don't implement everything at once
2. **Consistency is key**: Pick standards and stick to them
3. **Automate what you can**: Pre-commit hooks, CI/CD, releases
4. **Document everything**: Future you will thank present you
5. **Make contributing easy**: Lower barriers attract better contributors
6. **Learn from established projects**: Vendure's practices are battle-tested

## Resources

- [Conventional Commits](https://www.conventionalcommits.org/)
- [Semantic Versioning](https://semver.org/)
- [Lerna Documentation](https://lerna.js.org/)
- [GitHub Actions](https://github.com/features/actions)
- [Docusaurus](https://docusaurus.io/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)

---

## Conclusion

Vendure's repository demonstrates professional open-source project management. You don't need to implement all these practices at once, but selectively adopting them based on your project's size and needs can significantly improve:

- **Code quality**: Through linting, formatting, and TypeScript
- **Contributor experience**: Through clear guidelines and easy setup
- **Development speed**: Through automation and good tooling
- **Project sustainability**: Through documentation and community building

Start with the basics and gradually adopt more practices as your project grows. The investment in good repository practices pays dividends in reduced maintenance burden and increased contributor satisfaction.
