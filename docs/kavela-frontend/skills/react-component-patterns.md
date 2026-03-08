# React Component Patterns

> Frontend React patterns, component structure, and state management conventions

**Domain:** engineering · **Version:** 1.0.0 · **By:** Yong (Kavela)

**Keywords:** `react` `component` `hook` `state` `frontend` `ui` `typescript` `props`

[View on Kavela Marketplace](https://kavela.ai/marketplace/kavela-frontend)

---
# React Component Patterns

## File Organization
```
components/
  Button/
    Button.tsx        # Component implementation
    Button.test.tsx   # Tests
    index.ts          # Re-export
```

## Component Template
```tsx
interface ButtonProps {
  variant: "primary" | "secondary" | "ghost";
  size?: "sm" | "md" | "lg";
  children: React.ReactNode;
  onClick?: () => void;
  disabled?: boolean;
}

export function Button({
  variant,
  size = "md",
  children,
  onClick,
  disabled = false,
}: ButtonProps) {
  return (
    <button
      className={cn(styles.base, styles[variant], styles[size])}
      onClick={onClick}
      disabled={disabled}
    >
      {children}
    </button>
  );
}
```

## State Management
- Local state: useState for component-specific state
- Shared state: Zustand stores (not Redux)
- Server state: TanStack Query for API data
- Form state: React Hook Form + Zod validation

## Naming Conventions
- PascalCase for components: `UserProfile`
- camelCase for hooks: `useAuth`, `useUserProfile`
- kebab-case for files: `user-profile.tsx`
- UPPER_CASE for constants: `MAX_RETRIES`

## Custom Hooks
Extract logic into hooks when:
- Logic is reused across 2+ components
- Component exceeds ~150 lines
- Logic involves side effects (API calls, subscriptions)
