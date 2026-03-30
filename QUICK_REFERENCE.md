# Quick Reference Guide

## Project Structure at a Glance

```
src/app/
├── components/         # Reusable UI components
├── pages/             # Route-level components
├── services/          # Business logic & API calls
├── models/            # TypeScript interfaces
├── constants/         # App-wide constants
└── app.ts            # Root component
```

## Common Tasks

### Adding a New Service

```typescript
// 1. Create service file
import { Injectable } from '@angular/core';

@Injectable({ providedIn: 'root' })
export class MyService {
  // Your logic here
}

// 2. Export from services/index.ts
export * from './my-service.service';

// 3. Inject in component
constructor(private myService: MyService) {}
```

### Using the OpenAI Service

```typescript
// Transcribe audio
const text = await this.openAIService.transcribeAudio(audioBlob);

// Get chat response
const response = await this.openAIService.sendChatMessage(messages);

// Check if configured
if (this.openAIService.isConfigured()) {
  // API key is set
}
```

### Using the Audio Recording Service

```typescript
// Subscribe to state
this.audioService.recordingState$
  .pipe(takeUntil(this.destroy$))
  .subscribe(state => {
    this.isRecording = state.isRecording;
    this.isProcessing = state.isProcessing;
  });

// Start recording
await this.audioService.startRecording();

// Stop recording
const audioBlob = await this.audioService.stopRecording();

// Set processing state
this.audioService.setProcessing(true);
```

### Using the Error Handler Service

```typescript
try {
  // Your code
} catch (error) {
  // Log error
  this.errorHandler.logError('Context', error);
  
  // Get user-friendly message
  const message = this.errorHandler.handleApiError(error);
  // or
  const message = this.errorHandler.handleRecordingError(error);
}
```

## Import Paths

```typescript
// Models
import { ChatMessage, AudioRecordingState } from '../../models';

// Services
import { OpenAIService, AudioRecordingService } from '../../services';

// Constants
import { APP_CONSTANTS, ROUTES } from '../../constants';

// Environment
import { environment } from '../../environments/environment';
```

## Constants Reference

```typescript
// App info
APP_CONSTANTS.APP_NAME
APP_CONSTANTS.APP_VERSION

// Messages
APP_CONSTANTS.THINKING_MESSAGE
APP_CONSTANTS.DEFAULT_SYSTEM_PROMPT

// Error messages
APP_CONSTANTS.ERROR_MESSAGES.NO_MICROPHONE
APP_CONSTANTS.ERROR_MESSAGES.API_KEY_MISSING
APP_CONSTANTS.ERROR_MESSAGES.RECORDING_IN_PROGRESS

// Audio config
APP_CONSTANTS.AUDIO.DEFAULT_MIME_TYPE
APP_CONSTANTS.AUDIO.FILE_NAME

// Routes
ROUTES.HOME
ROUTES.SPEECH_TO_TEXT
```

## Component Lifecycle Pattern

```typescript
export class MyComponent implements OnInit, OnDestroy {
  // Use signals for reactive state
  data = signal<Data | null>(null);
  private destroy$ = new Subject<void>();

  constructor(private myService: MyService) {}

  ngOnInit(): void {
    // Subscribe with automatic cleanup
    this.myService.data$
      .pipe(takeUntil(this.destroy$))
      .subscribe(data => {
        // Update signal - triggers automatic UI updates
        this.data.set(data);
      });
  }

  ngOnDestroy(): void {
    this.destroy$.next();
    this.destroy$.complete();
  }
}
```

## Environment Configuration

```typescript
// Access environment variables
environment.production
environment.openai.apiKey
environment.openai.chatModel
environment.openai.whisperModel
environment.openai.maxTokens
environment.openai.temperature
```

## Error Handling Pattern

```typescript
async myMethod(): Promise<void> {
  try {
    // Your logic
    const result = await this.service.doSomething();
  } catch (error) {
    // Log for debugging
    this.errorHandler.logError('MyMethod', error);
    
    // Get user message
    const message = this.errorHandler.handleApiError(error);
    
    // Show to user
    this.showError(message);
  } finally {
    // Cleanup
    this.isLoading = false;
  }
}
```

## Service Method Documentation Pattern

```typescript
/**
 * Brief description of what the method does
 * @param paramName Description of parameter
 * @returns Description of return value
 * @throws Description of possible errors
 */
async myMethod(paramName: Type): Promise<ReturnType> {
  // Implementation
}
```

## Common Angular Patterns

### Async Pipe in Template
```html
<div *ngIf="data$ | async as data">
  {{ data.property }}
</div>
```

### NgFor with TrackBy
```html
<div *ngFor="let item of items; trackBy: trackById">
  {{ item.name }}
</div>
```

```typescript
trackById(index: number, item: any): number {
  return item.id;
}
```

### Conditional Classes
```html
<button 
  [class.active]="isActive"
  [class.disabled]="isDisabled">
  Click me
</button>
```

## Signals (Modern Angular 21 Reactivity)

```typescript
// Define signals
isRecording = signal(false);
count = signal(0);

// Update signals
this.isRecording.set(true);
this.count.update(v => v + 1);

// Read signals in TypeScript
if (this.isRecording()) { ... }

// Read signals in templates
{{ isRecording() }}
[disabled]="isRecording()"
```

**Note:** With Signals, you don't need manual zone management - Angular handles change detection automatically!

## Useful Commands

```bash
# Development
npm start                    # Start dev server with auto-open
npm run watch               # Build and watch for changes

# Build
npm run build               # Development build
npm run build:prod          # Production build

# Quality
npm run lint                # Run linter
npm test                    # Run tests

# Environment Setup
cp src/environments/environment.template.ts src/environments/environment.ts
```

## File Naming Conventions

```
component-name.ts           # Component class
component-name.html         # Component template
component-name.scss         # Component styles
my-service.service.ts       # Service
my.model.ts                # Model/Interface
app.constants.ts           # Constants
index.ts                   # Barrel export
```

## Git Workflow

```bash
# Start new feature
git checkout -b feature/my-feature

# Make changes, then commit
git add .
git commit -m "feat: add my feature"

# Push and create PR
git push origin feature/my-feature
```

## Debugging Tips

### Browser Console
```typescript
console.log('Value:', value);           // Simple logging
console.error('Error:', error);         // Error logging
console.table(arrayOfObjects);          // Table view
```

### Angular DevTools
- Install Angular DevTools extension
- Inspect component state
- View component tree
- Profile performance

### Network Debugging
- Open browser DevTools → Network tab
- Check API calls
- Verify request/response
- Check status codes

## Performance Tips

1. **Use trackBy in ngFor**
2. **Unsubscribe from observables**
3. **Use OnPush change detection** (when ready)
4. **Lazy load routes** (for larger apps)
5. **Minimize bundle size**

## Security Checklist

- [ ] API keys in environment files (not committed)
- [ ] Environment files in .gitignore
- [ ] No sensitive data in error messages
- [ ] Input validation on user data
- [ ] HTTPS for API calls

## Resources

- [Angular Docs](https://angular.io/docs)
- [RxJS Docs](https://rxjs.dev/)
- [TypeScript Handbook](https://www.typescriptlang.org/docs/)
- [OpenAI API Docs](https://platform.openai.com/docs)

---

**Need more help?** Check the full [README.md](./README.md) or [SETUP.md](./SETUP.md)

