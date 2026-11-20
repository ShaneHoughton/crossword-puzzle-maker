---
name: js-ts-react-standards
description: Standards for writing and reviewing JavaScript, TypeScript, and React code in this project.
---

# JavaScript, TypeScript, and React standards

## JavaScript and TypeScript

### Names

- Name module constants in `UPPER_SNAKE_CASE`.
- Name functions and local constants in `camelCase`.
- Prefix boolean variables and functions with question-style terms such as `is`, `has`, `can`, or `should`.
- Prefix type aliases with `T` and interfaces with `I`, using PascalCase.
- Use plural nouns for arrays and collections.
- Use `Id` for a single identifier and `Ids` for a collection.

### Imports

- Use ES module imports; do not use CommonJS `require`.
- Prefer named imports and exports. Avoid default exports.
- Namespace imports are allowed.
- Use side-effect-only imports only for setup, registration, or styles.

### Async

- Use `async`/`await` instead of chaining `.then()`, `.catch()`, or `.finally()`.
- Awaited Promise combinators such as `Promise.all()` are allowed.
- Use `try`/`catch` when handling or translating a rejection. Let errors propagate when no local handling is needed.

### Comments

- Avoid line comments. Prefer self-documenting code and focused refactoring.
- Do not add TODO or FIXME comments; track follow-up work in an issue instead.
- Avoid workarounds. If one must remain, track its removal or replacement in an issue.

### Shared code

- Keep feature-specific constants, types, and utilities within their feature.
- Put shared utilities, constants, and types in `src/utils/`, `src/constants/`, and `src/types/`.
- Extract code for a second real consumer or a clearly shared domain concept.

## React

### Components and props

- Write components as functions. Use PascalCase component names and filenames, matching each filename to its component.
- Put each component in its own file and use named exports.
- Define a component's props interface in the component file, named `I<ComponentName>Props`.
- Accept `children` only for intentional composition, typed as `React.ReactNode`.
- Keep components focused; extract reusable or substantial UI.
- Prefix callback props with `on` and internal event handlers with `handle`.

```tsx
interface IStatusMessageProps {
  message: string;
  onRetry: () => void;
}

export function StatusMessage({ message, onRetry }: IStatusMessageProps) {
  return (
    <main>
      <p>{message}</p>
      <button type="button" onClick={onRetry}>
        Retry
      </button>
    </main>
  );
}
```

### Custom hooks

- Name hooks with `use` followed by descriptive camelCase, such as `useRecordSelection`.
- Put each hook in a file named after it, such as `useRecordSelection.ts`.
- Do not define components in hook files.
- Extract a hook when components share stateful behavior or a component has cohesive stateful logic worth isolating.
- Keep each hook focused; related state, effects, derived values, and handlers may stay together.
- For files over 100 lines that manage state without a hook, consider extracting cohesive stateful behavior.

### Organization and reuse

- Keep feature-specific components in feature folders, such as `src/features/records/components/`.
- Put cross-feature components in `src/components/<category>/`, outside feature folders.
- Keep simple components in one file under their category. Create component folders when related files or subcomponents are needed.

### Styling

- Use Tailwind CSS or UI components/libraries already present in the project.
- Do not add custom CSS rules, CSS Modules, inline styles through the `style` property, Styled Components, Emotion, or Sass/SCSS.
- Prefer existing UI components for standard controls.
- Follow surrounding responsive layout patterns.
- Give interactive elements clear hover, keyboard `focus-visible`, and disabled states.

### State

- Use `useState` or `useReducer` for local/transient state owned by one component or a single custom-hook invocation.
- Keep custom-hook state private to each invocation. Use Redux when state must be shared across components or distant parts of the app.
- Put API calls in services, async orchestration in thunks, and results/request status in slices.
- Track Redux request status and errors, and render loading, success, and error states.
- Compute derived values in selectors or components; do not store duplicate derived state.
- Prefix Redux selectors with `select`, such as `selectRecordById`.
- Name state-changing actions with clear verbs: `set`, `add`, `update`, `remove`, or `toggle`.

### Rendering, composition, and accessibility

- Render loading, error, and other UI states explicitly.
- Use `useEffect` for side effects, include required dependencies, and clean up subscriptions and listeners.
- Use conditional classes for component state.
- Prefer semantic HTML and native keyboard behavior. Add ARIA only when native semantics are insufficient.
- Give every form field a visible label.
- Use suitable live regions for dynamic updates; reserve alert announcements for urgent errors.

### Testing

- Name React test files after the component, using `.test.tsx`, such as `StatusMessage.test.tsx`.
