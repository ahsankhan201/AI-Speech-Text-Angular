# Contributing to AI Speech Angular

Thank you for your interest in contributing to AI Speech Angular! This document provides guidelines and instructions for contributing to this project.

## Code of Conduct

- Be respectful and inclusive
- Focus on constructive feedback
- Help others learn and grow

## Getting Started

1. Fork the repository
2. Clone your fork: `git clone https://github.com/your-username/AI-Speech-Angular.git`
3. Install dependencies: `npm install`
4. Create a branch: `git checkout -b feature/your-feature-name`

## Development Workflow

### Branch Naming Convention

- `feature/` - New features
- `fix/` - Bug fixes
- `refactor/` - Code refactoring
- `docs/` - Documentation updates
- `test/` - Test additions or updates

Examples:
- `feature/add-text-to-speech`
- `fix/microphone-permission-issue`
- `refactor/audio-service-optimization`

### Coding Standards

#### TypeScript/Angular

- Use TypeScript strict mode
- Follow Angular style guide
- Use meaningful variable and function names
- Add JSDoc comments for public methods
- Keep functions small and focused (< 50 lines)
- Prefer composition over inheritance

#### File Organization

```
src/app/
├── components/     # Reusable UI components
├── pages/         # Route-level page components
├── services/      # Business logic and API calls
├── models/        # TypeScript interfaces and types
├── constants/     # Application constants
└── utils/         # Utility functions (if needed)
```

#### Naming Conventions

- **Components**: PascalCase with suffix (e.g., `HomePage`, `SidenavComponent`)
- **Services**: PascalCase with `Service` suffix (e.g., `OpenAIService`)
- **Interfaces**: PascalCase (e.g., `ChatMessage`, `AudioConfig`)
- **Constants**: UPPER_SNAKE_CASE (e.g., `API_KEY`, `MAX_RETRIES`)
- **Files**: kebab-case (e.g., `audio-recording.service.ts`)

#### Code Style

```typescript
// ✅ Good
export class AudioRecordingService {
  /**
   * Starts audio recording
   * @returns Promise that resolves when recording starts
   * @throws Error if recording is not supported
   */
  async startRecording(): Promise<void> {
    // Implementation
  }
}

// ❌ Bad
export class AudioRecordingService {
  async startRecording(): Promise<void> {
    // No documentation
    // Implementation
  }
}
```

### Service Layer Guidelines

- Keep components thin (presentation logic only)
- Put business logic in services
- Use dependency injection
- Return Observables or Promises
- Handle errors in services, not components
- Use the ErrorHandlerService for consistent error messages

### Testing

Before submitting a PR:

```bash
# Run linter
npm run lint

# Run tests (if available)
npm test

# Build the project
npm run build
```

### Commit Messages

Follow conventional commit format:

```
<type>(<scope>): <subject>

<body>

<footer>
```

**Types:**
- `feat`: New feature
- `fix`: Bug fix
- `docs`: Documentation only
- `style`: Code style changes (formatting, etc.)
- `refactor`: Code refactoring
- `test`: Adding or updating tests
- `chore`: Maintenance tasks

**Examples:**

```
feat(speech): add voice activity detection

Implement voice activity detection to automatically start/stop recording
when the user speaks or pauses.

Closes #123
```

```
fix(audio): resolve microphone permission handling

Fixed an issue where permission denial wasn't properly handled,
causing the app to hang.
```

## Pull Request Process

1. **Update Documentation**: If you change functionality, update the README
2. **Test Thoroughly**: Test your changes in different browsers
3. **Lint Your Code**: Ensure no linting errors
4. **Write Clear PR Description**:
   - What changes were made
   - Why they were needed
   - How to test them

### PR Template

```markdown
## Description
Brief description of changes

## Type of Change
- [ ] Bug fix
- [ ] New feature
- [ ] Breaking change
- [ ] Documentation update

## Testing
How to test these changes

## Screenshots (if applicable)

## Checklist
- [ ] Code follows project style guidelines
- [ ] Self-review completed
- [ ] Documentation updated
- [ ] No console errors or warnings
- [ ] Tested in multiple browsers
```

## Project Architecture

### Service Pattern

```typescript
// Services handle business logic and external dependencies
@Injectable({ providedIn: 'root' })
export class MyService {
  constructor(private http: HttpClient) {}
  
  async fetchData(): Promise<Data> {
    // API calls, data processing
  }
}
```

### Component Pattern

```typescript
// Components handle presentation and user interaction
@Component({ /* ... */ })
export class MyComponent implements OnInit, OnDestroy {
  private destroy$ = new Subject<void>();
  
  constructor(private myService: MyService) {}
  
  ngOnInit(): void {
    // Subscribe to observables with takeUntil for cleanup
    this.myService.data$
      .pipe(takeUntil(this.destroy$))
      .subscribe(/* ... */);
  }
  
  ngOnDestroy(): void {
    this.destroy$.next();
    this.destroy$.complete();
  }
}
```

## Adding New Features

### Checklist for New Features

- [ ] Create feature branch
- [ ] Implement service layer (if needed)
- [ ] Create/update models/interfaces
- [ ] Implement component
- [ ] Add constants (no magic strings)
- [ ] Update navigation (if needed)
- [ ] Add error handling
- [ ] Update documentation
- [ ] Test in multiple browsers
- [ ] Create pull request

## Common Tasks

### Adding a New Page

1. Create page component: `src/app/pages/my-page/`
2. Add route in `src/app/app.routes.ts`
3. Add navigation item in `SidenavComponent`
4. Update constants if needed

### Adding a New Service

1. Create service: `src/app/services/my-service.service.ts`
2. Add `@Injectable({ providedIn: 'root' })`
3. Export from `src/app/services/index.ts`
4. Add documentation
5. Handle errors appropriately

### Adding Constants

1. Add to `src/app/constants/app.constants.ts`
2. Use throughout the app instead of magic strings/numbers
3. Export from `src/app/constants/index.ts`

## Security Guidelines

- **Never commit API keys** - Always use environment files
- **Validate user input** - Sanitize and validate all user inputs
- **Handle errors gracefully** - Don't expose sensitive error details to users
- **Use HTTPS** - Always use secure connections for API calls
- **Keep dependencies updated** - Regularly update npm packages

## Questions?

If you have questions:
1. Check existing issues
2. Read the documentation
3. Create a new issue with your question

## Recognition

Contributors will be recognized in the project README. Thank you for helping make this project better!

